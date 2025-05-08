# yapi

![Version: 0.1.0](https://img.shields.io/badge/Version-0.1.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.10.2](https://img.shields.io/badge/AppVersion-1.10.2-informational?style=flat-square)

A Helm chart for [yapi](<https://github.com/fjc0k/docker-YApi>)

Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+
- mongodb（2.6+）

## Usage


[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

    helm repo add <alias> https://joelee2012.github.io/helm-charts

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo
yapi` to see the charts.

To install the yapi chart:

    helm install my-yapi <alias>/yapi

To uninstall the chart:

    helm uninstall my-yapi


## Configuration

See [Customizing the Chart Before Installing](https://helm.sh/docs/intro/using_helm/#customizing-the-chart-before-installing).
To see all configurable options with detailed comments, visit the chart's [values.yaml](./values.yaml), or run these configuration commands:

```console
# Helm 3
$ helm show values <alias>/yapi
```



## 通过环境变量配置
通过环境变量配置的选项会覆盖通过 `config.json` 或者 `config.js` 配置的选项。
#### 基础配置
| 环境变量名称        | 类型    | 说明                                                                                           | 示例                            |
| ------------------- | ------- | ---------------------------------------------------------------------------------------------- | ------------------------------- |
| YAPI_ADMIN_ACCOUNT  | string  | 管理员账号（邮箱）                                                                             | admin@foo.bar                   |
| YAPI_ADMIN_PASSWORD | string  | 管理员密码                                                                                     | adm1n                           |
| YAPI_CLOSE_REGISTER | boolean | 是否关闭注册，由于 docker-YApi 已[内置相关插件](#内置插件)，你可在关闭注册后在后台手动添加用户 | true                            |
| YAPI_NPM_REGISTRY   | string  | npm 源，目前仅在安装插件时使用，默认官方源，国内可以设为淘宝源加速                             | https://registry.npm.taobao.org |
#### 数据库配置
| 环境变量名称           | 类型   | 说明                                                                                                                                                                  | 示例                                                            |
| ---------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| YAPI_DB_SERVERNAME     | string | MongoDB 服务地址                                                                                                                                                      | yapi-mongo                                                      |
| YAPI_DB_PORT           | number | MongoDB 服务端口                                                                                                                                                      | 27017                                                           |
| YAPI_DB_DATABASE       | string | 使用的 MongoDB 数据库                                                                                                                                                 | yapi                                                            |
| YAPI_DB_USER           | string | 登录 MongoDB 服务的用户名                                                                                                                                             | root                                                            |
| YAPI_DB_PASS           | string | 登录 MongoDB 服务的用户密码                                                                                                                                           | r00t                                                            |
| YAPI_DB_AUTH_SOURCE    | string | MongoDB 身份认证所用库                                                                                                                                                | admin                                                           |
| YAPI_DB_CONNECT_STRING | string | 使用 MongoDB 集群时配置                                                                                                                                               | mongodb://127.0.0.100:8418,127.0.0.101:8418/yapidb?slaveOk=true |
| YAPI_DB_OPTIONS        | json   | Mongoose 连接 MongoDB 服务时的额外选项，一般不用设置。请参考: [Mongoose.prototype.connect()](https://mongoosejs.com/docs/api/mongoose.html#mongoose_Mongoose-connect) | {}                                                              |
#### 邮件配置
| 环境变量名称        | 类型    | 说明                                                                                                            | 示例                                 |
| ------------------- | ------- | --------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| YAPI_MAIL_ENABLE    | boolean | 是否启用                                                                                                        | true                                 |
| YAPI_MAIL_HOST      | string  | 邮件服务地址                                                                                                    | smtp.163.com                         |
| YAPI_MAIL_PORT      | number  | 邮件服务端口                                                                                                    | 465                                  |
| YAPI_MAIL_FROM      | string  | 发送人邮箱                                                                                                      | foo@163.com                          |
| YAPI_MAIL_AUTH_USER | string  | 登录邮件服务的用户名                                                                                            | bar@163.com                          |
| YAPI_MAIL_AUTH_PASS | string  | 登录邮件服务的用户密码                                                                                          | f00bar                               |
| YAPI_MAIL_OPTIONS   | json    | 传递给 Nodemailer 的额外选项，一般不用设置。请参考：[Nodemailer > SMTP transport](https://nodemailer.com/smtp/) | {"tls":{"rejectUnauthorized":false}} |
#### LDAP 登录配置
[点击查看 YApi 仓库下 LDAP 相关的 issues 👉](https://github.com/YMFE/yapi/issues?utf8=%E2%9C%93&q=ldap)
| 环境变量名称                    | 类型    | 说明                                                                                                                                                             | 示例                   |
| ------------------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------- |
| YAPI_LDAP_LOGIN_ENABLE          | boolean | 是否启用                                                                                                                                                         | true                   |
| YAPI_LDAP_LOGIN_SERVER          | string  | LDAP 服务地址                                                                                                                                                    | ldap://ldap.foo.bar    |
| YAPI_LDAP_LOGIN_BASE_DN         | string  | 登录 LDAP 服务的用户名                                                                                                                                           | cn=admin,dc=foo,dc=bar |
| YAPI_LDAP_LOGIN_BIND_PASSWORD   | string  | 登录 LDAP 服务的用户密码                                                                                                                                         | f00bar                 |
| YAPI_LDAP_LOGIN_SEARCH_DN       | string  | 查询用户数据的路径                                                                                                                                               | ou=users,dc=foo,dc=bar |
| YAPI_LDAP_LOGIN_SEARCH_STANDARD | string  | 支持两种值：<br />1、前端登录账号对应的查询字段，如：`mail`、`uid` 等；<br />2、自定义查询条件，其中 `%s` 会被前端登录账号替换，如：`&(objectClass=user)(cn=%s)` | -                      |
| YAPI_LDAP_LOGIN_EMAIL_POSTFIX   | string  | 登录邮箱后缀                                                                                                                                                     | @163.com               |
| YAPI_LDAP_LOGIN_EMAIL_KEY       | string  | LDAP 数据库存储用户邮箱的字段                                                                                                                                    | mail                   |
| YAPI_LDAP_LOGIN_USERNAME_KEY    | string  | LDAP 数据库存储用户名的字段                                                                                                                                      | name                   |
#### 插件配置
| 环境变量名称 | 类型 | 说明                                                                                                                                                                                                                                                                                                                                                                                                                               | 示例                             |
| ------------ | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| YAPI_PLUGINS | json | 要使用的插件列表。[点击查看开源 YApi 插件列表 →](https://www.npmjs.com/search?q=yapi-plugin-)<br /><br />**配置项数据格式：**<br />{<br /> "name": "插件名称，必须去除前缀 yapi-plugin-",<br /> "options": "插件配置，没有配置则不必设置"<br />}<br /><br />**注意：**<br />安装插件会运行 YApi 自带的打包命令，其内存消耗较大，因此，在安装插件时，物理机可用内存最好大于等于 `4GB`，否则，易出现内存溢出错误，导致插件安装失败。 | [{"name":"gitlab","options":{}}] |


## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| autoscaling.enabled | bool | `false` |  |
| autoscaling.maxReplicas | int | `100` |  |
| autoscaling.minReplicas | int | `1` |  |
| autoscaling.targetCPUUtilizationPercentage | int | `80` |  |
| envVars | object | `{}` |  |
| fullnameOverride | string | `""` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"jayfong/yapi"` |  |
| image.tag | string | `""` |  |
| imagePullSecrets | list | `[]` |  |
| ingress.annotations | object | `{}` |  |
| ingress.className | string | `""` |  |
| ingress.enabled | bool | `false` |  |
| ingress.hosts[0].host | string | `"chart-example.local"` |  |
| ingress.hosts[0].paths[0].path | string | `"/"` |  |
| ingress.hosts[0].paths[0].pathType | string | `"ImplementationSpecific"` |  |
| ingress.tls | list | `[]` |  |
| livenessProbe.httpGet.path | string | `"/"` |  |
| livenessProbe.httpGet.port | string | `"http"` |  |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| podAnnotations | object | `{}` |  |
| podLabels | object | `{}` |  |
| podSecurityContext | object | `{}` |  |
| readinessProbe.httpGet.path | string | `"/"` |  |
| readinessProbe.httpGet.port | string | `"http"` |  |
| replicaCount | int | `1` |  |
| resources | object | `{}` |  |
| securityContext | object | `{}` |  |
| service.port | int | `3000` |  |
| service.type | string | `"ClusterIP"` |  |
| serviceAccount.annotations | object | `{}` |  |
| serviceAccount.automount | bool | `true` |  |
| serviceAccount.create | bool | `true` |  |
| serviceAccount.name | string | `""` |  |
| tolerations | list | `[]` |  |
| volumeMounts | list | `[]` |  |
| volumes | list | `[]` |  |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.11.0](https://github.com/norwoodj/helm-docs/releases/v1.11.0)
