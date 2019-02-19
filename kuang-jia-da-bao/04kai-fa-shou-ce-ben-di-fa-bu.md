1. 安装node.js，再执行npm install gitbook-cli -g，安装成功完成后就搞定了。
2. 然后每次在gitbook editor中同步更新好文档后，在文档windows所在目录cmd进入命令行，执行gitbook serve，等服务启动，再ctrl+c停止服务，去文档所在目录就会看到有一个\_book目录，里面就是静态页面了。
3. 部署，现在部署目录是在公司服务器84上，使用nginx部署静态资源，安装配置信息，详见《公司网站维护手册》，下面重点说明nginx的配置

```
#配置文件：/usr/local/nginx/nginx.conf
 server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

       #公司网站
        location / {
        proxy_pass http://127.0.0.1:90;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        #手册地址映射
        location /dev_guide/content  {
          alias  /home/soeasy/apps/apache-tomcat-7.0.91/webapps/shouyi_web_mysql/dev_guide/;
          index index.html;
        }

 }
```



