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
#    rewrite (.*)$ https://$http_host$request_uri/$1 permanent; }
rewrite ^(.*)$ https://$http_host$request_uri$1 permanent;
```

### nginx wiki configure

`https://www.nginx.com/resources/wiki/start/`

