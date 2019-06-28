### RESTful API设计规范

REST API 是一系列个体可描述 (individually-addressable) 的资源（API的名词）的模型。 资源可以通过他们的资源名称来提及，并可以通过一个小集合内的方法（即API的动词）来操作。



**建议按照下列步骤来设计面向资源的API**

1.确定API提供的资源类型

2.查明不同资源间的关系

3.根据资源的类型和关系，决定资源名称的规范

4.决定资源的范式 (schema)

5.为资源加上方法的最小集合



**协议**

API与用户的通信协议，总是使用HTTPs协议。



**域名**

应该尽量将API部署在专用域名之下。如果确定API很简单，不会有进一步扩展，可以考虑放在主域名下。



**路径**

路径又称”终点”（endpoint），表示API的具体网址。

在RESTful架构中，每个网址代表一种资源（resource），**所以网址中不能有动词，只能有名词**，而且所用的名词往往与数据库的表格名对应。

一般来说，数据库中的表都是同种记录的”集合”（collection），**所以API中的名词也应该使用复数。**

举例来说，有一个API提供动物园（zoo）的信息，还包括各种动物和雇员的信息，则它的路径应该设计成下面这样。

https://api.example.com/v1/zoos



**HTTP动词**

对于资源的具体操作类型，由HTTP动词表示。

常用的HTTP动词有下面五个（括号里是对应的SQL命令）。

GET（SELECT）：从服务器取出资源（一项或多项）。

POST（CREATE）：在服务器新建一个资源。

PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。

PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。

DELETE（DELETE）：从服务器删除资源。



**过滤、排序、选择、分页**

API应该提供参数，**过滤**返回结果。

如：http://api.example.com/zoos/ID/animals?limit=10

指定返回的动物园的动物记录的数量



GET /cars?sort=-manufactorer,+model

这是返回根据生产者降序和模型升序**排列**的car集合



可以使用fields查询参数来**选择**限制返回的域例如：

GET /cars?fields=manufacturer,model,id,color



将**分页**信息放到link header里面：http://tools.ietf.org/html/rfc5988#page-6。



**状态码**

服务器向用户返回的状态码和提示信息，常见的有以下一些（方括号中是该状态码对应的HTTP动词）。

200 – OK – 一切正常

201 – OK – 新的资源已经成功创建

204 – OK – 资源已经成功擅长

304 – Not Modified – 客户端使用缓存数据

400 – Bad Request – 请求无效，需要附加细节解释如 “JSON无效”

401 – Unauthorized – 请求需要用户验证

403 – Forbidden – 服务器已经理解了请求，但是拒绝服务或这种请求的访问是不允许的。

404 – Not found – 没有发现该资源

422 – Unprocessable Entity – 只有服务器不能处理实体时使用，比如图像不能被格式化，或者重要字段丢失。

500 – Internal Server Error – API开发者应该避免这种错误。



**使用Http头声明序列化格式**

在客户端和服务端，双方都要知道通讯的格式，格式在HTTP-Header中指定

Content-Type 定义请求格式

Accept 定义系列可接受的响应格式
