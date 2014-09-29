#RESTful和api的设计实现
>2014/6/13

>Representational State Transfer，即表现层状态转移。要实现设计就要高清什么事表现层。
#表现层

简单的理解就是对资源的表现。资源是实体，应该是事物，不是动作。这一点需要明确。错误的设计会将动作设计进uri。应该确保动词不出现在你的uri里面。

#状态转化

http是无状态的协议。通过增删改查实现状态的变化，一般对应的是http的post，delete，put，get四个方法，这只是一个约束。

#实例

	查询一本书
	get /book/id
	查询符合过滤的一些书
	get /book?filter
	入库一本书
	post /book name=山海经&price=51.6
	更新书的内容
	put /book/id price=30.0
	删除一本书
	delete /book/id
	借出一本书
	post /event-borrow borrower=甲&date=20130104
	查询一个读者借阅的书
	get /person/id/book


#总结

避免在uri里出现动词,不要将表现层的资源和数据库的实体等同起来