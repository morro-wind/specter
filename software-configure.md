## nginx alias configure php

Your nested`location ~ \.php$`block will not find your PHP scripts. The`$document_root`is set to`/path/to/php-app`. And`$fastcgi_script_name`is the same as`$uri`which still includes the`/app`prefix.

The correct approach is to use`$request_filename`and remove your bogus`root`statement:

```
location ^~ /app {
    alias   /path/to/php-app;
    index  index.php;

    location ~ \.php$ {
        fastcgi_pass   127.0.0.1:9000;
        include        fastcgi_params;
        fastcgi_param  SCRIPT_FILENAME $request_filename;
    }
}
```

Always include`fastcgi_params`before any`fastcgi_param`statements to avoid them being silently overwritten by the contents of the include file. See[this document](http://nginx.org/en/docs/http/ngx_http_core_module.html#var_request_filename)for details.

## nginx rewrite

`http://nginx.org/en/docs/http/converting_rewrite_rules.html`

## nginx wordpress rewrite

```
location / {
    if (-f $request_filename/index.html){
        rewrite (.*) $1/index.html break;
    }
    if (-f $request_filename/index.php){
        rewrite (.*) $1/index.php;
    }
    if (!-f $request_filename){
        rewrite (.*) /index.php;
    }
}
```

## nginx rewrite http to https

```
#if ($scheme = "http") {
#    rewrite (.*)$ https://$http_host$request_uri/$1 permanent;
#}
rewrite ^(.*)$ https://$http_host$request_uri$1 permanent;
```

### nginx wiki configure

`https://www.nginx.com/resources/wiki/start/`

## keepalived float ip on aws ec2

通过keepalived 状态变更为master 时，执行notify\_master 对应脚本将floatip 分配给自己

```
global_defs {
  script_user ec2-user
  enable_script_security
}

vrrp_script check_haproxy {
  script "/sbin/pidof haproxy"
  interval 5
  fall 2
  rise 2
  weight -30
  user ec2-user
}

vrrp_instance VI_1 {
  state BACKUP
  interface eth0
  smtp_alert
  virtual_router_id 51
  priority 100
  unicast_src_ip 10.240.1.20

  unicast_peer {
      10.240.1.28
  }

  advert_int 1
  authentication {
      auth_type PASS
      auth_pass 1111
  }

  track_script {
      check_haproxy
  }

  notify_master  "/etc/keepalived/failover.sh"
}
```

```
#!/bin/bash

PIP=10.240.1.30
INTERFACE_ID=eni-f65555

/usr/bin/aws ec2 assign-private-ip-addresses --allow-reassignment --network-interface-id \
$INTERFACE_ID --private-ip-addresses $PIP
```

# filebeat

```
multiline.max_lines
multiline.pattern: '^\['
multiline.negate: true
multiline.match: after
```

```
filter {
    if [type] == "res-shopping-info" {
        multiline {
            pattern => "^\["
            negate => true
            what => "previous"
        }
        grok {
            match => {
                "message" => "(?m)\[%{LOGLEVEL:loglevel}\] \[%{TIMESTAMP_ISO8601:time}\] \[%{JAVAFILE:class}\] \| (?<info>([\s\S]*))"
            }
           overwrite => ["message"]
        }
    }
```

```
    codec => multiline {
        max_bytes => "10MiB"
        max_lines => 500
        charset => "GBK"
        pattern => "^(?!.*?=== >>>>>>>> ===).*$"
        what => "previous"
    }
```

# kibana

[https://www.elastic.co/guide/cn/kibana/current/advanced-options.html](https://www.elastic.co/guide/cn/kibana/current/advanced-options.html)

## 修改默认显示列

defaultColumns  
默认值是 \_source 。定义“发现”标签页上默认显示的列

## 修改默認單元格顯示默認高度

truncate:maxHeight  
这个属性指定了表格中单元格显示时占用的最大高度，设置为0则不限制。



yumdownloader --resolve(可选，意为下依赖包) --destdir=软件存放位置 (可选) +软件包名


issue: The certificate of is not trusted
yum install ca-certificates

## build haproxy
```
# yum install make openssl-devel pcre2-devel systemd-devel pcre2-static
# make -j $(nproc) TARGET=linux-glibc USE_OPENSSL=1 USE_PCRE2=1 USE_PCRE2_JIT=1 USE_SYSTEMD=1 USE_THREAD=1 USE_STATIC_PCRE2=1 USE_TFO=1 USE_EPOLL=1 USE_LINUX_TPROXY=1
# make install PREFIX=/opt/haproxy
```


## nginx log format

```
    log_format  main  '$remote_addr - $remote_user [$time_local] $http_host "$request" '
                      '"$proxy_protocol_addr" $status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"'
                      '$upstream_addr $upstream_status $upstream_response_time"m" $request_time"m" '
                      '"$ssl_protocol" "$ssl_cipher" "$gzip_ratio" ';
```