## init ansible.cfg

```
# Since Ansible 2.12 (core):
# To generate an example config file (a "disabled" one with all default settings, commented out):
#               $ ansible-config init --disabled > ansible.cfg
# including existing plugins:
```
`#ansible-config init --disabled -t all > ansible.cfg`


# (boolean) Toggle Ansible logging to syslog on the target when it executes tasks. On Windows hosts this will disable a newer style PowerShell modules from writting to the event log.
;no_target_syslog=False
no_target_syslog=True

# (string) Path to the Python interpreter to be used for module execution on remote targets, or an automatic discovery mode. Supported discovery modes are ``auto`` (the default), ``auto_silent``, ``auto_legacy``, and ``auto_legacy_silent``. All discovery modes employ a lookup table to use the included system Python (on distributions known to include one), falling back to a fixed ordered list of well-known Python interpreter locations if a platform-specific default is not available. The fallback behavior will issue a warning that the interpreter should be set explicitly (since interpreters installed later may change which one is used). This warning behavior can be disabled by setting ``auto_silent`` or ``auto_legacy_silent``. The value of ``auto_legacy`` provides all the same behavior, but for backwards-compatibility with older Ansible releases that always defaulted to ``/usr/bin/python``, will use that interpreter if present.
;interpreter_python=auto
interpreter_python=/usr/bin/python3





build ansible
build python3
ansible_collections
whereis openssl
openssl version -a

OpenSSL 1.0.1e-fips 11 Feb 2013
built on: Wed Aug 14 16:32:19 UTC 2019
platform: linux-x86_64
options:  bn(64,64) md2(int) rc4(16x,int) des(idx,cisc,16,int) idea(int) blowfish(idx) 
compiler: gcc -fPIC -DOPENSSL_PIC -DZLIB -DOPENSSL_THREADS -D_REENTRANT -DDSO_DLFCN -DHAVE_DLFCN_H -DKRB5_MIT -m64 -DL_ENDIAN -DTERMIO -Wall -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector --param=ssp-buffer-size=4 -m64 -mtune=generic -Wa,--noexecstack -DPURIFY -DOPENSSL_IA32_SSE2 -DOPENSSL_BN_ASM_MONT -DOPENSSL_BN_ASM_MONT5 -DOPENSSL_BN_ASM_GF2m -DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DMD5_ASM -DAES_ASM -DVPAES_ASM -DBSAES_ASM -DWHIRLPOOL_ASM -DGHASH_ASM
OPENSSLDIR: "/etc/pki/tls"
engines:  rdrand dynamic 


openssl version -a
OpenSSL 1.0.2k-fips  26 Jan 2017
built on: reproducible build, date unspecified
platform: linux-x86_64
options:  bn(64,64) md2(int) rc4(8x,int) des(idx,cisc,16,int) idea(int) blowfish(idx) 
compiler: gcc -I. -I.. -I../include  -fPIC -DOPENSSL_PIC -DZLIB -DOPENSSL_THREADS -D_REENTRANT -DDSO_DLFCN -DHAVE_DLFCN_H -DKRB5_MIT -m64 -DL_ENDIAN -Wall -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches    -m64 -mtune=generic -Wa,--noexecstack -DPURIFY -DOPENSSL_IA32_SSE2 -DOPENSSL_BN_ASM_MONT -DOPENSSL_BN_ASM_MONT5 -DOPENSSL_BN_ASM_GF2m -DRC4_ASM -DSHA1_ASM -DSHA256_ASM -DSHA512_ASM -DMD5_ASM -DAES_ASM -DVPAES_ASM -DBSAES_ASM -DWHIRLPOOL_ASM -DGHASH_ASM -DECP_NISTZ256_ASM
OPENSSLDIR: "/etc/pki/tls"
engines:  rdrand dynamic 



rpm -ql python34
rpm -qc python34
rpm -qa python34
## update openssl for centos6.10
build openssl
./config --prefix=/usr --libdir=/usr/lib64 --openssldir=/usr/local/ssl zlib

make distclean
## install python2.7.10 for centos6.10
edit Modules/Setup.dist for SSL
./configure --enable-optimizations  --enable-unicode=ucs4 --enable-shared --prefix=/usr


## install python3.7 for centos7
`./configure --prefix=/usr --enable-optimizations --with-system-ffi --with-ssl-default-suites=openssl --with-openssl=/usr/include/openssl`
make altinstall

## node run docker-compose
`pip install -i https://pypi.example.com/root/pypi/ docker docker-compose`

## proxy pypi
`devpi-server --host=0.0.0.0 --port=13141`

---
- hosts: "{{ variable_hosts }}"

  tasks:
    - name: Create a ext4 filesystem on /dev/nvme1n1
      filesystem:
        fstype: ext4
        dev: /dev/nvme1n1
    
    - name: Mount a volume
      mount:
        path: /data
        src: /dev/nvme1n1
        fstype: ext4
        state: mounted
{
  "data-root": "/data/docker",
  "selinux-enabled": true,
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3",
    "compress": "true"
  }
}
---
- hosts: "{{ variable_hosts }}"

  tasks:
    - name: upgrade all packages
      yum:
        name: '*'
        update_cache: yes
        state: latest

    - name: install latest version of docker
      shell: sudo amazon-linux-extras install docker selinux-ng -y

    - name: install container-selinux
      yum:
        name: 'container-selinux'
        state: latest

    - name: Enable SElinux
      selinux:
        policy: targeted
        state: enforcing

    - name: update docker configure
      copy:
        src: docker-daemon.json
        dest: /etc/docker/daemon.json
        owner: root
        group: root
        mode: 644
      notify:
        - restart docker

    - name: change security contexts
      shell: /usr/bin/chcon -R -t container_var_lib_t /data/docker && /usr/bin/chcon -R -t container_share_t /data/docker/overlay2
      notify:
        - restart docker

  handlers:
    - name: restart docker
      service:
        name: docker
        enabled: yes
        state: restarted