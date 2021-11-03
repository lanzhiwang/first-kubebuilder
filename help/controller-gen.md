# controller-gen CLI

KubeBuilder makes use of a tool called [controller-gen](https://sigs.k8s.io/controller-tools/cmd/controller-gen) for generating utility code and Kubernetes YAML. This code and config generation is controlled by the presence of special [“marker comments”](https://book.kubebuilder.io/reference/markers.html) in Go code.  KubeBuilder 使用名为 controller-gen 的工具来生成实用程序代码和 Kubernetes YAML。 此代码和配置生成由 Go 代码中特殊“标记注释”的存在控制。

controller-gen is built out of different **generators** (which specify what to generate) and **output rules** (which specify how and where to write the results).  controller-gen 由不同的“生成器”（指定生成什么）和“输出规则”（指定如何以及在何处写入结果）构建而成。

Both are configured through command line options specified in [marker format](https://book.kubebuilder.io/reference/markers.html).  两者都是通过以标记格式指定的命令行选项配置的。

For instance,

```bash
$ controller-gen \
rbac:roleName=controller-perms \
crd:trivialVersions=true \
paths=./... \
output:crd:artifacts:config=config/crd/bases

$ controller-gen \
crd:trivialVersions=true, preserveUnknownFields=false, crdVersions=v1 \
rbac:roleName=operator-role \
paths="./api/...;./controllers/..." \
output:crd:artifacts:config=config/crd/bases
```

generates CRDs and RBAC, and specifically stores the generated CRD YAML in `config/crd/bases`. For the RBAC, it uses the default output rules (`config/rbac`). It considers every package in the current directory tree (as per the normal rules of the go `...` wildcard).  生成 CRD 和 RBAC，并将生成的 CRD YAML 专门存放在 config/crd/bases 中。 对于 RBAC，它使用默认输出规则 (config/rbac)。 它考虑当前目录树中的每个包（按照 go ... 通配符的正常规则）。

## Generators

Each different generator is configured through a CLI option. Multiple generators may be used in a single invocation of `controller-gen`.  每个不同的生成器都通过 CLI 选项进行配置。 可以在单次调用控制器生成器中使用多个生成器。

Show Detailed Argument Help

```bash
+webhook package

	generates (partial部分的) {Mutating变异,Validating验证}WebhookConfiguration objects.

##########################################################

+schemapatch package

	patches existing CRDs with new schemata.

	For legacy (v1beta1) single-version CRDs, it will simply replace the global schema.  对于旧版 (v1beta1) 单版本 CRD，它将简单地替换全局架构。

	For legacy (v1beta1) multi-version CRDs, and any v1 CRDs, it will replace schemata of existing versions and *clear the schema* from any versions not specified in the Go code.  It will *not* add new versions, or remove old ones.  对于旧版 (v1beta1) 多版本 CRD 和任何 v1 CRD，它将替换现有版本的架构，并从 Go 代码中未指定的任何版本中*清除架构*。 它不会*添加新版本，或删除旧版本。

	For legacy multi-version CRDs with identical schemata, it will take care of lifting the per-version schema up to the global schema.  对于具有相同模式的旧版多版本 CRD，它将负责将每个版本模式提升到全局模式。

	It will generate output for each "CRD Version" (API version of the CRD type itself) , e.g. apiextensions/v1beta1 and apiextensions/v1) available.  它将为每个“CRD 版本”（CRD 类型本身的 API 版本）生成输出，例如 apiextensions/v1beta1 和 apiextensions/v1) 可用。

	manifests=<string>
		contains the CustomResourceDefinition YAML files.
	[maxDescLen=<int>]
		specifies the maximum description length for fields in CRD's OpenAPI schema.  指定 CRD 的 OpenAPI 架构中字段的最大描述长度。
		0 indicates drop the description for all fields completely. n indicates limit the description to at most n characters and truncate the description to closest sentence boundary if it exceeds n characters.  0 表示完全删除所有字段的描述。 n 表示将描述限制为最多 n 个字符，如果超过 n 个字符，则将描述截断到最近的句子边界。

##########################################################

+rbac package
	generates ClusterRole objects.

	roleName=<string>
		sets the name of the generated ClusterRole.

##########################################################

+object package
	generates code containing DeepCopy, DeepCopyInto, and DeepCopyObject method implementations.

	[headerFile=<string>]
		specifies the header text (e.g. license) to prepend to generated files.
	[year=<string>]
		specifies the year to substitute for "YEAR" in the header file.

##########################################################

+crd package
	generates CustomResourceDefinition objects.

	[allowDangerousTypes=<bool>]
		allows types which are usually omitted from CRD generation because they are not recommended.  允许通常从 CRD 生成中省略的类型，因为它们是不推荐的。
		Currently the following additional types are allowed when this is true: float32 float64
		Left unspecified, the default is false  未指定，默认为 false

	[crdVersions=<[]string>]
		specifies the target API versions of the CRD type itself to generate. Defaults to v1.
		The first version listed will be assumed to be the "default" version and will not get a version suffix in the output filename.  列出的第一个版本将被假定为“默认”版本，并且不会在输出文件名中获得版本后缀。
		You'll need to use "v1" to get support for features like defaulting, along with an API server that supports it (Kubernetes 1.16+).  您需要使用“v1”来获得对默认功能等功能的支持，以及支持它的 API 服务器（Kubernetes 1.16+）。

	[maxDescLen=<int>]
		specifies the maximum description length for fields in CRD's OpenAPI schema.
		0 indicates drop the description for all fields completely. n indicates limit the description to at most n characters and truncate the description to closest sentence boundary if it exceeds n characters.  0 表示完全删除所有字段的描述。 n 表示将描述限制为最多 n 个字符，如果超过 n 个字符，则将描述截断到最近的句子边界。

	[preserveUnknownFields=<bool>]
		indicates whether or not we should turn off pruning.  指示我们是否应该关闭修剪。
		Left unspecified, it'll default to true when only a v1beta1 CRD is generated (to preserve compatibility with older versions of this tool), or false otherwise.  如果未指定，则仅在生成 v1beta1 CRD 时默认为 true（以保持与此工具旧版本的兼容性），否则为 false。
		It's required to be false for v1 CRDs.

	[trivialVersions=<bool>]
		indicates that we should produce a single-version CRD.  表示我们应该生成一个单一版本的 CRD。
		Single "trivial-version" CRDs are compatible with older (pre 1.13) Kubernetes API servers.  The storage version's schema will be used as the CRD's schema.
		Only works with the v1beta1 CRD version.

```

## Output Rules

Output rules configure how a given generator outputs its results. There is always one global “fallback” output rule (specified as `output:<rule>`), plus per-generator overrides (specified as `output:<generator>:<rule>`).  输出规则配置给定生成器如何输出其结果。 总是有一个全局“后备”输出规则（指定为 `output:<rule>`），加上每个生成器的覆盖（指定为 `output:<generator>:<rule>`）。

### Default Rules

When no fallback rule is specified manually, a set of default per-generator rules are used which result in YAML going to `config/<generator>`, and code staying where it belongs.  当没有手动指定回退规则时，会使用一组默认的每生成器规则，这会导致 YAML 转到 `config/<generator>`，并且代码停留在它所属的位置。

The default rules are equivalent to `output:<generator>:artifacts:config=config/<generator>` for each generator.

When a “fallback” rule is specified, that’ll be used instead of the default rules.

For example, if you specify `crd rbac:roleName=controller-perms output:crd:stdout`, you’ll get CRDs on standard out, and rbac in a file in `config/rbac`. If you were to add in a global rule instead, like `crd rbac:roleName=controller-perms output:crd:stdout output:none`, you’d get CRDs to standard out, and everything else to /dev/null, because we’ve explicitly specified a fallback.  例如，如果您指定 `crd rbac:roleName=controller-perms output:crd:stdout`，您将在标准输出上获得 CRD，在 config/rbac 的文件中获得 rbac。 如果您要添加全局规则，例如 `crd rbac:roleName=controller-perms output:crd:stdout output:none`，您将获得 CRD 为标准输出，而其他所有内容为 `/dev/null`，因为我们 已经明确指定了后备。

For brevity, the per-generator output rules (`output:<generator>:<rule>`) are omitted below. They are equivalent to the global fallback options listed here.  为简洁起见，下面省略了每个生成器的输出规则 (`output:<generator>:<rule>`)。 它们等效于此处列出的全局回退选项。

Show Detailed Argument Help

```bash
output rules (optionally as output:<generator>:...)

+output:artifacts package
	outputs artifacts to different locations, depending on whether they're package-associated or not.  将工件输出到不同的位置，具体取决于它们是否与包相关联。
	Non-package associated artifacts are output to the Config directory, while package-associated ones are output to their package's source files' directory, unless an alternate path is specified in Code.

	[code=<string>]
		overrides the directory in which to write new code (defaults to where the existing code lives).
	config=<string>
		points to the directory to which to write configuration.

+output:dir package
	=<string>  outputs each artifact to the given directory, regardless of if it's package-associated or not.

+output:none package
	skips outputting anything.

+output:stdout package
	outputs everything to standard-out, with no separation.
	Generally useful for single-artifact outputs.

```

## Other Options

Show Detailed Argument Help

```bash
+paths package
	=<[]string>  represents paths and go-style path patterns to use as package roots.
```

