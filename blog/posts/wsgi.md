#tornado和wsgi互操作
>2015-01-22

#web server
tornado=web server+web framework
而像flask,bottle这些都是web framework,一般是运行在wsgi之上的.
wsgi是python的web server gateway interface,就是web服务器的接口,框架的底层就是要实现这个接口.

一个简单wsgi的例子

    def app(environ, start_response):
  
      """Simplest possible application object"""
  
      data = 'Hello, World!\n'
  
      status = '200 OK'
  
      response_headers = [
  
          ('Content-type','text/plain'),
  
          ('Content-Length', str(len(data)))
  
      ]
  
      start_response(status, response_headers)
  
      return iter([data])

安装gunicorn后就可以,gunicorn app:app,来运行web服务了.

而tornado是包含web server的,其实也可以用wsgi来运行.但是wsgi没有异步特性,因此那些异步的东西都是没法用的.

    wsgi_app = tornado.wsgi.WSGIAdapter(application)
    server = wsgiref.simple_server.make_server('', 8888, wsgi_app)
    server.serve_forever()
  
而其他的web 框架也可以运行在tornado上面

    container = tornado.wsgi.WSGIContainer(simple_app)
    http_server = tornado.httpserver.HTTPServer(container)
    http_server.listen(8888)
    tornado.ioloop.IOLoop.instance().start()

因为tornado是单线程异步的,运行在其上的框架也只能是单线程运行了.

#框架的效率
其实各种基于wsgi的框架的,总体效率是可见的,如果需求比较简单,不需要复杂的路由,验证机制,框架越简单,效率会越高,越复杂,效率越低.

但是,web应用的性能中框架只是一环,更多的是io上的消耗,主要是数据库读写.

如果单纯是写api,建议使用轻量级的框架,比如falcon,bobo等.





