#mongodb基本使用

##安装

1. sudo apt-get install -y mangodb
1. service mangodb start
1. mango #shell
1. 安装robomongo图形客户端 [http://robomongo.org/](http://robomongo.org/) #可选
1. sudo pip install pymongo

#使用

    from pymongo import MongoClient
    client = MongoClient()
    db=client.test_db
    websites=db.websites #colletions
    print websites.insert({
    'domain':'xxx',
    'server':'test',
    'js':'',
    })
