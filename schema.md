# Openshift对象定义

<!-- TOC depthFrom:2 depthTo:4 withLinks:1 updateOnSave:1 orderedList:0 -->

- [说明](#说明)
- [Route](#route)
- [ObjectMeta](#objectmeta)
- [Service](#service)
	- [ServiceSpec](#servicespec)
- [DeploymentConfig](#deploymentconfig)
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

推荐使用YAML来编写一个对象的定义，请看[这里](http://ansible-tran.readthedocs.io/en/latest/docs/YAMLSyntax.html)，了解YAML的语法。

本文参考[Openshift官方REST API文档](https://docs.openshift.org/latest/rest_api/index.html)以及Openshift的Swagger API文档，使用Swagger Codegen生成的[对象定义文件](https://github.com/packagewjx/swagger-openshift-client/tree/master/src/model)编辑而成。如果发现任何错误，请尽管[提交Issue](https://github.com/packagewjx/openshift-api-object-schema/issues)到本仓库，或是提交PR，谢谢大家！

## Route

## ObjectMeta

|属性名|类型|说明|
|-|-|-|
|name   |String   |对象名。小写字母或数字开头，后接小写字母、数字或连字符“-”。本名称在一个项目（名称空间）中是唯一的，创建时必须设置本属性，并且不能更新名称。   |
|generateName   |String   |对象生成名。这是一个前缀，只有当对象名不存在时，服务器才会使用本前缀自动产生一个对象名给该对象，   |
|namespace   |String   |定义对象名的空间，使得一个对象名在该空间唯一。这与Openshift的项目是同一个。默认空间为“default”。不是所有的对象都有名称空间，并且无名称空间的对象这一属性为空。无法更新本属性。   |
|labels   |Object   |标签。用于组织和分类对象的属性，使用键值对的方式设置，常用于复制控制器、服务、路由等的选择器的依据。   |
|annotations   |Object   |注解。用于给一些外部插件或程序使用的额外的元数据，当修改本对象时，应该保证注解不改变。   |

## Service

|属性名|类型|说明|
|-|-|-|
|kind   |String   |值为Service   |
|apiVersion   |String   |值为v1   |
|metadata   |[ObjectMeta](objectmeta)   |元数据   |
|spec   |[ServiceSpec](#servicespec)   |服务的描述对象   |

### ServiceSpec

## DeploymentConfig

## BuildConfig
