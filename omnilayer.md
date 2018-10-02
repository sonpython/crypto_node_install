## Instance Specs
```
OS: Debian GNU-Linux 8 -Jessie [important to use this OS version]
Disk size: 400Gb
RAM: 8Gb
CPU: 2Cores
```
## install utils + update system

```
sudo apt-get update -y; sudo apt-get upgrade -y; sudo apt-get install git htop nload curl wget zip unzip nano cron build-essential python-pip -y
```

## install dependent libs

```
sudo apt-get install libtool-dev libtool autoconf autotools-dev gcc g++ libdb++-dev pkg-config
sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils
sudo apt-get install libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-program-options-dev libboost-test-dev libboost-thread-dev software-properties-common python3-pip
```

## install omnid

```
git clone https://github.com/OmniLayer/omnicore.git
cd omnicore/
./autogen.sh
./configure --with-incompatible-bdb
```

## install supervisor

```
sudo apt install supervisor -y
```
## create blocknoti.py
```
pip install requests
nano /home/admin/omni/blocknoti.py

```
#### blocknoti.py content
```
import requests
import sys
requests.post("http://devapi.sencoinex.com/v1/omni-webhook/", json={'hash':sys.argv[1]})
```

## create bitcoind.conf

```
nano /home/admin/omni/bitcoind.conf
```

#### bitcoind.conf content
```
rpcuser=bitcoin_mainnet
rpcpassword=bitcoin_mainnetbitcoin_mainnet
testnet=1
rpcport=8332
rpcallowip=0.0.0.0/0
server=1

# notify script trigger 
blocknotify=python /home/admin/omni/blocknoti.py %s
```

## create bitcoind supervisor

```
sudo nano /etc/supervisor/conf.d/bitcoind.conf
```

#### bitcoind.conf supervisor content
```
[program:omni]
command = /home/admin/omni/omnicore/src/omnicored -conf=/home/admin/omni/bitcoin.conf -txindex -txconfirmtarget=25
user = admin
directory = /home/admin/omni/
startsecs=20
autostart=true
autorestart=true
startretries=1
redirect_stderr=true
logfile_maxbytes = 50MB
loglevel         = warn
stderr_logfile=/var/log/omni.err.log
stdout_logfile=/var/log/omni.out.log
```

## update new bitcoind supervisor config file
```
sudo supervisorctl reread;sudo supervisorctl update; sudo supervisorctl restart all
```
