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
'''text
[[sssd[krb5_child[42521]]]] [unpack_buffer] (0x1000): total buffer size: [156]
[[sssd[krb5_child[42521]]]] [tgt_req_child] (0x1000): Attempting to get a TGT
[sssd[krb5_child[42509]]]: Disk quota exceeded
[sssd[krb5_child[42509]]]: Disk quota exceeded
'''

`cat /proc/key-users`
'''
10846:   206 206/206 205/1000 4713/20000
'''
`cat /proc/keys`

fix

'''text
up */proc/sys/kernel/keys/maxkeys *(from default of 200 to 1000) 
'''


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



