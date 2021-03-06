```bash
worker_processes  2;
worker_cpu_affinity 0101 1010;
error_log logs/error.log;
 
#配置Nginx worker进程最大打开文件数
worker_rlimit_nofile 65535;
 
user www www;
events {
    #单个进程允许的客户端最大连接数
    worker_connections  20480;
    #使用epoll模型
    use epoll;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    #sendfile        on;
    #keepalive_timeout  65;
    #访问日志配置
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
 
 
    #虚拟主机
    include /application/nginx/conf/extra/www.conf;
    include /application/nginx/conf/extra/blog.conf;
    include /application/nginx/conf/extra/bbs.conf;
    include /application/nginx/conf/extra/edu.conf;
    include /application/nginx/conf/extra/phpmyadmin.conf;
    include /application/nginx/conf/extra/status.conf;
 
    #nginx优化----------------------
    #隐藏版本号
    server_tokens on;
 
    #优化服务器域名的散列表大小 
    server_names_hash_bucket_size 64;
    server_names_hash_max_size 2048;
 
    #开启高效文件传输模式
    sendfile on;
    #减少网络报文段数量
    #tcp_nopush on;
    #提高I/O性能
    tcp_nodelay on;
 
    #连接超时 时间定义 默认秒 默认65秒
    keepalive_timeout 60;
    
    #读取客户端请求头数据的超时时间 默认秒 默认60秒
    client_header_timeout 15;
    
    #读取客户端请求主体的超时时间 默认秒 默认60秒
    client_body_timeout 15;
    
    #响应客户端的超时时间 默认秒 默认60秒
    send_timeout 25;
    
    include /etc/nginx/conf.d/*.conf;
}
```
