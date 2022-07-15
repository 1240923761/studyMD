# Change passwd
```cmd
sudo passwd
```
# Change hostname

```cmd
hostnamectl set-hostname xxx
```

# allow ssh root 
```cmd
vim /etc/ssh/ssd_config

port 22
PermitRootLogin yes

systemctl restart sshd
```

# Change ip addr
```cmd
vim /etc/netplan/xxx.yaml
addresses: [xxx.xxx.xxx.xxx/24]
gateway4: ip
nameservers:
    addresses: [gateway4ip]

sudo netplan apply
```