
## Instance Specs
```
OS: Ubuntu 18-04 LTS
Disk size: 200Gb
RAM: 8Gb
CPU: 2Cores
```

Offical Docs: https://developers.ripple.com/install-rippled-on-ubuntu-with-alien.html

## install utils + update system

```
sudo apt-get update -y; sudo apt-get upgrade -y; sudo apt-get install git htop nload curl wget zip unzip nano cron build-essential python3-pip -y
sudo apt-get install yum-utils alien
```

## install Rippled

```
sudo rpm -Uvh https://mirrors.ripple.com/ripple-repo-el7.rpm
yumdownloader --enablerepo=ripple-stable --releasever=el7 rippled
sudo rpm --import https://mirrors.ripple.com/rpm/RPM-GPG-KEY-ripple-release && rpm -K rippled*.rpm
sudo alien -i --scripts rippled*.rpm && rm rippled*.rpm

```


## update rippled.cfg

```
sudo nano /etc/opt/ripple/rippled.cfg
```
Change `online_delete=1000` to `online_delete=500`

## enable and start the rippled service:
```commandline
sudo systemctl enable rippled.service
sudo systemctl start rippled.service
```

## check the rippled status:
#### service status:
```commandline
sudo systemctl status rippled
===============
● rippled.service - Ripple Daemon
   Loaded: loaded (/usr/lib/systemd/system/rippled.service; enabled; vendor preset: enabled)
  Drop-In: /etc/systemd/system/rippled.service.d
           └─nofile_limit.conf
   Active: active (running) since Sun 2018-12-16 20:58:51 UTC; 6min ago
 Main PID: 909 (rippled: #1)
    Tasks: 23 (limit: 4704)
   CGroup: /system.slice/rippled.service
           ├─ 909 /opt/ripple/bin/rippled --net --silent --conf /etc/opt/ripple/rippled.cfg
           └─1155 /opt/ripple/bin/rippled --net --silent --conf /etc/opt/ripple/rippled.cfg

```
#### service ports:
```commandline
sudo netstat -ntpl
===============
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:6006          0.0.0.0:*               LISTEN      1155/rippled        
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1003/sshd           
tcp        0      0 0.0.0.0:51235           0.0.0.0:*               LISTEN      1155/rippled        
tcp        0      0 127.0.0.1:5005          0.0.0.0:*               LISTEN      1155/rippled        
tcp6       0      0 :::22                   :::*                    LISTEN      1003/sshd     

```
