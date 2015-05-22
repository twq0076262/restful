# RESTful Web 服务之无状态

根据 REST 架构，一个 RESTful Web 服务不应该在服务器上保持客户端状态。这种约束被称为无状态。客户端的职责是传递其上下文给服务器，然后服务器存储这个上下文以处理客户端的请求。比如，由服务器维护的会话是通过客户端传递的会话标示符识别的。

RESTful Web 服务应该遵守这一约束。我们已经在 __RESTful Web 服务之方法[methods.md]__ 教程中见过，Web 服务方法不会存储调用它们的客户端的任意信息。

考虑如下 URI：

```
http://localhost:8080/UserManagement/rest/UserService/users/1
```

如果我们使用浏览器，使用基于 Java 的客户端或者使用 postman 访问上面的 url，结果始终是 User XML 并且它的 ID 为 1，因此服务器并没有存储客户端相关的任意信息。

```
<user>
<id>1</id>
<name>mahesh</name>
<profession>1</profession>
</user>
```

## 无状态的优势

下面是 RESTful Web 服务中无状态的好处：

- Web 服务可以独立对待每个请求方法。
- Web 服务不需要维护客户端先前的交互。简化了应用程序设计。
- HTTP 本身是一个无状态协议，RESTful Web 服务可与 HTTP 协议无缝协作。

## 无状态的缺点

下面是 RESTful Web 服务中无状态的缺点：

- Web 服务需要在每个请求中获取额外的信息，然后在客户端交互需要处理的情况下解读客户端状态。