# RESTful Web 服务之安全性

因为 RESTful Web 服务使用 HTTP URLs 路径，因此以保护网站同样的方式维护 RESTful Web 服务是非常重要的。以下是设计 RESTful Web 服务时要遵循的最佳实践。

- __验证__ - 验证服务器上的所有输入。保护服务器免受 SQL 或者 NoSQL 注入攻击。
- __基于会话的认证__ - 请求一个 Web 服务方法时使用基于会话的认证对用户进行身份验证。
- __URL 不要有敏感数据__ - 永远不要在 URL 中使用用户名，密码或者会话标记，这些值应该通过 POST 方法传递给 Web 服务。
- __限制方法执行__ - 允许限制使用方法，比如 GET，POST，DELET。GET 方法不应该能够删除数据。
- __验证有缺陷的 XML/JSON__ - 检查格式良好的输入传递给 Web 服务方法。
- __抛出通用错误消息__ - Web 服务方法应该使用 HTTP 错误消息，比如 403 展示禁止访问等。

## HTTP 状态码

<table class="src">
	<tbody>
		<tr>
			<th>
				编号
			</th>
			<th>
				HTTP 状态码 &amp; 描述
			</th>
		</tr>
		<tr>
			<td>
				1
			</td>
			<td>
				<b>
					200
				</b>
				<br>
				<b>
					OK
				</b>
				，显示成功。
			</td>
		</tr>
		<tr>
			<td>
				2
			</td>
			<td>
				<b>
					201
				</b>
				<br>
				<b>
					CREATED
				</b>
				，当资源使用 POST 或者 PUT 请求建立成功时。使用位置头返回新建资源的链接。
			</td>
		</tr>
		<tr>
			<td>
				3
			</td>
			<td>
				<b>
					204
				</b>
				<br>
				<b>
					NO CONTENT
				</b>
				，当响应体为空时。比如，DELETE 请求。
			</td>
		</tr>
		<tr>
			<td>
				4
			</td>
			<td>
				<b>
					304
				</b>
				<br>
				<b>
					NOT MODIFIED
				</b>
				在有条件的 GET 请求的情况下用于减少网络带宽的使用。响应体应该为空。头信息应该包含日期，位置等。
			</td>
		</tr>
		<tr>
			<td>
				5
			</td>
			<td>
				<b>
					400
				</b>
				<br>
				<b>
					BAD REQUEST
				</b>
				，指出提供的输入无效。比如验证错误，数据缺失。
			</td>
		</tr>
		<tr>
			<td>
				6
			</td>
			<td>
				<b>
					401
				</b>
				<br>
				<b>
					UNAUTHORIZED
				</b>
				，指出用户正在使用无效的或者错误的认证令牌。
			</td>
		</tr>
		<tr>
			<td>
				7
			</td>
			<td>
				<b>
					403
				</b>
				<br>
				<b>
					FORBIDDEN
				</b>
				，指出用户没有使用访问方法。比如，没有管理员权限访问删除操作。
			</td>
		</tr>
		<tr>
			<td>
				8
			</td>
			<td>
				<b>
					404
				</b>
				<br>
				<b>
					NOT FOUND
				</b>
				，指出该方法不可用。
			</td>
		</tr>
		<tr>
			<td>
				9
			</td>
			<td>
				<b>
					409
				</b>
				<br>
				<b>
					CONFLICT
				</b>
				，指出执行方法时冲突，比如添加重复的条目。
			</td>
		</tr>
		<tr>
			<td>
				10
			</td>
			<td>
				<b>
					500
				</b>
				<br>
				<b>
					INTERNAL SERVER ERROR
				</b>
				，指出执行该方法时服务器抛出了一些异常。
			</td>
		</tr>
	</tbody>
</table>