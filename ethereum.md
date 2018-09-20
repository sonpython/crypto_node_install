## Instance Specs
```
OS: Ubuntu 18-04 LTS
Disk size: 50Gb (geth, syncmode=fast, rinkeby test net)
RAM: 8Gb
CPU: 2Cores
```
## install utils + update system

```
sudo apt-get update -y; sudo apt-get upgrade -y; sudo apt-get install git htop nload curl wget zip unzip nano cron build-essential python3-pip -y
```

## install dependent libs

```
sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils libminiupnpc-dev libzmq3-dev
sudo apt-get install libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-program-options-dev libboost-test-dev libboost-thread-dev software-properties-common python3-pip
```

## install ethereum

```
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum

```

## install supervisor

```
sudo apt install supervisor -y
```

## create ethereumd.conf

```
mkdir ~/.ethereum/
nano ~/.ethereum/ethereumd.conf
```

## create geth supervisor

```
sudo nano /etc/supervisor/conf.d/geth.conf
```

#### geth.conf supervisor content
```
[program:geth]
command=/usr/bin/geth --rinkeby --syncmode "fast" --rpc --rpcaddr "0.0.0.0" -rpcapi "db,eth,net,web3,personal,web3" --cache=2048
user=michaelphan
autostart=true
autorestart=true
stderr_logfile=/var/log/geth.err.log
stdout_logfile=/var/log/geth.out.log
```

## update new litecoind supervisor config file
```
sudo supervisorctl reread;sudo supervisorctl update; sudo supervisorctl restart all
```

