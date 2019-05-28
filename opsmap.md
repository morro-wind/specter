* 初級——管理員
  * 
  * 系統目錄結構
  * lsusb, lspci, etc
    /sys/
    /proc/
    /dev/
    modprobe
    lsmod
    lspci
    lsusb
  * 系統啓動過程
    * dmesg
    * journalctl
    * systemd
    * BIOS
    * SysVinit
    * UEFI
    * init
    * bootloader
    * initramfs
    * kernel
  * 運行級別、啓動、重啓、關機
    * /etc/inittab
    * shutdown
    * init
    * /etc/init.d/
    * telinit
    * systemd
    * systemctl
    * /etc/systemd/
    * /usr/lib/systemd/
    * wall
  * linux 包管理
    * / (root) filesystem
    * /var filesystem
    * /home filesystem
    * /boot filesystem
    * EFI System Partition (ESP)
    * swap space
    * mount points
    * partitions

menu.lst, grub.cfg and grub.conf
grub-install
grub-mkconfig
MBR

ldd
ldconfig
/etc/ld.so.conf
LD_LIBRARY_PATH

/etc/apt/sources.list
dpkg
dpkg-reconfigure
apt-get
apt-cache

rpm
rpm2cpio
/etc/yum.conf
/etc/yum.repos.d/
yum
zypper


Virtual machine
Linux container
Application container
Guest drivers
SSH host keys
D-Bus machine id

bash
echo
env
export
pwd
set
unset
type
which
man
uname
history
.bash_history
Quoting

bzcat
cat
cut
head
less
md5sum
nl
od
paste
sed
sha256sum
sha512sum
sort
split
tail
tr
uniq
wc
xzcat
zcat

cp
find
mkdir
mv
ls
rm
rmdir
touch
tar
cpio
dd
file
gzip
gunzip
bzip2
bunzip2
xz
unxz
file globbing

tee
xargs

&
bg
fg
jobs
kill
nohup
ps
top
free
uptime
pgrep
pkill
killall
watch
screen
tmux

nice
ps
renice
top

grep
egrep
fgrep
sed
regex(7)

vi
/, ?
h,j,k,l
i, o, a
d, p, y, dd, yy
ZZ, :w!, :q!
EDITOR

Manage MBR and GPT partition tables
Use various mkfs commands to create various filesystems such as:
ext2/ext3/ext4
XFS
VFAT
exFAT

fdisk
gdisk
parted
mkfs
mkswap


du
df
fsck
e2fsck
mke2fs
tune2fs
xfs_repair
xfs_fsr
xfs_db


/etc/fstab
/media/
mount
umount
blkid
lsblk

chmod
umask
chown
chgrp


ln
ls

find
locate
updatedb
whereis
which
type
/etc/updatedb.conf




* 中級——工程師

* Tools and software list



