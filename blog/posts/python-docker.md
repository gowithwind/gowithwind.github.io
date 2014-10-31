# 打包python app 到docker容器
>2014.10.31

##执行命令列表

    665  apt-get install docker.io #安装docker.io,注意不叫docker
    672  mkdir wind #新建一个容器配置的目录
    673  docker pull ubuntu:14.04 #拉取ubuntu的镜像
    675  cd wind 
    676  vi Dockerfile  #新建 Docker容器描述文件
  
##Dockerfile内容

    # This is a comment
    FROM ubuntu:14.04
    MAINTAINER gowithwind
    RUN apt-get update
    RUN apt-get install -y git
    RUN apt-get install -y vim
    RUN apt-get install -y python-pip
    RUN apt-get install -y python2.7-dev
    RUN pip install tornado
    ADD One app
    EXPOSE 8888
    WORKDIR app
    CMD python app.py

##创建容器

    694  git clone https://github.com/gowithwind/One.git  #下载app代码
    696  docker build -t="wind-2" . #建立容器 wind-2

##运行容器

    723  docker run -idt -p 8888:8888  wind-2:latest #运行容器，端口8888
  
##后记

先后在aliyun测试过，阿里云的速度实在太慢，构建过程很长，并且遇到一个docker服务不能启动的问题，具体参考<http://hanjianwei.com/2014/07/30/docker-on-aliyun.html>

docker 入门学习<http://yeasy.gitbooks.io/docker_practice/content/install/ubuntu.html>

比较好的参考 <https://www.digitalocean.com/community/tutorials/docker-explained-how-to-containerize-python-web-applications>


