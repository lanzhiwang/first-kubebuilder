# Markers for Config/Code Generation



```
// +kubebuilder:skipversion
// +kubebuilder:storageversion
// +kubebuilder:subresource:scale
// +kubebuilder:unservedversion
// +kubebuilder:skip
// +versionName

// +kubebuilder:object:generate=true
// +groupName=rabbitmq.com

// +kubebuilder:object:root=true

// +kubebuilder:subresource:status
// +kubebuilder:printcolumn:name="Age",type="date",JSONPath=".metadata.creationTimestamp"
// +kubebuilder:printcolumn:name="Status",type="string",JSONPath=".status.clusterStatus"
// +kubebuilder:resource:shortName={"rmq"}

// +optional

// +kubebuilder:validation:Optional
// +kubebuilder:validation:Minimum:=0
// +kubebuilder:validation:Pattern:="^\\w+$"
// +kubebuilder:validation:MaxLength=100
// +kubebuilder:validation:MaxItems:=100
// +kubebuilder:validation:Enum=ClusterIP;LoadBalancer;NodePort
// +kubebuilder:validation:Enum={"crackers, Gromit, we forgot the crackers!","not even wensleydale?"}
// +kubebuilder:validation:ExclusiveMaximum=false
// +kubebuilder:validation:Format="date-time"
// +kubebuilder:validation:Maximum=42
// +kubebuilder:validation:Type=string
// +kubebuilder:validation:EmbeddedResource
// +kubebuilder:validation:ExclusiveMinimum
// +kubebuilder:validation:MaxProperties
// +kubebuilder:validation:MinItems
// +kubebuilder:validation:MinLength
// +kubebuilder:validation:MinProperties
// +kubebuilder:validation:MultipleOf
// +kubebuilder:validation:UniqueItems

// +kubebuilder:default:=1
// +kubebuilder:default:="rabbitmq:3.8.12-management"
// +kubebuilder:default:={type: "ClusterIP"}
// +kubebuilder:default:={storage: "10Gi"}
// +kubebuilder:default:={limits: {cpu: "2000m", memory: "2Gi"}, requests: {cpu: "1000m", memory: "2Gi"}}
// +kubebuilder:default:="10Gi"
// +kubebuilder:default:="ClusterIP"

// +kubebuilder:rbac:groups="",resources=pods/exec,verbs=create
// +kubebuilder:rbac:groups="",resources=pods,verbs=update;get;list;watch
// +kubebuilder:rbac:groups="",resources=services,verbs=get;list;watch;create;update
// +kubebuilder:rbac:groups="",resources=endpoints,verbs=get;watch;list
// +kubebuilder:rbac:groups=apps,resources=statefulsets,verbs=get;list;watch;create;update;delete
// +kubebuilder:rbac:groups="",resources=configmaps,verbs=get;list;watch;create;update
// +kubebuilder:rbac:groups="",resources=secrets,verbs=get;list;watch;create;update
// +kubebuilder:rbac:groups=rabbitmq.com,resources=rabbitmqclusters,verbs=get;list;watch;create;update
// +kubebuilder:rbac:groups=rabbitmq.com,resources=rabbitmqclusters/status,verbs=get;update
// +kubebuilder:rbac:groups=rabbitmq.com,resources=rabbitmqclusters/finalizers,verbs=update
// +kubebuilder:rbac:groups="",resources=events,verbs=get;create;patch
// +kubebuilder:rbac:groups="",resources=serviceaccounts,verbs=get;list;watch;create;update
// +kubebuilder:rbac:groups="",resources=persistentvolumeclaims,verbs=get;list;watch;create;update
// +kubebuilder:rbac:groups="rbac.authorization.k8s.io",resources=roles,verbs=get;list;watch;create;update

```

