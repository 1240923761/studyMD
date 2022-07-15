# download iso
minimal

# network
```cmd
dhclient

vim /etc/sysconfig/network-scripts/ifcfg-ens33
将ONBOOT=no改为yes，将BOOTPROTO=dhcp改为BOOTPROTO=static,并在后面增加几行内容：
IPADDR=192.168.127.128
NETMASK=255.255.255.0
GATEWAY=192.168.127.2
```

# install docker
```cmd
sudo yum update

sudo yum remove docker  docker-common docker-selinux docker-engine

sudo yum install -y yum-utils device-mapper-persistent-data lvm2

sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

yum list docker-ce --showduplicates | sort -r

sudo yum install docker-ce-version

sudo systemctl start docker

sudo systemctl enable docker
```