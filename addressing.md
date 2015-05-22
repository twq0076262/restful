# RESTful Web 服务之寻址

寻址指的是定位存储在服务器上的一个或多个资源。类似于定位某个人的邮寄地址。

REST 架构中的每个资源都通过它的 URI（统一资源标示符）标识。URI 格式如下：

```
<protocol>://<service-name>/<ResourceType>/<ResourceID>
```

URI 的目的是定位托管 Web 服务的服务器上的资源。请求的另一个重要的属性是 VERB，它用于标识要在资源上执行的操作。比如，在 __第一个 RESTful Web 服务应用程序[first-application.md]__ 教程中，URI 就是 __http://localhost:8080/UserManagement/rest/UserService/users__，VERB 是 GET。

## 构建一个标准的 URI

下面是设计 URI 时要考虑的要点：

- __使用复数名词__ - 使用复数名词定义资源。比如，我们使用 __users__ 标识用户资源。
- __避免使用空格__ - 处理长资源名时使用下划线（_）或者连字符（-），比如，用 authorized_users 而不是 authorized%20users。
- __使用小写字母__ - 尽管 URI 不区分带小写，但是在 url 中使用小写字母是一种很好的做法。
- __保持向后兼容__ - 由于 Web 服务是一种公共服务，URI 一旦公开之后应该始终可用。这种情况下，要更新 URI，请使用 HTTP 状态码 - 300 重定向老的 URI 到新的 URI。
- __使用 HTTP Verb__ - 始终使用 HTTP Verb，比如 GET，PUT 以及 DELETE 处理资源操作。在 URL 中使用操作名并不好。

## 示例

下面是一个获取用户的不好的 URI 示例：

```
http://localhost:8080/UserManagement/rest/UserService/getUser/1
```

下面是一个获取用户的好的 URI 示例：

```
http://localhost:8080/UserManagement/rest/UserService/users/1
```