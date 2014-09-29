#随机洗牌和有权重的随机洗牌
>2014/8/25

python中已经有shuffle的实现了。

这里思考下自己指明实现，其实很简单。洗牌就是每个元素给个随机数，然后排个序就好。


	items.sort(key=lambda item:random.random())

带权重的随机洗牌，其实就是，随机数需要加权。

	items.sort(key=lambda item:random.random()*item.weight)