Instance Specs and Run Command for Mainnet Crypto Node

### 1. Bitcoin
##### Instance Specs
```
OS: Ubuntu 18-04 LTS
Disk size: 400Gb
RAM: 16Gb
CPU: 4Cores
```
##### Running command
```
/usr/bin/bitcoind -maxconnections=500 -conf=/home/ubuntu/.bitcoin/bitcoind.conf -datadir=/home/ubuntu/.bitcoin/
```

### 2. BitcoinCash
##### Instance Specs
```
OS: Ubuntu 18-04 LTS
Disk size: 250Gb
RAM: 16Gb
CPU: 4Cores
```
##### Running command
```
/usr/bin/bitcoind -maxconnections=500 -conf=/home/ubuntu/.bitcoin/bitcoind.conf -datadir=/home/ubuntu/.bitcoin/
```

### 3. Ethereum
##### Instance Specs
```
OS: Ubuntu 18-04 LTS
Disk size: 50Gb
RAM: 16Gb
CPU: 4Cores
```
##### Running command
```
/usr/bin/geth --syncmode "light" --datadir "/home/ubuntu/.ethereum" --rpc --rpcaddr "0.0.0.0" --rpcapi "db,eth,net,web3,personal,web3"
```

### 4. Litecoin
##### Instance Specs
```
OS: Ubuntu 18-04 LTS
Disk size: 100Gb
RAM: 16Gb
CPU: 4Cores
```
##### Running command
```
/usr/local/bin/litecoind -maxconnections=500 -conf=/home/ubuntu/litecoin/litecoind.conf -datadir=/home/ubuntu/litecoin/
```

### 5. OmniLayer
##### Instance Specs
```
OS: Debian GNU-Linux 8 -Jessie [important to use this OS version]
Disk size: 400Gb
RAM: 16Gb
CPU: 4Cores
```
##### Running command
```
/home/admin/omni/omnicore/src/omnicored -conf=/home/admin/omni/bitcoin.conf -txindex -txconfirmtarget=25 -datadir=/home/admin/omni/data -minrelaytxfee=0.00005 -paytxfee=0.00005
```