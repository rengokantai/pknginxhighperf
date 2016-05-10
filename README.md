#### pknginxhighperf
#####3
######configuring nginx workers
get cpuinfo
```
lscpu
cat /proc/cpuinfo | grep 'precessor' |wc -l
```
(e)
```
accept_mutex on;  //if off,only one worker will process the connection
```
(e)
```
accept_mutex_delay 500ms;
```
check number of descriptors
```
ulimit -n
```
setting the available file descriptor
```
worker_rlimit_nofile 20960;
```
accept as many as possible when it gets the notification of a new connection
```
multi_accept on;
```
how to process connections? pocess multiple socket streams without getting stuck
```
events{
  use select/poll(--without-poll_module)  / kqueue(freebsd4.1,osx)/epoll
}
```
######Configuring nginx i/o
sendfile (h s l)
```
sendfile on;// default off
```
######configuring tcp
(h s l)
```
tcp_nodelay on;
```
(h s l)The option tells the tcp stack to append packets and send them when the app instructs to send the packet by explicitly removing TCP_CORK 
```
tcp_nopush on;
```


#####4 Controlling buffers
######configuring buffers
request body
```
client_body_buffer_size 8k  (default 8k,32bit system, 16k, 64bitsystem)
```
If client body is too large, it will return 413
```
server{
  client_max_body_size 2m;
}
```

only save request body in temporary file
```
client_body_in_file_only on/off/clean;
```
store complete request body in single buffer
```
client_body_in_single_buffer on;
```
save files in temp path
```
client_body_temp_path filepath 1 2
```
allocate a buffer for request header
```
client_header_buffer_size 1k
```
If header does not fit into a specified buffer,use
```
large_client_header_buffers 4 8k
```
######configuring timeouts
(h s l) (may have a second param,end back to Keep-Alive header)
```
keepalive_timeout 20s (18s);  
```
max requests during a keep-alive (h s l)
```
keepalive_requests 20;
```
```
keepalive_disabled msie6 safari;
```
(h s l)If the timeout expires, nginx will close the connection.(does not apply to the entire transfer but only between two successive write operations)
```
send_timeout 30s
```
bodytimeout. 408error(request timed out)
```
client_body_timeout 30s;
```
sets a timeout to send a complete request header from the client
```
client_header_timeout 30s;
```
######compression
basic
```
gzip on;
```
level (1-3)
```
gzip_comp_level 3;
```
min length (in bytes)
```
gzip_min_length 1000;
```
limit gzip types
```
gzip_types text/xml text/css text/plain;
```
what kind of response should be compressed (for example,this enables compression if cache-control header field has the value no-cache)
```
gzip_proxied no-cache expired
```
```
gzip_http_version 1.1
```

```
gzip_vary on;
```
disable gzip in some browser
```
gzip_disable "MSIE [1-6]\."
```
```
gzip_static always;
```

######configuring logs
(h s l)
```
access_log logs/access/log combined;
log_format combined '$remote_addr - $remote_user [$time_local]'  '"$request" $status $body_bytes_sent ' '"$http_referer" "$http_user_agent"';
```
(h s l)
```
log_subrequest on;
```
error + level
```
error_log logs/warn.log warn;
```
```
log_not_found on;
```


```
#####6 Using nginx Cache
######cache static content
```
open_file_cache max=1000 inactive=20s;
open_file_cache_valid 60s;
open_file_cache_min_uses 5;
open_file_cache_errors off;
```
######cache dynamic content
install php
```
tar -xvf php-5.tar.gz
cd php-5
./configure --enable-fpm -q
make --quiet
make install
```
Or install via apt
```
apt-get install php5 php5-fpm
```

