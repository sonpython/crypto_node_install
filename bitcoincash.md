## Instance Specs
```
OS: Ubuntu 18-04 LTS
Disk size: 50Gb
RAM: 8Gb
CPU: 2Cores
```
## install utils + update system

```
sudo apt-get update -y; sudo apt-get upgrade -y; sudo apt-get install git htop nload curl wget zip unzip nano cron build-essential -y
```

## install dependent libs

```
sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils
sudo apt-get install libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-program-options-dev libboost-test-dev libboost-thread-dev software-properties-common python3-pip
```

## install bitcoincash

```
sudo apt-get install software-properties-common -y;sudo add-apt-repository ppa:bitcoin-abc/ppa;sudo apt-get update -y

sudo apt-get update

sudo apt-get install bitcoind
```

## install supervisor

```
sudo apt install supervisor -y
```

## create bitcoind.conf

```
mkdir ~/.bitcoin/ && cd ~/.bitcoin/
nano bitcoind.conf
```

#### bitcoind.conf content
```
rpcuser=bitcoincash_testnet
rpcpassword=bitcoincash_testnetbitcoincash_testnet
testnet=1
rpcport=8332
rpcallowip=0.0.0.0/0
server=1

# this is for zmq notification
zmqpubrawtx=tcp://127.0.0.1:28332
zmqpubrawblock=tcp://127.0.0.1:28332
zmqpubhashtx=tcp://127.0.0.1:28332
zmqpubhashblock=tcp://127.0.0.1:28332
```

## create bitcoind supervisor

```
sudo nano /etc/supervisor/conf.d/bitcoind.conf
```

#### bitcoind.conf supervisor content
```
[program:bitcoind]
command=/usr/bin/bitcoind -maxconnections=500 -conf=/home/michaelphan/.bitcoin/bitcoind.conf -datadir=/home/michaelphan/.bitcoin/
user=michaelphan
autostart=true
autorestart=true
stderr_logfile=/var/log/bitcoind.err.log
stdout_logfile=/var/log/bitcoind.out.log
```

## update new bitcoind supervisor config file
```
sudo supervisorctl reread;sudo supervisorctl update; sudo supervisorctl restart all
```
## install zmq lib
```
pip3 install zmq
```
## create zmq_sub.py
```
nano /home/michaelphan/.bitcoin/zmq_sub.py
```
## create zmq supervisor

```
[program:zmq]
command=python3 /home/michaelphan/.bitcoin/zmq_sub.py
user=michaelphan
autostart=true
autorestart=true
stderr_logfile=/var/log/zmq.err.log
stdout_logfile=/var/log/zmq.out.log
```


