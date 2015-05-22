# RESTful Web 服务之方法

正如目前为止我们所讨论的，RESTful Web 服务大量使用 HTTP 动词确定要对指定资源进行的操作。下面的表格演示了常用 HTTP 动词的例子。

<table>
	<tbody>
		<tr>
			<th>
				编号
			</th>
			<th>
				HTTP 方法，URI 和操作
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
				<br>
				http://localhost:8080/UserManagement/rest/UserService/users
				<br>
				获取用户列表
				<br>
				（只读）
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
				<br>
				http://localhost:8080/UserManagement/rest/UserService/users/1
				<br>
				获取 ID 为 1 的用户
				<br>
				(只读)
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
				<br>
				http://localhost:8080/UserManagement/rest/UserService/users/2
				<br>
				插入 ID 为 2 的用户
				<br>
				（幂等）
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
				<br>
				http://localhost:8080/UserManagement/rest/UserService/users/2
				<br>
				更新 ID 为 2 的用户
				<br>
				(N/A)
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
				<br>
				http://localhost:8080/UserManagement/rest/UserService/users/1
				<br>
				删除 ID 为 1 的用户
				<br>
				（幂等）
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
				<br>
				http://localhost:8080/UserManagement/rest/UserService/users
				<br>
				列出 Web 服务所支持的操作
				<br>
				（只读）
			</td>
		</tr>
		<tr>
			<td>
				7
			</td>
			<td>
				<b>
					HEAD
				</b>
				<br>
				http://localhost:8080/UserManagement/rest/UserService/users
				<br>
				只返回 HTTP 头，不返回 HTTP 体
				<br>
				（只读）
			</td>
		</tr>
	</tbody>
</table>

这里是要考虑的要点：

- GET 操作是只读且安全的。
- PUT 和 DELETE 操作是幂等的意味着它们的结果总是相同的，无论这个操作被调用多少次。
- PUT 和 POST 操作几乎是相同的，区别在于 PUT 操作的结果是幂等的，而 POST 操作会导致不同的结果。

## 示例

让我们更新一下 __第一个 RESTful Web 服务应用程序[first-application.md]__ 教程中创建的例子来创建一个可以执行 CRUD（创建，读取，更新，移除）操作的 Web 服务。简单起见，这里我们使用文件 I/O 替代数据库操作。

我们来更新一下 com.tutorialspoint 包下面的 __User.java，UserDao.java__ 和 __UserService.java__ 文件。

_User.java_

```
package com.tutorialspoint;

import java.io.Serializable;

import javax.xml.bind.annotation.XmlElement;
import javax.xml.bind.annotation.XmlRootElement;
@XmlRootElement(name = "user")
public class User implements Serializable {

   private static final long serialVersionUID = 1L;
   private int id;
   private String name;
   private String profession;

   public User(){}

   public User(int id, String name, String profession){
      this.id = id;
      this.name = name;
      this.profession = profession;
   }

   public int getId() {
      return id;
   }
   @XmlElement
   public void setId(int id) {
      this.id = id;
   }
   public String getName() {
      return name;
   }
   @XmlElement
      public void setName(String name) {
      this.name = name;
   }
   public String getProfession() {
      return profession;
   }
   @XmlElement
   public void setProfession(String profession) {
      this.profession = profession;
   }	

   @Override
   public boolean equals(Object object){
      if(object == null){
         return false;
      }else if(!(object instanceof User)){
         return false;
      }else {
         User user = (User)object;
         if(id == user.getId()
            && name.equals(user.getName())
            && profession.equals(user.getProfession())
         ){
            return true;
         }			
      }
      return false;
   }	
}
```

_UserDao.java_

```
package com.tutorialspoint;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.util.ArrayList;
import java.util.List;

public class UserDao {
   public List<User> getAllUsers(){
      List<User> userList = null;
      try {
         File file = new File("Users.dat");
         if (!file.exists()) {
            User user = new User(1, "Mahesh", "Teacher");
            userList = new ArrayList<User>();
            userList.add(user);
            saveUserList(userList);		
         }
         else{
            FileInputStream fis = new FileInputStream(file);
            ObjectInputStream ois = new ObjectInputStream(fis);
            userList = (List<User>) ois.readObject();
            ois.close();
         }
      } catch (IOException e) {
         e.printStackTrace();
      } catch (ClassNotFoundException e) {
         e.printStackTrace();
      }		
      return userList;
   }

   public User getUser(int id){
      List<User> users = getAllUsers();

      for(User user: users){
         if(user.getId() == id){
            return user;
         }
      }
      return null;
   }

   public int addUser(User pUser){
      List<User> userList = getAllUsers();
      boolean userExists = false;
      for(User user: userList){
         if(user.getId() == pUser.getId()){
            userExists = true;
            break;
         }
      }		
      if(!userExists){
         userList.add(pUser);
         saveUserList(userList);
         return 1;
      }
      return 0;
   }

   public int updateUser(User pUser){
      List<User> userList = getAllUsers();

      for(User user: userList){
         if(user.getId() == pUser.getId()){
            int index = userList.indexOf(user);			
            userList.set(index, pUser);
            saveUserList(userList);
            return 1;
         }
      }		
      return 0;
   }

   public int deleteUser(int id){
      List<User> userList = getAllUsers();

      for(User user: userList){
         if(user.getId() == id){
            int index = userList.indexOf(user);			
            userList.remove(index);
            saveUserList(userList);
            return 1;   
         }
      }		
      return 0;
   }

   private void saveUserList(List<User> userList){
      try {
         File file = new File("Users.dat");
         FileOutputStream fos;

         fos = new FileOutputStream(file);

         ObjectOutputStream oos = new ObjectOutputStream(fos);		
         oos.writeObject(userList);
         oos.close();
      } catch (FileNotFoundException e) {
         e.printStackTrace();
      } catch (IOException e) {
         e.printStackTrace();
      }
   }
}
```

_UserService.java_

```
package com.tutorialspoint;

import java.io.IOException;
import java.util.List;

import javax.servlet.http.HttpServletResponse;
import javax.ws.rs.Consumes;
import javax.ws.rs.DELETE;
import javax.ws.rs.FormParam;
import javax.ws.rs.GET;
import javax.ws.rs.OPTIONS;
import javax.ws.rs.POST;
import javax.ws.rs.PUT;
import javax.ws.rs.Path;
import javax.ws.rs.PathParam;
import javax.ws.rs.Produces;
import javax.ws.rs.core.Context;
import javax.ws.rs.core.MediaType;

@Path("/UserService")
public class UserService {
	
   UserDao userDao = new UserDao();
   private static final String SUCCESS_RESULT="<result>success</result>";
   private static final String FAILURE_RESULT="<result>failure</result>";


   @GET
   @Path("/users")
   @Produces(MediaType.APPLICATION_XML)
   public List<User> getUsers(){
      return userDao.getAllUsers();
   }

   @GET
   @Path("/users/{userid}")
   @Produces(MediaType.APPLICATION_XML)
   public User getUser(@PathParam("userid") int userid){
      return userDao.getUser(userid);
   }

   @PUT
   @Path("/users")
   @Produces(MediaType.APPLICATION_XML)
   @Consumes(MediaType.APPLICATION_FORM_URLENCODED)
   public String createUser(@FormParam("id") int id,
      @FormParam("name") String name,
      @FormParam("profession") String profession,
      @Context HttpServletResponse servletResponse) throws IOException{
      User user = new User(id, name, profession);
      int result = userDao.addUser(user);
      if(result == 1){
         return SUCCESS_RESULT;
      }
      return FAILURE_RESULT;
   }

   @POST
   @Path("/users")
   @Produces(MediaType.APPLICATION_XML)
   @Consumes(MediaType.APPLICATION_FORM_URLENCODED)
   public String updateUser(@FormParam("id") int id,
      @FormParam("name") String name,
      @FormParam("profession") String profession,
      @Context HttpServletResponse servletResponse) throws IOException{
      User user = new User(id, name, profession);
      int result = userDao.updateUser(user);
      if(result == 1){
         return SUCCESS_RESULT;
      }
      return FAILURE_RESULT;
   }

   @DELETE
   @Path("/users/{userid}")
   @Produces(MediaType.APPLICATION_XML)
   public String deleteUser(@PathParam("userid") int userid){
      int result = userDao.deleteUser(userid);
      if(result == 1){
         return SUCCESS_RESULT;
      }
      return FAILURE_RESULT;
   }

   @OPTIONS
   @Path("/users")
   @Produces(MediaType.APPLICATION_XML)
   public String getSupportedOperations(){
      return "<operations>GET, PUT, POST, DELETE</operations>";
   }
}
```

现在，使用 Eclipse 将你的应用程序导出为 war 文件然后部署到 Tomcat 中。要使用 eclipse 创建 WAR 文件，遵循选项 __File -> export -> Web -> War File__，最后选择项目 UserManagement 和目标文件夹即可。要在 Tomcat 中部署 war 文件，把 UserManagement.war 放到 __Tomcat 安装目录__ 下的 webapps 目录中并启动 Tomcat 即可。

## 测试 Web 服务

Jersey 提供了创建 Web 服务客户端的 API 以测试 Web 服务。我们在同一项目的 com.tutorialspoint 包下面创建了一个范例测试类 __WebServiceTester.java__。

_WebServiceTester.java_

```
package com.tutorialspoint;

import java.util.List;

import javax.ws.rs.client.Client;
import javax.ws.rs.client.ClientBuilder;
import javax.ws.rs.client.Entity;
import javax.ws.rs.core.Form;
import javax.ws.rs.core.GenericType;
import javax.ws.rs.core.MediaType;

public class WebServiceTester  {

   private Client client;
   private String REST_SERVICE_URL = "http://localhost:8080/UserManagement/rest/UserService/users";
   private static final String SUCCESS_RESULT="<result>success</result>";
   private static final String PASS = "pass";
   private static final String FAIL = "fail";

   private void init(){
      this.client = ClientBuilder.newClient();
   }

   public static void main(String[] args){
      WebServiceTester tester = new WebServiceTester();
      //initialize the tester
      tester.init();
      //test get all users Web Service Method
      tester.testGetAllUsers();
      //test get user Web Service Method 
      tester.testGetUser();
      //test update user Web Service Method
      tester.testUpdateUser();
      //test add user Web Service Method
      tester.testAddUser();
      //test delete user Web Service Method
      tester.testDeleteUser();
   }
   //Test: Get list of all users
   //Test: Check if list is not empty
   private void testGetAllUsers(){
      GenericType<List<User>> list = new GenericType<List<User>>() {};
      List<User> users = client
         .target(REST_SERVICE_URL)
         .request(MediaType.APPLICATION_XML)
         .get(list);
      String result = PASS;
      if(users.isEmpty()){
         result = FAIL;
      }
      System.out.println("Test case name: testGetAllUsers, Result: " + result );
   }
   //Test: Get User of id 1
   //Test: Check if user is same as sample user
   private void testGetUser(){
      User sampleUser = new User();
      sampleUser.setId(1);

      User user = client
         .target(REST_SERVICE_URL)
         .path("/{userid}")
         .resolveTemplate("userid", 1)
         .request(MediaType.APPLICATION_XML)
         .get(User.class);
      String result = FAIL;
      if(sampleUser != null && sampleUser.getId() == user.getId()){
         result = PASS;
      }
      System.out.println("Test case name: testGetUser, Result: " + result );
   }
   //Test: Update User of id 1
   //Test: Check if result is success XML.
   private void testUpdateUser(){
      Form form = new Form();
      form.param("id", "1");
      form.param("name", "suresh");
      form.param("profession", "clerk");

      String callResult = client
         .target(REST_SERVICE_URL)
         .request(MediaType.APPLICATION_XML)
         .post(Entity.entity(form,
            MediaType.APPLICATION_FORM_URLENCODED_TYPE),
            String.class);
      String result = PASS;
      if(!SUCCESS_RESULT.equals(callResult)){
         result = FAIL;
      }

      System.out.println("Test case name: testUpdateUser, Result: " + result );
   }
   //Test: Add User of id 2
   //Test: Check if result is success XML.
   private void testAddUser(){
      Form form = new Form();
      form.param("id", "2");
      form.param("name", "naresh");
      form.param("profession", "clerk");

      String callResult = client
         .target(REST_SERVICE_URL)
         .request(MediaType.APPLICATION_XML)
         .put(Entity.entity(form,
            MediaType.APPLICATION_FORM_URLENCODED_TYPE),
            String.class);
   
      String result = PASS;
      if(!SUCCESS_RESULT.equals(callResult)){
         result = FAIL;
      }

      System.out.println("Test case name: testAddUser, Result: " + result );
   }
   //Test: Delete User of id 2
   //Test: Check if result is success XML.
   private void testDeleteUser(){
      String callResult = client
         .target(REST_SERVICE_URL)
         .path("/{userid}")
         .resolveTemplate("userid", 2)
         .request(MediaType.APPLICATION_XML)
         .delete(String.class);

      String result = PASS;
      if(!SUCCESS_RESULT.equals(callResult)){
         result = FAIL;
      }

      System.out.println("Test case name: testDeleteUser, Result: " + result );
   }
}
```

现在，使用 Eclipse 运行这个测试。右击文件，遵循选项 __Run as -> Java Application__。在 Eclipse 控制台中我们会看到如下所示结果：

```
Test case name: testGetAllUsers, Result: pass
Test case name: testGetUser, Result: pass
Test case name: testUpdateUser, Result: pass
Test case name: testAddUser, Result: pass
Test case name: testDeleteUser, Result: pass
```