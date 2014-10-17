#nginx与域名配置
>2014/10/17

1. 一般首先将dns里裸域配置到ip,nginx里配置裸域301跳转到www

        # redirect http(s)://example.com to http(s)://www.example.com
        server {
        server_name 1day1app.com;
        return 301 $scheme://www.$host$request_uri;
        }

2. dns里配置各种二级域名到裸域，server_name 其实可以正则匹配的

        upstream frontends {
        server 127.0.0.1:1234; #定义后端
        }
        server {
        listen 80; #监听端口
        server_name www.1day1app.com photo.1day1app.com; #支持正则
        location / {
        proxy_read_timeout 1800;
        proxy_pass_header Server;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Scheme $scheme;
        proxy_pass http://frontends;
        }
        }


3. 对于没有匹配到的域名可以在nginx.conf 文件里进行配置

        server{
          return 404;
        }

4. dns配置是支持通配符的，例如 *.example.com 就会匹配所有域名
    如果有单个域名的配置，优先返回单个域名的配置。


这样一个简单的单服务器的域名配置就完成了.
