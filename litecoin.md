## Instance Specs
```
OS: Ubuntu 18-04 LTS
Disk size: 50Gb
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
## install berkeley-db
```
wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz'
LITECOIN_ROOT=$(pwd)/litecoin
BDB_PREFIX="${LITECOIN_ROOT}/db4"
mkdir -p $BDB_PREFIX
echo '12edc0df75bf9abd7f82f821795bcee50f42cb2e5f76a6a281b85732798364ef  db-4.8.30.NC.tar.gz' | sha256sum -c
tar -xzvf db-4.8.30.NC.tar.gz
cd db-4.8.30.NC/build_unix/
../dist/configure --enable-cxx --disable-shared --with-pic --prefix=$BDB_PREFIX
make install
```

## install litecoin

```
git clone https://github.com/litecoin-project/litecoin.git
cd $LITECOIN_ROOT
./autogen.sh
./configure LDFLAGS="-L${BDB_PREFIX}/lib/" CPPFLAGS="-I${BDB_PREFIX}/include/" CXXFLAGS="--param ggc-min-expand=1 --param ggc-min-heapsize=32768" --enable-upnp-default --without-gui
make
sudo make install

```

## install supervisor

```
sudo apt install supervisor -y
```

## create litecoind.conf

```
mkdir ~/.config/
nano ~/.config/litecoind.conf
```

#### litecoind.conf content
```
rpcuser=litecoin_testnet
rpcpassword=litecoin_testnetlitecoin_testnet
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

## create litecoind supervisor

```
sudo nano /etc/supervisor/conf.d/litecoind.conf
```

#### litecoind.conf supervisor content
```
[program:litecoind]
command=/usr/bin/litecoind -maxconnections=500 -conf=/home/michaelphan/.config/litecoind.conf -datadir=/home/michaelphan/.config/
user=michaelphan
autostart=true
autorestart=true
stderr_logfile=/var/log/litecoind.err.log
stdout_logfile=/var/log/litecoind.out.log
```

## update new litecoind supervisor config file
```
sudo supervisorctl reread;sudo supervisorctl update; sudo supervisorctl restart all
```
## install zmq lib
```
pip3 install zmq
```
## create zmq_sub.py
```
nano /home/michaelphan/.config/zmq_sub.py
```
## create zmq supervisor

```
[program:zmq]
command=python3 /home/michaelphan/.config/zmq_sub.py
user=michaelphan
autostart=true
autorestart=true
stderr_logfile=/var/log/zmq.err.log
stdout_logfile=/var/log/zmq.out.log
```


