# RESTful Web 服务介绍

## 什么是 REST？

REST 表示表征性状态传输（REpresentational State Transfer）。REST 是一种基于 Web 标准的软件架构，它使用 HTTP 协议处理数据通信。它以资源为中心，其中每个组成部分都是一个资源，并且资源通过使用 HTTP 标准方法的公共接口访问。REST 由 Roy Fielding 在 2000 年首次提出。

在 REST 架构中，一个 REST 服务器只提供对资源的访问，REST 客户端访问并呈现资源。这里每个资源都通过 URIs/ 全局 ID 标识。REST 使用各种不同的表现形式表示资源，比如文本，JSON 和 XML。目前，JSON 是用于 Web 服务最流行的格式。

## HTTP 方法

下面是常用于基于 REST 架构中的众所周知的 HTTP 方法：

- __GET__ - 提供资源的只读访问。
- __PUT__ - 用于创建一个新资源。
- __DELETE__ - 用于移除一个资源。
- __POST__ - 用于更新现有资源或者创建一个新资源。
- __OPTIONS__ - 用于获取资源上支持的操作。

## RESTFul Web 服务

一个 Web 服务就是一个用于在应用程序或系统之间交换数据的开放协议和标准的集合。使用不同语言编写以及运行在不同平台上的软件应用可以使用 Web 服务跨计算机网络交换数据，比如互联网的方式类似于一台计算机上的进程通信。这种互操作性（比如，Java 和 Python，或者 Windows 和 Linux 应用程序之间）归功于开放标准的使用。

这种基于 REST 架构的 Web 服务就被称为 RESTful Web 服务。这些 Web 服务使用 HTTP 方法实现 REST 架构的概念。一个 RESTful Web 服务通常定义了一个 URI，即统一资源标示符服务；提供资源表示形式比如 JSON 和设置 HTTP 方法。

## 创建 RESTFul Web 服务

本教程将会创建一个带以下功能的用户管理 Web 服务：

<table>
	<tbody>
		<tr>
			<th>
				编号
			</th>
			<th>
				HTTP 方法
			</th>
			<th>
				URI
			</th>
			<th>
				操作
			</th>
			<th>
				操作类型
			</th>
		</tr>
		<tr>
			<td>
				1
			</td>
			<td>
				<b>
					GET
				</b>
			</td>
			<td>
				/UserService/users
			</td>
			<td>
				获取用户列表
			</td>
			<td>
				只读
			</td>
		</tr>
		<tr>
			<td>
				2
			</td>
			<td>
				<b>
					GET
				</b>
			</td>
			<td>
				/UserService/users/1
			</td>
			<td>
				获取 ID 为 1 的用户
			</td>
			<td>
				只读
			</td>
		</tr>
		<tr>
			<td>
				3
			</td>
			<td>
				<b>
					PUT
				</b>
			</td>
			<td>
				/UserService/users/2
			</td>
			<td>
				插入 ID 为 2 的用户
			</td>
			<td>
				幂等
			</td>
		</tr>
		<tr>
			<td>
				4
			</td>
			<td>
				<b>
					POST
				</b>
			</td>
			<td>
				/UserService/users/2
			</td>
			<td>
				更新 ID 为 2 的用户
			</td>
			<td>
				N/A
			</td>
		</tr>
		<tr>
			<td>
				5
			</td>
			<td>
				<b>
					DELETE
				</b>
			</td>
			<td>
				/UserService/users/1
			</td>
			<td>
				删除 ID 为 1 的用户
			</td>
			<td>
				幂等
			</td>
		</tr>
		<tr>
			<td>
				6
			</td>
			<td>
				<b>
					OPTIONS
				</b>
			</td>
			<td>
				/UserService/users
			</td>
			<td>
				列出 Web 服务所支持的操作
			</td>
			<td>
				只读
			</td>
		</tr>
	</tbody>
</table>