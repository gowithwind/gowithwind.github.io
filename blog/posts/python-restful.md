#python restful api框架
>2014/6/30

随着app的流行，客户端与服务器间的交互不再是简单的http请求，响应返回html的年代。

客户端开始更多在mvc的架构上实现，而后端更多是负责提供数据，相当于web services。

而restful就是一种web services的架构。

> 已有的python restful框架 参考
> <http://www.django-rest-framework.org/>

实现一个框架主要是可重用性。
一个抽象的资源具有以下的方法，


     class Resource(object):
          def get():
               return
          def post():
               return
          def delete():
               return    
          def put():
               return

实现的表现层资源需要在web框架里注册，以便路由控制到这里。
不同的框架实现是不同的。这里以tornado为例


     class Handler(Request):
          def get():
          def post():
          def delete():
          def put():
               resource=getResourceClass()
               return resource.put()