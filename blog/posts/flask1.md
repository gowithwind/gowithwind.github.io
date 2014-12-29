#并不神奇,Flask的route解析(装饰器)
>2014-12-29

>翻译 <http://ains.co/blog/things-which-arent-magic-flask-part-1.html>

这篇文章主要讲解flask的@app.route的实现方法.

    app = Flask(__name__)
    
    @app.route("/")
    def hello():
        return "Hello World!"

##什么是装饰器

并不神奇,装饰器=f1(f),数学上其实就是这样的,返回一个包含f的f1函数.

    # This is our decorator
    def f1(f):
        # This is the new function we're going to return
        # This function will be used in place of our original definition
        def wrapper():
            print "Entering Function"
            f()
            print "Exited Function"
    
        return wrapper
    
    @f1 
    def f():
        print "Hello World"
    
    f()
    
执行过程就是f1(f)了.

下面要带参数呢.

    def f1(enter_message, exit_message):
        # We're going to return this decorator
        def simple_decorator(f):
            def wrapper():
                print enter_message
                f()
                print exit_message
    
            return wrapper
    
        return simple_decorator
    
    @decorator_generator("Start", "End")
    def f():
        print "Hello World"
    
    f()

其实相对于这样的f1(f)(args),返回一个带参数的函数.

#路由
路由其实就是根据路径去匹配相应的函数,是一个映射(map).
得益于python的函数是first class citizen.

因此你可以很方便的这样执行

    router['/]=index()
    router['/page']=page()
    
用上面的装饰器实现就是

    @router('/')
    def index():pass
    
这样就可以根据路径找到对应的执行函数了.

翻译的比较简单.这里面主要有一个**闭包**的概念.



