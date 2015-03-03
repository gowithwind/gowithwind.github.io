#部署tornado应用
>2015-03-03

1. apt-get update
1. apt-get install git nginx
1. git clone *git地址*
1. pip install -r requestments.txt
1. pip install supervisor
1. 编写nginx配置文件,并链接到/etc/nginx/site-enabled

upstream frontends {
    server 127.0.0.1:1234; #supervisor¿ØÖÆµÄÓòÃû
}
server {
    listen 80;  #¼àÌý¶Ë¿Ú
    server_name low-poly.1day1app.com;
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

1. echo_supervisord_conf >super.conf
1. 编辑super.conf 添加应用配置内容

  [program:tornado_app]
  command=python app.py --port=1234
  startsecs=0
  stopwaitsecs=0
  autostart=true
  autorestart=true
  stdout_logfile=/var/log/app.log
  stderr_logfile=/var/log/app.err

1. supervisord -c super.conf
1. nginx -s reload

部署完成
