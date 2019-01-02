## Instance Specs
```
OS: Ubuntu 18-04 LTS
Disk size: 200Gb
RAM: 8Gb
CPU: 2Cores
```
## Install utils + update system

```
sudo apt-get update -y; sudo apt-get upgrade -y; sudo apt-get install git htop nload curl wget zip unzip nano cron build-essential -y
```

## Install dependent libs

```
sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils
sudo apt-get install libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-program-options-dev libboost-test-dev libboost-thread-dev software-properties-common python3-pip
```

## Install bitcoin-sv
```
wget wget https://github.com/bitcoin-sv/bitcoin-sv/releases/download/v0.1.0/bitcoin-sv-0.1.0-x86_64-linux-gnu.tar.gz
tar -xvf bitcoin-sv-0.1.0-x86_64-linux-gnu.tar.gz
mv bitcoin-sv-0.1.0 bitcoin-sv
sudo cp /home/ubuntu/bitcoin-sv/bin/* /usr/bin/
mkdir /home/ubuntu/data

(Note: check latest version on https://github.com/bitcoin-sv/bitcoin-sv/releases)
```

## Create bitcoind.conf content
```
nano bitcoind.conf
rpcuser=coinhe_bitcoinsv_v1
rpcpassword=bitcoinsv
testnet=0
rpcport=8332
rpcallowip=0.0.0.0/0
server=1

# this is for zmq notification
zmqpubrawtx=tcp://127.0.0.1:28332
zmqpubrawblock=tcp://127.0.0.1:28332
zmqpubhashtx=tcp://127.0.0.1:28332
zmqpubhashblock=tcp://127.0.0.1:28332
```

## Install supervisor
```
sudo apt install supervisor -y
```

## Create bitcoind supervisor
```
sudo nano /etc/supervisor/conf.d/bitcoind.conf
```

## Create bitcoind.conf supervisor content
```
[program:bitcoind]
command=/usr/bin/bitcoind -maxconnections=500 -conf=/home/ubuntu/bitcoind.conf -datadir=/home/ubuntu/data/
user=ubuntu
autostart=true
autorestart=true
stderr_logfile=/var/log/bitcoind.err.log
stdout_logfile=/var/log/bitcoind.out.log
```
## Install zmq lib
```
pip3 install zmq
```

## Create bsv_zmq_sub.py
```
nano /home/ubuntu/bsv_zmq_sub.py
(Copy from /zmq/bsv_zmq_sub.py)
```

## Create zmq supervisor
```
sudo nano /etc/supervisor/conf.d/zmq.conf
```

## Create zmq supervisor content
```
[program:zmq]
command=python3 /home/ubuntu/bsv_zmq_sub.py
user=ubuntu
autostart=true
autorestart=true
stderr_logfile=/var/log/zmq.err.log
stdout_logfile=/var/log/zmq.out.log
```

## Update new bitcoind supervisor config file
```
sudo supervisorctl reread;sudo supervisorctl update; sudo supervisorctl restart all
```

## Check result
bitcoin-cli -rpcpassword=bitcoinsv -rpcuser=coinhe_bitcoinsv_v1 getnetworkinfo
```{
  "version": 100010000,
  "subversion": "/Bitcoin SV:0.1.0(EB128.0)/",
  "protocolversion": 70015,
  "localservices": "0000000000000025",
  "localrelay": true,
  "timeoffset": 0,
  "networkactive": true,
  "connections": 5,
  "networks": [
    {
      "name": "ipv4",
      "limited": false,
      "reachable": true,
      "proxy": "",
      "proxy_randomize_credentials": false
    },
    {
      "name": "ipv6",
      "limited": false,
      "reachable": true,
      "proxy": "",
      "proxy_randomize_credentials": false
    },
    {
      "name": "onion",
      "limited": true,
      "reachable": false,
      "proxy": "",
      "proxy_randomize_credentials": false
    }
  ],
  "relayfee": 0.00001000,
  "excessutxocharge": 0.00000000,
  "localaddresses": [
  ],
  "warnings": ""
}
```

bitcoin-cli -rpcpassword=bitcoinsv -rpcuser=coinhe_bitcoinsv_v1 getinfo
```{
  "version": 100010000,
  "protocolversion": 70015,
  "walletversion": 160300,
  "balance": 0.00000000,
  "blocks": 168894,
  "timeoffset": 0,
  "connections": 6,
  "proxy": "",
  "difficulty": 1376302.26788638,
  "testnet": false,
  "keypoololdest": 1545043857,
  "keypoolsize": 2000,
  "paytxfee": 0.00000000,
  "relayfee": 0.00001000,
  "errors": "",
  "maxblocksize": 128000000,
  "maxminedblocksize": 32000000
}
```
