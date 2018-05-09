#### GraphQL，一种新的思路
我们知道，用户信息对应的数据模型是固定的，每次请求其实是对这些数据做了过滤和筛选。对应到数据库操作，就是数据的查询操作。如果客户端也能够像“查询”一样发送请求，那不就可以从后端接口这个大的“大数据库”去过滤筛选业务需要的数据了吗？

GraphQL 就是基于这样的思想来设计的。上面提到的（a）和（b）类型的数据结构就是 GraphQL 的查询内容。使用上面的查询，GraphQL 服务器会分别返回如下响应内容。

a 查询对应的响应：

{
  "user" : {
    "id": 3500401,
    "name": "Jing Chen",
    "isViewerFriend": true
  }
}
b 查询对应的响应：

{
  "user" : {
    "name": "Jing Chen",
    "profilePicture": {
      "uri": "http: //someurl.cdn/pic.jpg",
      "width": 50,
      "height": 50
    }
  }
}
只需要改变查询内容，前端就能定制服务器返回的响应内容，这就是 GraphQL 的客户端指定查询（Client Specified Queries）。假如我们能够将基础数据平台做成一个 GraphQL 服务器，不就能为这个平台上的所有业务提供统一可复用的数据接口了吗？

##### 验证下效果：
curl -v -XPOST -H "Content-Type:application/graphql"  -d 'query RootQueryType { count }' http://localhost:3000/graphql

检查服务器

GraphQL 最让人感兴趣的是可以编写 GraphQL 查询来让 GraphQL 服务器告诉我们它支持那些查询，即官方文档提到的自检性（introspection）。

例如：

curl -XPOST -H 'Content-Type:application/graphql'  -d '{__schema { queryType { name, fields { name, description} }}}' http://localhost:3000/graphql

