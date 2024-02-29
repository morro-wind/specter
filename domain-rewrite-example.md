## domain rewrite

多域名重定向到不同的二级域名

需求是公司有多个二级域名，现在需要重定向到不同的二级域名上，最初的想法是用不同的if判定，后面太难维护，写出相关变量

```
listen 80;
server_name 1.liepass.com 2.liepass.com;

if ( $host ~* ^(.*).liepass.com )
{
	set $host_pre $1;
	rewrite ^(.*)$ https://$host_pre.morro-wind.com;
}
```


注解：判定host是否包含相关二级域名，有的话传相关参数

1、^： 匹配字符串的开始位置；

2、 $：匹配字符串的结束位置；

3、.: .匹配任意字符，匹配数量0到正无穷；

4、. 斜杠用来转义，.匹配 . 特殊使用方法，记住记性了；

5、（值1|值2|值3|值4）：或匹配模式，例：（jpg|gif|png|bmp）匹配jpg或gif或png或bmp

6、i不区分大小写

　

一．正则表达式匹配，其中：

~ 为区分大小写匹配
~* 为不区分大小写匹配
!~和!~*分别为区分大小写不匹配及不区分大小写不匹配
二．文件及目录匹配，其中：
-f和!-f用来判断是否存在文件
-d和!-d用来判断是否存在目录
-e和!-e用来判断是否存在文件或目录
-x和!-x用来判断文件是否可执行
三．rewrite指令的最后一项参数为flag标记，flag标记有：
1.last 相当于apache里面的[L]标记，表示rewrite。
2.break本条规则匹配完成后，终止匹配，不再匹配后面的规则。
3.redirect 返回302临时重定向，浏览器地址会显示跳转后的URL地址。
4.permanent 返回301永久重定向，浏览器地址会显示跳转后的URL地址。

使用last和break实现URI重写，浏览器地址栏不变。而且两者有细微差别，使用alias指令必须用last标记;使用proxy_pass指令时，需要使用break标记。Last标记在本条rewrite规则执行完毕后，会对其所在server{......}标签重新发起请求，而break标记则在本条规则匹配完成后，终止匹配。
例如：如果我们将类似URL/photo/123456 重定向到/path/to/photo/12/1234/123456.png
rewrite "/photo/([0-9]{2})([0-9]{2})([0-9]{2})"/path/to/photo/1/12/123.png ;

四．NginxRewrite 规则相关指令

1.break指令
使用环境：server,location,if;
该指令的作用是完成当前的规则集，不再处理rewrite指令。

2.if指令
使用环境：server,location
该指令用于检查一个条件是否符合，如果条件符合，则执行大括号内的语句。If指令不支持嵌套，不支持多个条件&&和||处理。

3.return指令
语法：returncode ;
使用环境：server,location,if;
该指令用于结束规则的执行并返回状态码给客户端。
示例：如果访问的URL以".sh"或".bash"结尾，则返回403状态码
location ~ .*.(sh|bash)?$
{
return 403;
}

4.rewrite 指令
语法：rewriteregex replacement flag
使用环境：server,location,if
该指令根据表达式来重定向URI，或者修改字符串。指令根据配置文件中的顺序来执行。注意重写表达式只对相对路径有效。如果你想配对主机名，你应该使用if语句，示例如下：
if( $host ~ www.(.) )
{
set hostwithoutwww1;
rewrite ^(.*)http://host_without_www$1permanent;
}

5.Set指令
语法：setvariable value ; 默认值:none; 使用环境：server,location,if;
该指令用于定义一个变量，并给变量赋值。变量的值可以为文本、变量以及文本变量的联合。
示例：set$varname "hello world";

6.Uninitialized_variable_warn指令
语法：uninitialized_variable_warnon|off
使用环境：http,server,location,if
该指令用于开启和关闭未初始化变量的警告信息，默认值为开启。

五．Nginx的Rewrite规则编写实例
1.当访问的文件和目录不存在时，重定向到某个php文件
if( !-e $request_filename )
{
rewrite ^/(.*)$ index.php last;
}

2.目录对换 /123456/xxxx ====> /xxxx?id=123456
rewrite ^/(d+)/(.+)/ /2?id=1 last;

3.如果客户端使用的是IE浏览器，则重定向到/ie目录下
if( $http_user_agent ~ MSIE)
{
rewrite ^(.*)/ie/1 break;
}

4.禁止访问多个目录
location ~ ^/(cron|templates)/
{
deny all;
break;
}

5.禁止访问以/data开头的文件
location ~ ^/data
{
deny all;
}

6.禁止访问以.sh,.flv,.mp3为文件后缀名的文件
location ~ .*.(sh|flv|mp3)$
{
return 403;
}

7.设置某些类型文件的浏览器缓存时间
location ~ .*.(gif|jpg|jpeg|png|bmp|swf)expires30d;location .∗.(js|css)
{
expires 1h;
}

8.给favicon.ico和robots.txt设置过期时间;
这里为favicon.ico为99天,robots.txt为7天并不记录404错误日志
location ~(favicon.ico) {
log_not_found off;
expires 99d;
break;
}
location ~(robots.txt) {
log_not_found off;
expires 7d;
break;
}

9.设定某个文件的过期时间;这里为600秒，并不记录访问日志
location ^~ /html/scripts/loadhead_1.js {
access_log off;
root /opt/lampp/htdocs/web;
expires 600;
break;
}

10.文件反盗链并设置过期时间
这里的return412 为自定义的http状态码，默认为403，方便找出正确的盗链的请求
“rewrite ^/ http://img.linuxidc.net/leech.gif;”显示一张防盗链图片
“access_log off;”不记录访问日志，减轻压力
“expires 3d”所有文件3天的浏览器缓存

location ~*^.+.(jpg|jpeg|gif|png|swf|rar|zip|css|js)$ {
valid_referers none blocked .linuxidc.com.linuxidc.net localhost 208.97.167.194;
if ($invalid_referer) {
rewrite ^/ http://img.linuxidc.net/leech.gif;
return 412;
break;
}
access_log off;
root /opt/lampp/htdocs/web;
expires 3d;
break;
}

11.只允许固定ip访问网站，并加上密码

root /opt/htdocs/www;
allow 208.97.167.194; 
allow 222.33.1.2; 
allow 231.152.49.4;
deny all;
auth_basic “C1G_ADMIN”;
auth_basic_user_file htpasswd;

12将多级目录下的文件转成一个文件，增强seo效果
/job-123-456-789.html 指向/job/123/456/789.html

rewrite^/job-([0-9]+)-([0-9]+)-([0-9]+).html/job/1/Missing superscript or subscript argumentMissing superscript or subscript argument3.html last;

13.文件和目录不存在的时候重定向：

if (!-e $request_filename) {
proxy_pass http://127.0.0.1;
}

14.将根目录下某个文件夹指向2级目录
如/shanghaijob/ 指向 /area/shanghai/
如果你将last改成permanent，那么浏览器地址栏显是/location/shanghai/
rewrite ^/([0-9a-z]+)job/(.*)/area/1/上面例子有个问题是访问时将不会匹配2last;上面例子有个问题是访问/shanghai时将不会匹配rewrite/([0−9a−z]+)job /area/1/last;rewrite/([0−9a−z]+)job/(.∗) /area/1/2last;
这样/shanghai 也可以访问了，但页面中的相对链接无法使用，
如./list_1.html真实地址是/area/shanghia/list_1.html会变成/list_1.html,导至无法访问。
那我加上自动跳转也是不行咯
(-d 它有个条件是必需为真实目录，而我的不是的，所以没有效果requestfilename)它有个条件是必需为真实目录，而我的rewrite不是的，所以没有效果if(−drequest_filename){
rewrite ^/(.*)(1)http://host/12/permanent;
}
知道原因后就好办了，让我手动跳转吧
rewrite ^/([0-9a-z]+)job/1job/permanent;
rewrite ^/([0-9a-z]+)job/(.*)/area/1/$2last;

15.域名跳转
server
{
listen 80;
server_name jump.linuxidc.com;
index index.html index.htm index.php;
root /opt/lampp/htdocs/www;
rewrite ^/ http://www.linuxidc.com/;
access_log off;
}

16.多域名转向
server_name www.linuxidc.comwww.linuxidc.net;
index index.html index.htm index.php;
root /opt/lampp/htdocs;
if ($host ~ "linuxidc.net") {
rewrite ^(.*) http://www.linuxidc.com$1permanent;
}

六．nginx全局变量
arg_PARAMETER #这个变量包含GET请求中，如果有变量PARAMETER时的值。
args #这个变量等于请求行中(GET请求)的参数，如：foo=123&bar=blahblah;
binary_remote_addr #二进制的客户地址。
body_bytes_sent #响应时送出的body字节数数量。即使连接中断，这个数据也是精确的。
content_length #请求头中的Content-length字段。
content_type #请求头中的Content-Type字段。
cookie_COOKIE #cookie COOKIE变量的值
document_root #当前请求在root指令中指定的值。
document_uri #与uri相同。
host #请求主机头字段，否则为服务器名称。
hostname #Set to themachine’s hostname as returned by gethostname
http_HEADER
is_args #如果有args参数，这个变量等于”?”，否则等于”"，空值。
http_user_agent #客户端agent信息
http_cookie #客户端cookie信息
limit_rate #这个变量可以限制连接速率。
query_string #与args相同。
request_body_file #客户端请求主体信息的临时文件名。
request_method #客户端请求的动作，通常为GET或POST。
remote_addr #客户端的IP地址。
remote_port #客户端的端口。
remote_user #已经经过Auth Basic Module验证的用户名。
request_completion #如果请求结束，设置为OK. 当请求未结束或如果该请求不是请求链串的最后一个时，为空(Empty)。
request_method #GET或POST
request_filename #当前请求的文件路径，由root或alias指令与URI请求生成。
request_uri #包含请求参数的原始URI，不包含主机名，如：”/foo/bar.php?arg=baz”。不能修改。
scheme #HTTP方法（如http，https）。
server_protocol #请求使用的协议，通常是HTTP/1.0或HTTP/1.1。
server_addr #服务器地址，在完成一次系统调用后可以确定这个值。
server_name #服务器名称。
server_port #请求到达服务器的端口号。

七．Apache和Nginx规则的对应关系
Apache的RewriteCond对应Nginx的if
Apache的RewriteRule对应Nginx的rewrite
Apache的[R]对应Nginx的redirect
Apache的[P]对应Nginx的last
Apache的[R,L]对应Nginx的redirect
Apache的[P,L]对应Nginx的last
Apache的[PT,L]对应Nginx的last

例如：允许指定的域名访问本站，其他的域名一律转向www.linuxidc.net
Apache:
RewriteCond %{HTTP_HOST} !^(.*?).aaa.com[NC]RewriteCond 
RewriteCond %{HTTP_HOST}!^192.168.0.(.*?)RewriteRule/(.∗) http://www.linuxidc.net[R,L]

Nginx:
if( host ∗(.∗)\.aaa\.com )
{
set Extra close brace or missing open braceExtra close brace or missing open bracehost ~* ^localhost )
{
set Extra close brace or missing open braceExtra close brace or missing open bracehost ~* ^192\.168\.1\.(.*?))set$allowHost‘1′;if(allowHost !~ ‘1’ )
{
rewrite ^/(.*)$ http://www.linuxidc.netredirect ;
}