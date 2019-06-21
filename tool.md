## xmllint \(libxml2-utils\)

检查XML有效性

编辑XML时，最好使用支持XML的编辑器来确保语法正确并且XML格式正确。您还可以使用该`xmllint`实用程序检查XML是否格式正确。默认情况下，`xmllint`重新流动并将XML打印到标准输出。要检查格式是否正确并且仅在出现错误时打印输出，请使用该命令`xmllint -noout filename.xml`。

##### Git Parameter 参数化构建过程 {#3}

# CentOS 7下搭建高可用集群（pcs，pacemaker，corosync，fence-agents-all） {#article_title}

[https://linux.cn/article-3963-1.html](https://linux.cn/article-3963-1.html)

[https://access.redhat.com/documentation/zh-cn/red\_hat\_enterprise\_linux/7/html/high\_availability\_add-on\_reference/s1-pcsfullconfig-HAAR](https://access.redhat.com/documentation/zh-cn/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/s1-pcsfullconfig-HAAR)

[https://www.ibm.com/developerworks/cn/linux](https://www.ibm.com/developerworks/cn/linux)

# Linux 集群大全 {#ibm-pagetitle-h1}

[https://www.ibm.com/developerworks/cn/linux/cluster/lw-clustering.html](https://www.ibm.com/developerworks/cn/linux/cluster/lw-clustering.html)

[https://www.ibm.com/developerworks/cn/linux/cluster/index.html](https://www.ibm.com/developerworks/cn/linux/cluster/index.html)

## curl

-H "Accept-Encoding: gzip,deflate" 添加壓縮請求頭

-IL/iL  顯示Content-Encoding: gzip響應

curl -G -d "username=name&password=pwd&verificationCode=1111&type=saveGateway" -IL -H "Accept-Encoding: gzip,deflate" 127.0.0.1

curl -d "username=name&password=pwd&verificationCode=1111&type=saveGateway" -iL -H "Accept-Encoding: gzip,deflate" 127.0.0.1

## postgresql

\# CLOSE DATABASE CONNECT

$BIN -h $HOST -U $USER -d postgres -c "SELECT pg\_terminate\_backend\(pg\_stat\_activity.pid\) FROM pg\_stat\_activity WHERE datname='$DB1' AND pid&lt;&gt;pg\_backend\_pid\(\);"

## GoCD

## ansible-playbook

host alias, group_vars, roles takes, files, import\_playbook, vars\_\_files\_

# heka 配置 一个go语言实现轻量级logstash 干掉ELK

# unison
## unison
http://www.cis.upenn.edu/~bcpierce/unison/download/releases/stable/unison-manual.html#profile
https://github.com/bcpierce00/unison/releases
## ocaml:http://caml.inria.fr/


