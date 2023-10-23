# ToolsList

- [日志处理]
    - [Grok Exporter](https://github.com/fstab/grok_exporter)
    - [mtail](https://github.com/google/mtail)
- [正则表达式]
    - [RE2](https://github.com/google/re2/wiki/Syntax)
- [用户环境]
    - [Environment Modules](http://modules.sourceforge.net/)[doc](https://modules.readthedocs.io/en/stable/index.html)
- [镜像]
    - [Win10](https://www.microsoft.com/zh-cn/software-download/windows10ISO)
    - [license](https://www.microsoft.com/licensing/servicecenter/default.aspx)
    - [Lenove Recovery](https://support.lenovo.com/hk/en/solutions/ht103653) [Lenove Recovery](https://pcsupport.lenovo.com/hk/zc/lenovorecovery)
- [配置中心]
    + [Apollo]
    + [consul]
- [MYSQL web 端 SQL 审核平台]
    + [Yearning](https://github.com/cookieY/Yearning)
- [CMDBulid]
    + [CMDBuild](https://www.cmdbuild.org/en)
- [OCSNG]
    + [OCSNG](https://ocsinventory-ng.org/?lang=en)
- [GLPI]
    + [GLPI]() 
- [Time-Tracking 时间跟踪]
    + [Kimai](https://www.kimai.org/)
    + [hamster](https://github.com/projecthamster/hamster)
- [项目管理]
    + [Planner](https://wiki.gnome.org/Apps/Planner)
- [流程图]
    + [drawio](https://www.diagrams.net/)
- [文件系统]
    + [lustre文件系统](http://wiki.lustrefs.cn/index.php?title=Lustre%E4%BB%8B%E7%BB%8D)
- [基础架构]
    + [terraform](https://www.terraform.io/docs)
- [私有仓库]
    + [nexus](https://help.sonatype.com/repomanager3/product-information/download)
    + [rust crates-io-proxy](https://crates.io/crates/crates-io-proxy)
    + [pypi proxy devpi-server]
- [MySQL]
    + [Mysql authentication credentials](https://dev.mysql.com/doc/refman/8.0/en/mysql-config-editor.html)
    + [resetting-permission](https://dev.mysql.com/doc/refman/5.6/en/resetting-permissions.html)
- [Kubernetes]
    + [Ranch](https://docs.ranchermanager.rancher.io/)
    + [Wayne](https://github.com/Qihoo360/wayne])
- [ChatOps]
    + [HipChat]
    + [Slack]
    + [MatterMost]
    + [Zulip]
- [配置管理系统]
    + [Ansible]
    + [Salt]
    + [Puppet]
    + [Chef]
- [持续集成]
    + [Jenkins]
    + [Bamboo]
- [IaC基础设施即代码]
    + [Packer]
    + [Terraform]
- [文件同步]
    + [csync2](https://github.com/LINBIT/csync2/blob/master/doc/csync2.adoc)
    + [lsyncd]
- [cloud]
    + [cloud-init](https://cloudinit.readthedocs.io/en/19.2/topics/examples.html)
    + [cloud-utils-growpart]
- [DevOps]
    + [devops-toolchain](https://www.opsera.io/learn/beginners-devops-toolchain)
    + [security-devops](https://www.opsera.io/learn/integrate-security-to-devops-toolchain)
    + [testing-devops](https://www.opsera.io/learn/continuous-testing-devops)
    + [ci-cd-pipeline](https://www.opsera.io/ci-cd-pipeline)
    + [devops-tools](https://www.opsera.io/blog/best-devops-tools)
    + [IaC](https://www.opsera.io/learn/10-infrastructure-as-code-tools-for-automating-deployments-in-2022)
- [SysOS]
    + [pentoo](https://pentoo.org/downloads)
    + [parrotsec](https://www.parrotsec.org/download/)
    + [kali.org](https://www.kali.org/get-kali/#kali-platforms)
    
- [sketch]


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

# ansible
ansible-playbook -i /opt/ansible/haproxy_aws/hosts /opt/ansible/haproxy_aws/main.yml --limit 10.254.9.10
