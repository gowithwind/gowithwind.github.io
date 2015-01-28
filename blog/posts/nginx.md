#nginx的110和111错误
>20150128

#环境
生产环境是一个高并发场景,并且几乎每个请求都需要访问数据库.进程管理用的是supervisor,后端是tornado.

#111错误
是指找不到upstream,查了一下是因为跑了几十个后端实例,但是部分实例启动失败,失败原因是端口占用.
但是查下来又没有被占用,真的是很苦恼了.翻了stackoverflow也没找到什么方案.不管supervisor是在virtualenv环境下跑的.
在原生python环境重新安装了supervisor,居然可以了，只能说什么呢，莫名其妙，是不是virtualenv存在问题？
至此111错误不见了

#110错误
是指连接upstream超时，起初觉得是后端的问题，但是查看tornado的日志并没错误。mysql的负载也没有很大，怎么会就超时了呢。
各种测试nginx的proxy timeout的参数，无济于事。
查看了tail -f -n 100 /var/log/messages
    conntrack table full；drop packet

也就是在丢包了。看来是并发量太大，网络跟踪出问题了。关闭防火墙iptables以后，世界安静了。

当然，俺不是专业运维，还不太明白这些玩意具体是为啥子。其实iptables里设置可以解决问题。

#真正的程序问题
没有哗啦啦的错误日志了，但是系统的负载突然变高了，因为报错的流量都回来了。mysql跑到了90%，负载超到了3.

接下来当然是修改代码，加入缓存，或者是直接使用kv数据库。

