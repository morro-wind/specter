# issuse

yum install epel-release
yum install perl-Module-Install
You are visiting the local directory
  '/root/src/fusioninventory-agent-2.6/.'
  without lock, take care that concurrent processes do not do likewise.
Running make for /root/src/fusioninventory-agent-2.6/.

  CPAN.pm: Building /root/src/fusioninventory-agent-2.6/.

Can't locate inc/Module/Install.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .) at Makefile.PL line 8.
BEGIN failed--compilation aborted at Makefile.PL line 8.
Warning: No success on command[/usr/bin/perl Makefile.PL]
Directory '/root/src/fusioninventory-agent-2.6/.' not below /root/.cpan/build, will not store persistent state
  /root/src/fusioninventory-agent-2.6/.
  /usr/bin/perl Makefile.PL -- NOT OK

## gitlab

### This user cannot be unlocked manually from GitLab

```
# gitlab-rails console
  Loading production environment (Rails 4.2.7.1)

  irb(main):001:0> user = User.find_by_email("email@something.com")

  => .... output snipped ...

irb(main):002:0> user.state = "active"
=> "active"
irb(main):003:0> user.save
=> true
irb(main):004:0> exit
```



perl Makefile.PL 
Can't locate inc/Module/Install.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .) at Makefile.PL line 8.
BEGIN failed--compilation aborted at Makefile.PL line 8.



## sssd

issuse
https://www.kernel.org/doc/Documentation/security/keys.txt
https://www.mail-archive.com/sssd-users@lists.fedorahosted.org/msg07597.html
```text
[[sssd[krb5_child[42521]]]] [unpack_buffer] (0x1000): total buffer size: [156]
[[sssd[krb5_child[42521]]]] [tgt_req_child] (0x1000): Attempting to get a TGT
[sssd[krb5_child[42509]]]: Disk quota exceeded
[sssd[krb5_child[42509]]]: Disk quota exceeded
```


`cat /proc/key-users`
```
10846:   206 206/206 205/1000 4713/20000
```

`cat /proc/keys`

fix

```text
up */proc/sys/kernel/keys/maxkeys *(from default of 200 to 1000) 
```


sec

ps aux | grep scs
/etc/tcsd.preload 
/usr/local/lib/lmnettesting.so
ld.so.preload、crct10dif_intel.so、virtio_helper

### sssd 密码认证失败
secure log :pam_sss(sshd:auth): received for user $USERNAME: 4 (System error)

ssh clinet -vvv
Permission denied, please try again.

/tmp 目录权限错误

## x11 forwarding request failed on channel 0
ERROR: Unable to open X display

edit sshd_config modify X11MaxDisplays value
X11MaxDisplays
       Specifies the maximum number of displays available for sshd(8)'s X11 forwarding.  This prevents sshd from exhausting local ports.  The
       default is 1000.



## Failed to initialize credentials using keytab [MEMORY:/etc/krb5.keytab]: Preauthentication failed. Unable to create GSSAPI-encrypted LDAP connection.
https://docs.microsoft.com/en-us/answers/questions/570467/linux-server-join-to-ad-using-sssd-the-linux-serve.html
https://lists.fedoraproject.org/archives/list/freeipa-users@lists.fedorahosted.org/thread/REMZAIDC2DXFCFT5ZAOO4ZZAWQXGPMAA/

`# sssctl domain-status testlab.local`
`ipa-getkeytab`


https://sssd.io/release-notes/sssd-2.4.0.html



### ERR_SSL_PROTOCOL_ERROR
域名访问ERR_SSL_PROTOCOL_ERROR，域名未备案，tls 加密算法无法通过拦截
`ssl_ciphers         ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-SHA384:CDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:AES128-GCM-SHA256:ES128-SHA:AES256-SHA256:AES256-SHA;`


## git pull 冲突 error: Your local changes to the following files would be overwritten by merge

git stash 

git stash list

git pull

git pop

git diff

git commit 

git push





nfsiostat
iftop
ifstat
iperf






## debug x11forward
- remote x11 gui 无操作时异常退出
    - bsub -Is -q ali_test -m ali-cm001 gvim,等待故障复现
    - 观察lsf 日志-无相关内容
    - 观察网络连接状态，发现反向sshd 连接
    - sshd TCPKeepAlive 配置会检查网络和连接主机状态，并在故障时中断连接
    - sshd ClientAliveInterval 会发送消息请求响应，ClientAliveCountMax 配置发送消息次数，达到次数后无数据会断开连接，0禁用连接终止
    

## git config
git config http.postBuffer 1048576000
git config https.postBuffer 1048576000