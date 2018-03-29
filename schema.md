# Openshift对象定义

<!-- TOC depthFrom:2 depthTo:4 withLinks:1 updateOnSave:1 orderedList:0 -->

- [说明](#说明)
- [Route](#route)
- [RouteSpec](#routespec)
- [RouteTargetReference](#routetargetreference)
- [RoutePort](#routeport)
- [TLSConfig](#tlsconfig)
- [ObjectMeta](#objectmeta)
- [ObjectReference](#objectreference)
- [Service](#service)
- [ServiceSpec](#servicespec)
- [ServicePort](#serviceport)
- [DeploymentConfig](#deploymentconfig)
- [DeploymentConfigSpec](#deploymentconfigspec)
- [DeploymentStrategy](#deploymentstrategy)
- [DeploymentTriggerPolicy](#deploymenttriggerpolicy)
- [DeploymentTriggerImageChangeParams](#deploymenttriggerimagechangeparams)
- [PodTemplateSpec](#podtemplatespec)
- [PodSpec](#podspec)
- [PodSecurityContext](#podsecuritycontext)
- [LocalObjectReference](#localobjectreference)
- [Affinity](#affinity)
- [Toleration](#toleration)
- [CustomDeploymentStrategyParams](#customdeploymentstrategyparams)
- [RecreateDeploymentStrategyParams](#recreatedeploymentstrategyparams)
- [ResourceRequirements](#resourcerequirements)
- [EnvVar](#envvar)
- [BuildConfig](#buildconfig)

<!-- /TOC -->

## 说明

本文使用表格的形式，列明对象定义的各个属性，**只会列出需要我们设置的对象属性，而忽略那些服务器生成的对象属性，如selfLink等**。

对象类型有如下几种
- String 字符串类型
- Number 数字类型
- Boolean 布尔值类型
- Object 纯对象类型，通常用于存储键值对
- 对象类型，该对象定义在本文的其他位置
- 数组类型，若类型被[]包围，代表这是数组

推荐使用YAML来编写一个对象的定义，请看[这里](http://ansible-tran.readthedocs.io/en/latest/docs/YAMLSyntax.html)，了解YAML的语法。

本文参考[Openshift官方REST API文档](https://docs.openshift.org/latest/rest_api/index.html)以及Openshift的Swagger API文档，使用Swagger Codegen生成的[对象定义文件](https://github.com/packagewjx/swagger-openshift-client/tree/master/src/model)编辑而成。翻译工作困难且耗时，部分过于复杂的定义不再翻译。如果发现任何错误，或是一些改进，或是帮助翻译，请尽管[提交Issue](https://github.com/packagewjx/openshift-api-object-schema/issues)到本仓库，或是提交PR。谢谢大家！

## Route

|属性名|类型|说明|
|-|-|-|
|kind   |String   |值为Route   |
|apiVersion   |String   |值为v1   |
|metadata   |[ObjectMeta](#objectmeta)   |元数据   |
|spec   |[RouteSpec](#routespec)   |路由的定义   |

## RouteSpec

|属性名|类型|说明|
|-|-|-|
|host   |String   |主机名。是一个DNS域名或是一个代号，指向本服务。这是可选的。如果没有指定，这会被系统自动生成。必须符合DNS952子域名的规范。   |
|path   |String   |路径。路由器监听的地址，来到本地址的请求会发送给本路由的服务。可选属性。   |
|to   |[RouteTargetReference](#routetargetreference)   |本路由所指向的主要的后端服务。只有Service类型能够使用，且默认就是Service类型。如果权重被设为0，则没有请求会发送到该服务。   |
|alternateBackends   |[[RouteTargetReference](#routetargetreference)]   |额外的后端服务。本路由允许使用最多3个额外的后端服务。只有Service类型可以设置。使用RouteTargetReference对西哪个的权重属性来指定路由的偏好。   |
|port   |[RoutePort](#routeport)   |如果指定了这个端口，将会被路由器使用。多数的路由默认会使用服务暴露的所有终端，设置本属性来指定路由使用的端口。   |
|tls   |[TLSConfig](#tlsconfig)   |指定本路由使用的证书和termination代理。   |
|wildcardPolicy   |String   |通配符策略。有效值为Subdomain或者None   |

## RouteTargetReference

|属性名|类型|说明|
|-|-|-|
|kind   |String   |目前只允许使用Service。   |
|name   |String   |指定本路由使用的服务的名字。   |
|weight   |Number    |范围介于0到256之间，默认是1。本属性指定了相对于其他目标引用的权重，0代表不将请求发送给本引用的后端服务。   |

## RoutePort

|属性名|类型|说明|
|-|-|-|
|targetPort  |Strng   |本路由指向的由服务选择的容器上的目标端口，作为一个命名端口，用在查找目标终端的端口列表。必填属性。   |

## TLSConfig

|属性名|类型|说明|
|-|-|-|
|termination   |String   |指定termination代理的类型   |
|certificate   |String   |提供证书的内容   |
|key   |String   |提供证书密钥的内容   |
|caCertificate   |String   |证书机构的证书内容   |
|destinationCACertificate   |String   |提供终点的证书机构的证书内容。当使用重新加密的termination代理时，应提供这个文件，使得路由能够用它来进行证书验证和健康检查。如果没有提供，路由将会使用自身的目的地证书机构的证书，使用较短的服务名(service.namespace.svc)进行主机名的验证，允许基础软件生成的证书能够被自动的验证。   |
|insecureEdgeTerminationPolicy   |String   |指定到路由的不安全连接的路由行为，每一个路由都会有暴露不同端口的策略，通常是80端口。有三个值：<br/>Allow：允许使用不安全的连接来使用本服务。<br/>Disable：不允许使用不安全的连接。<br/>Redirect：重定向到安全的连接。   |

## ObjectMeta

|属性名|类型|说明|
|-|-|-|
|name   |String   |对象名。小写字母或数字开头，后接小写字母、数字或连字符“-”。本名称在一个项目（名称空间）中是唯一的，创建时必须设置本属性，并且不能更新名称。   |
|generateName   |String   |对象生成名。这是一个前缀，只有当对象名不存在时，服务器才会使用本前缀自动产生一个对象名给该对象，   |
|namespace   |String   |定义对象名的空间，使得一个对象名在该空间唯一。这与Openshift的项目是同一个。默认空间为“default”。不是所有的对象都有名称空间，并且无名称空间的对象这一属性为空。无法更新本属性。   |
|labels   |Object   |标签。用于组织和分类对象的属性，使用键值对的方式设置，常用于复制控制器、服务、路由等的选择器的依据。   |
|annotations   |Object   |注解。用于给一些外部插件或程序使用的额外的元数据，当修改本对象时，应该保证注解不改变。   |

## ObjectReference

|属性名|类型|说明|
|-|-|-|
|kind   |String   |被引用对象的类型   |
|namespace   |String   |被引用对象所在的名称空间   |
|name   |String   |被引用对象的名称   |
|uid   |String   |被引用对象的UID   |
|apiVersion   |String   |被引用对象的apiVersion   |
|resouceVersion   |String   |被引用对象的版本号   |
|fieldPath   |String   |如果引用的是对象的一部分而不是整个对象，本属性应该设置为一个有效得JSON/Go属性路径表达式。   |

## Service

|属性名|类型|说明|
|-|-|-|
|kind   |String   |值为Service   |
|apiVersion   |String   |值为v1   |
|metadata   |[ObjectMeta](#objectmeta)   |元数据   |
|spec   |[ServiceSpec](#servicespec)   |服务的描述对象   |

## ServiceSpec

|属性名|类型|说明|
|-|-|-|
|ports   |[[ServicePort](#serviceport)]   |端口列表。本服务暴露的端口列表   |
|selector   |Object   |选择器。把本服务的数据发送给符合这个选择器的容器。如果为空，该服务会被认为有一个外部的进程在管理本服务的终端，Kubernetes不会修改本选择器。本选择器应用于类型为ClusterIP，NodePort或LoadBalancer的服务。若类型为ExternalName，则本属性会被忽略。   |
|clusterIP   |String   |本服务在集群内的IP地址，通常被随机设置。如果指定了本IP，并且该IP没有被其他人使用，本IP会被使用，否则本服务的创建会失败。无法更新本属性。有效值有不设置，空字符串，与有效IP地址。不设置时，指定本服务为无终端服务（headless service），不需要进行代理。只在服务类型为ClusterIP，NodePort或LoadBalancer时有效。   |
|type   |String   |服务类型。服务类型指定了如何暴露本服务。默认为ClusterIP。有效的选项有ExternalName, clusterIP, NodePort和LoadBalancer。<br/>ExternalName：把服务映射到指定的外部域名。<br/>ClusterIP：分配一个集群内部的IP地址，提供到终端的负载均衡服务，终端由selector属性所选择，或是构建一个Endpoints对象来指定。如果clusterIP属性为空，则不会有任何的集群内IP分配给本服务，这些终端只会被暴露为一个终端的集合。<br/>NodePort：基于ClusterIP上构建的，并在所有的节点上分配一个端口，路由到该集群IP。<br/>LoadBalancer：基于NodePort构建的，创建一个外部的负载均衡器，并路由该集群IP。   |
|externalIPs   |[String]   |外部IP。这个服务也会接受来到外部IP列表内的IP的请求。这些IP不由Kubernetes管理。用户负责确保请求通过这个IP到达一个节点。举个例子，比如是外部的负载均衡服务的IP。   |
|sessionAffinity   |String   |值可以为ClientIP或None，默认为None。为ClientIP时，启用基于用户IP的粘滞会话。   |
|loadBalancerIP   |String   |只用于LoadBalancer类型的服务。使用这个IP来创建负载均衡器。这个功能依赖于底层的云服务提供商，要求他们支持使用一个IP创建负载均衡器。若不支持，则会被忽略。   |
|loadBalancerSourceRanges   |[String]   |若设置了本属性，并且云服务提供商支持，那么这会限制云服务提供商的负载均衡器只接受来自这些IP的请求。若不支持，则会被忽略。   |
|externalName   |String  |kubedns或相同功能的程序会返回一个包含本外部名的CNAME的记录，而不会进行代理。必须是一个有效的DNS域名，本服务类型必须为ExternalName   |
|externalTrafficPolicy   |String   |本属性指定是否这个服务需要将外部的请求路由到节点或者是整个集群的范围。若值为Local，将会在同一个节点上服务已经提供过服务的客户IP，而不会转发到其他节点或服务，但是可能会造成负载的集中。若为Cluster，则将请求均匀的分布到节点上，虽然可能造成一个客户的请求会发给多个节点处理，但是整体负载比较均衡。   |
|healthCheckNodePort   |Number   |指定本服务的健康检查的端口，如果没有设置，则会被后台服务API使用的节点端口。如果指定了将会使用该端口。只会在类型为LoadBalancer并且ExternalTrafficPolicy设为Local时生效。   |

## ServicePort

|属性名|类型|说明|
|-|-|-|
|name   |String   |本服务中的名字，必须是一个DNS_LABEL。一个服务中的所有端口必须有唯一的名字。若只有一个服务端口，则这个名字可以省略。这映射到EndpointPort对象的名称属性。   |
|protocol   |String   |本端口的IP协议，支持TCP和UDP   |
|port   |Number   |本服务暴露的物理端口   |
|targetPort   |String   |数字，或者是端口的名字，用于访问本服务的容器。数字必须在1-65535的范围内。名字必须时一个IANA_SVC_NAME。如果这是字符串，会被用来查找目标容器的端口。如果没有设置此属性，则会使用port属性。若ClusterIP=None，本属性会被忽略。如果本属性的值与port相同，应该省略本属性。   |
|nodePort   |Number   |当服务的类型为NodePort或LoadBalaner时，每个节点的这个端口都会被本服务使用。通常时系统自动设置的。如果指定了本属性，并且这些端口没有在用，服务将会成功创建，否则会失败。默认会自动分配一个端口。   |

## DeploymentConfig

|属性名|类型|说明|
|-|-|-|
|kind   |String   |值为DeploymentConfig   |
|apiVersion   |String   |值为v1   |
|metadata   |[ObjectMeta](#objectmeta)   |元数据   |
|spec   |[DeploymentConfigSpec](#deploymentconfigspec)   |指定如何部署   |

## DeploymentConfigSpec

|属性名|类型|说明|
|-|-|-|
|strategy   |[DeploymentStrategy](#deploymentstrategy)   |描述这个部署如何进行   |
|minReadySeconds   |Number   |一个新的容器被创建后，容器准备的最低时间，在该时间内准备好容器，则会被认为容器已经可用。默认为0，在容器准备完成后，就被认为是可用的了。   |
|triggers   |[[DeploymentTriggerPolicy](#deploymenttriggerpolicy)]   |触发器定义了更新部署配置时对新的部署产生的影响。如果没有触发器设置，则新的部署只会在部署配置更新的时候触发。如果本项为空，默认会有一个配置更新触发器。   |
|replicas   |Number   |用户希望同时运行的副本数。   |
|revisionHistoryLimit   |Number   |保留的复制控制器历史对象的数量上限。允许为0或者是不设置，默认是10。这个属性只适用于使用新的一组API资源创建的部署配置，而不是遗留的资源。（？）   |
|test   |Boolean   |本属性确保本部署配置会在除去一个部署运行的时候，有0个副本在运行。这允许部署配置被用作一个持续的部署测试——镜像修改时被触发，部署时运行，最终运行成功或失败。使用后策略钩子函数和后面的操作来集成成功的部署。   |
|paused   |Boolean   |表明这个部署配置是否暂停了，这使得模板更改或者是其他的触发器引起模板更改时，不会有新的部署产生。   |
|selector   |Object   |用于选择容器，以确定部署完成的副本数量。   |
|template   |[PodTemplateSpec](#podtemplatespec)   |模板是描述当副本数量不足时如何创建一个新的容器的方法。   |

## DeploymentStrategy

|属性名|类型|说明|
|-|-|-|
|type   |String   |这个部署策略的名字   |
|customParams   |[CustomDeploymentStrategyParams](#customdeploymentstrategyparams)   |输入到自定义部署策略的参数，也可以用在Recreate和Rolling策略，以自定义执行部署的进程。   |
|recreateParams   |[RecreateDeploymentStrategyParams](recreatedeploymentstrategyparams)   |输入给Recreate策略的参数。   |
|rollingParams   |[RollingDeploymentStrategyParams](#rollingdeploymentstrategyparams)   |输入给Rolling策略的参数。   |
|resources   |[ResourceRequirements](#resourcerequirements)   |为运行部署或者任何钩子函数，所需要的资源。   |
|labels   |Object   |一些键值对用于定制部署器和生命周期中前置和后置钩子容器（lifecycle pre/post hook pods）   |
|annotations   |Object   |与labels作用相同   |
|activeDeadlineSeconds   |Number   |这是一段时间，这段时间内，本部署配置的部署容器会一直保持活跃状态，之后，系统会主动尝试去关闭他们。   |

## DeploymentTriggerPolicy

|属性名|类型|说明|
|-|-|-|
|type   |String   |触发器的类型   |
|imageChangeParams   |[DeploymentTriggerImageChangeParams](#deploymenttriggerimagechangeparams)   |镜像修改触发器的函数   |

## DeploymentTriggerImageChangeParams

|属性名|类型|说明|
|-|-|-|
|automatic   |Boolean   |自动化的话意味着检测到新的镜像标签时候，容器模板内得镜像会进行更新   |
|containerNames   |[String]   |用于限制本标签的更新所影响的容器范围，只会影响到本属性设置的容器。如果多个触发器同时作用与相同得容器，结果将会是不可预测的，将来会把这个行为变成错误的输入。如果这些名字没有指向有效得容器，则这个触发器会被忽略。将来也会把这个行为视为错误的输入。   |
|from   |[ObjectReference](#objectreference)   |是需要本触发器监视的镜像流标签。需要设置Name属性，如果Namespace属性为空，则会使用当前的Namespace。   |
|lastTriggeredImage   |String   |最后一个触发的镜像   |

## PodTemplateSpec

|属性名|类型|说明|
|-|-|-|
|metadata   |[ObjectMeta](#objectmeta)   |元数据   |
|spec   |[PodSpec](#podspec)   |容器的行为设置   |

## PodSpec

|属性名|类型|说明|
|-|-|-|
|volumes   |[[Volume](https://github.com/packagewjx/swagger-openshift-client/blob/master/docs/V1Volume.md)]   |容器需要挂载的卷   |
|initContainers   |[[Container](https://github.com/packagewjx/swagger-openshift-client/blob/master/docs/V1Container.md)]   |本容器舱（Pod）的初始化容器列表。初始化容器会在容器启动之前运行。如果任何一个初始化容器失败了，本容器舱会被认为已经失败，并尝试重启。初始化容器或普通容器的名字必须在所有容器中是唯一的。初始化容器可能没有生命周期函数，可读性探测或者存活探测。初始化容器的资源需求也会在调度过程中考虑进去，寻找对每一种资源的最大的需求或限制，并使用那些值的最大值或者普通容器的资源需求值的最大值。初始化容器也会被项目的限制值所限制。初始化容器现在不能添加或删除，也不能更新。   |
|containers   |[[Container](https://github.com/packagewjx/swagger-openshift-client/blob/master/docs/V1Container.md)]   |本容器舱的容器列表。容器无法添加或移除，一个容器舱至少有一个容器，无法更新。   |
|restartPolicy   |String   |容器舱内所有容器的重启策略，有效值为Always,OnFailure,Never。默认是Always。   |
|terminationGracePeriodSeconds   |Number   |可选属性，指定让容器正常的停止所需要的时间。在删除请求中可能会被缩减。值必须为一个非负整数。0代表立即删除。如果本值为空，默认值为30秒。这个停止时间段内，容器的程序首先会被发送一个停止的信号，时间结束后，会被发送一个强制停止的信号。设置本值，以让你的程序能够在这段时间内做必须的清理工作。   |
|activeDeadlineSeconds   |Number   |这个时间段从容器启动的时间开始计算，如果过了这段时间容器仍未启动完成，将会被认为启动失败，并会结束相关的容器。值必须为一个正整数。   |
|dnsPolicy   |String   |设置容器舱内的容器的dns策略。有效值有ClusterFirstWithHostNet，ClusterFirst或Default。默认是ClusterFirst。   |
|nodeSelector   |Object   |选择部署容器舱的节点。   |
|serviceAccountName   |String   |运行该容器舱时，使用的服务账户的名称。   |
|automountServiceAccountToken   |Boolean   |指示是否自动装载服务账户的令牌   |
|nodeName   |String   |申请部署的节点。若设置了，将会假设该节点有资源运行该容器舱，并部署上去   |
|hostNetwork   |Boolean   |是否使用宿主的网络，使用宿主的网络空间。若设置了，必须设置使用的端口。默认为否。   |
|hostPID   |Boolean   |使用宿主的PID空间，默认为否   |
|hostIPC   |Boolean   |使用宿主的进程间通信空间，默认为否   |
|securityContext   |[PodSecurityContext](#podsecuritycontext)   |设置容器舱级别的安全配置以及常用的容器设置。默认为空。   |
|imagePullSecrets   |[[LocalObjectReference](#localobjectreference)]   |可选的用于拉取所需镜像的本命名空间内的密钥列表。   |
|hostname   |String   |指定容器舱的主机名   |
|subdomain   |String   |如设置了，容器的完整主机名将会是<hostname>.<subdomain>.<podnamespace>.svc.<cluster domain>。如果不设置，本容器舱不会有域名   |
|affinity   |[Affinity](#affinity)   |设置的会限制容器的调度   |
|schedulerName   |String   |设置的话，容器将会使用设置的调度器进行调度。   |
|tolerations   |[[Toleration](#toleration)]   |设置容器的容忍值   |
|hostAliases   |[[HostAlias](#hostalias)]   |可选列表，将会设置容器舱的hosts文件，添加主机及其对应的IP。只对非hostNetwork容器有效。   |

## PodSecurityContext

|属性名|类型|说明|
|-|-|-|
|seLinuxOptions   |[SelinuxOptions](#selinuxoptions)   |应用于所有容器的SELinux环境。如果没有设置，将会使用随即的SELinux环境。这个属性也会在SecurityContext对象中设置。如果都设置了，SecurityContext的同名属性将会被使用。   |
|runAsUser   |Number   |运行容器进程的用户UID，默认是镜像元数据中设置的用户   |
|runAsNonRoot   |Boolean   |指示是否用非root用户运行。如果设置为true，kubelet将会检查运行时，是否使用了非root用户，不设置或者为false时则不会检查。   |
|supplementalGroups   |[Number]   |给予每个容器第一个进程的组列表，除了GID以外的。   |
|fsGroup   |Number   |应用到所有容器的特别的额外用户组。有些卷允许Kubelet改变将来容器使用的卷的拥有者：1.拥有该卷的GID将会是FSGroup。2.setgid会被设置。3.权限位设置为OR'd with rw-rw（？）   |

## LocalObjectReference

## Affinity

## Toleration

## CustomDeploymentStrategyParams

|属性名|类型|说明|
|-|-|-|
|image   |String   |指定能够执行部署的一个docker镜像   |
|environment   |[[EnvVar](#envvar)]   |提供给运行部署镜像的容器的环境变量   |
|command   |[String]   |覆盖容器镜像本来的CMD语句   |

## RecreateDeploymentStrategyParams

## ResourceRequirements

## EnvVar

## BuildConfig
