# RESTful Web 服务之资源

## 什么是资源？

REST 架构把所有内容都视为资源。这些资源可以是文本文件，html 页面，图像，视频或者动态业务数据。REST 服务器只提供对资源的访问，REST 客户端访问和修改资源。这里每个资源都通过 URIs/ 全局 IDs 标识。REST 使用不同的表示形式表示资源，比如文本，JSON，XML。XML 和 JSON 是最流行的资源表示形式。

## 资源表示形式

REST 中的资源类似于面向对象编程中的对象或者类似于数据库中的实体。一旦资源被确定，它的表现就会决定使用某个标准的格式，因此服务器可以按照上述格式发送这些资源，客户端也可以理解同样的格式。

比如，在 __第一个 RESTful Web 服务应用程序[first-application.md]__ 教程中，用户就是一个资源，其中使用如下所示的 XML 格式表示：

```
<user>
   <id>1</id>
   <name>Mahesh</name>
   <profession>Teacher</profession>
</user>
```

同样的资源也可以使用如下所示的 JSON 格式表示：

```
{
   "id":1,
   "name":"Mahesh",
   "profession":"Teacher"
}
```

## 良好的资源表示形式

REST 并不对资源表示格式施加任何限制。对于服务器上的同一资源，一个客户端可以要求使用 JSON 表示，而另一个客户端可能会要求使用 XML 表示。REST 服务器的责任就是以客户端理解的格式传递客户端资源。

下面是在 RESTful Web 服务中设计资源表示格式时要考虑的重点。

- __可理解性__：服务器端和客户端都应该能理解和利用资源表示格式。
- __完整性__：格式应该能够完整地表示一个资源。比如，资源可以包含另一个资源。格式应该能够表示简单以及复杂的资源结构。
- __可连接性__：资源可以链接到另一个资源，格式应该能够处理这种情况。

然而，目前大多数 Web 服务都使用 XML 或者 JSON 格式表示资源。有很多库和工具都可以用来理解，解析和修改 XML 和 JSON 数据。