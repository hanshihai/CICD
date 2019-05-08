# Linux common commands

###git
```
git update-index --chmod=+x filepath      : add execute for scripts
gitk --follow [filename]                  : check git file history
git branch -va                           : show current branch info
git remote -v                            : show remote info
```

###grep
```
grep -rni keyword files      : search keyword in files
grep keyword | wc -l         : count lines that include the keyword
```

###rpm
```
rpm -qa | grep xxx          : list all package for xxx
rpm -e <package-name>      : uninstall package
```

###yum
```
yum search package      : search availd package to install
yum install package     : yum install package
```

###jar and tar
```
jar tvf xxx.jar               : list jar filesystem
jar xvf xxx.jar filename      : extract file from jar file

tar cvf xx.tar folder         : tar folder
tar xvf xx.tar                : untar to folder
tar czvf xx.tar.gz folder	  : tar gz folder
tar xzvf xx.tar.gz			  : ungz tar to folder
```

###split
```
split -d -a 3 -b 200M source prefix    : split source file to many small files
    -d : use digit, or character
    -a : how many digits, 3 means max number is 999
    -b : each package size
    prefix : new package name will start with the prefix
```

###logic volume
```
df -h     : show logic disk usage
fdisk -l  : show total hard disk space and partitions
pvs       : show physical disk space
lvextend -L +2G /dev/mapper/vg00-var : extend logical volume vg00-var
lvs       : show logical disk space
mount     : mount file systems 
resize2fs /dev/mapper/vg00-var : resize volume
unmount  folder : remove mount file system

du -sh *   : list folder size
```

###normal
```
uname -a                  : show os info
cat /etc/*-release        : show os name

netstat -anp | grep port   : show which process is using the port

ln -s target name         : create soft link to target
rm name                   : delete soft link
chown user file           : update file owner to user

scp file ip:/folder/                                : cp file into remote hostname folder
rsync -avr -e "ssh -i ~/id-rsa" file ip:/folder/    : sync file into remote hostanme folder

sudo su     : su to root

id userid  : show user info
useradd -g groupname username          : add a new user and attach to groupname
passwd username                        : unlock username with password
usermod -a -G groupname username      : add user into a second group 
usermod -u id username                : change user id
groupmod -g id groupname              : change group id
groupadd groupname                    : add a new group

getent passwd id | cut -d: -f1        : show id is null or link to some user
getent group id                       : show group id is null or link to some group

cut -d: -f1 /etc/group         : list all groups
cut -d: -f1 /etc/passwd        : list all users

su username                 : switch to another user login
```

###curl
```
curl -fL http://{site,host}.host[1-5].com -o "#1_#2"    : curl download files based on the name
```
###visudo
```
visudo , then add/remote    : enable/disable sudo for the user
username ALL=(ALL)   ALL
```
###process and thread limition
```
cat /proc/sys/kernel/pid_max
131072
cat /proc/sys/kernel/threads-max
62270

/etc/security/limits.conf
@user hard nproc number    --- user has the limition of process
@user soft nproc number    --- user has warning when process is touched the number
```

###domain

domain doesn't work, that hostname is not ok, but ip works
```
vi /etc/hosts , then add items
127.0.0.1    localhost
```

###firewall and iptable

######check firewall connection
```
nmap -p port host
nc -vz host port
```

######disable firewall

centos7
```
systemctl stop firewalld.service      (stop service)
systemctl disable firewalld.service   (remove service from auto-start after reboot)
```

selinux
```
vi /etc/selinux/config
SELINUX=disabled    (update content)
setenforce 0   (enable changes)
```

###### iptables
```
yum install iptables    : install iptables
iptables --list         : list iptables
iptables -I INPUT -p tcp --dport port -j ACCEPT/REJECT    : addd rule for input port
iptables -D INPUT -p tcp --dport port -j ACCEPT/REJECT    : remove rule for input port
```

###ip6
disable ip6
```
vi /etc/netconfig
#udp6       tpi_clts      v     inet6    udp     -       -
#tcp6       tpi_cots_ord  v     inet6    tcp     -       -
```

###nfs
```
yum search nfs
yum install ufs-utils

rpm -qa | grep nfs
rpm -qa | grep rpcbind

service rpcbind start
service nfs start

config nfs in server:
vi /etc/exports
/opt/mount2/nfsshare 127.0.0.1(rw,sync,no_root_squash)

exportfs -v  : list nfs export folder
showmount -e server_ip : list nfs share folders
```

###centos7 httpd
```
install: yum install httpd.x86_64
install: yum install mod_ssl.x86_64
serverroot: /etc/httpd
sub conf: conf.modules.d/*.conf and conf.d/*.conf
doc root: /var/www/html
logs: logs/

generate certification:
sudo openssl req -x509 -nodes -days 1095 -newkey rsa:2048 -out ssl/server.crt -keyout ssl/server.key

/etc/httpd/conf.d/ssl/server.crt
/etc/httpd/conf.d/ssl/server.key

libopentoken.so
mod_pf.so

apachectl start
apacheectl stop
```
