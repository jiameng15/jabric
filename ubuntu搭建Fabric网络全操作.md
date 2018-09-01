# 准备好Ubuntu1604环境

进入linux环境后，先设置root密码

```shell
sudo passwd
```



关闭linux防火墙

```shell
ufw disable
```

更新源

```
apt-get update
```

下载curl

```shell
apt-get install curl 
```

下载ssh

```shell
apt-get install ssh
```

变更ssh接入文件

```shell
vim /etc/ssh/sshd_config 
```

注释掉 #PermitRootLogin without-password，添加 PermitRootLogin yes 

重启ssh服务

```
service ssh restart
```

## 安装Go

#### 1、安装Go包
```shell
wget https://storage.googleapis.com/golang/go1.9.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.9.linux-amd64.tar.gz
```
#### 2、创建文件目录

```

```



#### 3、配置环境变量

```
export GOROOT=/usr/local/go
export GOBIN=$GOROOT/bin
export PATH=$GOBIN:$PATH
export GOPATH=/opt/gopath
```



## 安装docker及docker-compose

### 开始安装Docker CE

#### 1、安装docker包

```
$ sudo apt-get install \



    apt-transport-https \



    ca-certificates \



    curl \



    software-properties-common
```

#### 2、添加Docker的官方GPG密钥：

```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

#### 3、设置stable稳定的仓库(stable稳定版每季度发布一次，Edge版每月一次)

```
$ sudo add-apt-repository \



        "deb [arch=amd64] https://download.docker.com/linux/ubuntu \



        $(lsb_release -cs) \



        stable"
```

#### 4、更新apt包

```
$ apt-get update
```

#### 5、安装Docker CE

```
$ apt-get install docker-ce
```

#### 6、运行Docker

```
$ systemctl start docker
```

#### 7、安装Docker-compose

```
apt-get install docker-compose
```
#### 8、查看Docker及Docker-compose版本

```
docker -v
docker-compose -v
```



## Fabric源码下载

####1、下载fabric源码

```shell
# 下载fabric-sample
git clone https://github.com/hyperledger/fabric-samples.git
# 下载fabric
git clone https://github.com/hyperledger/fabric.git
```

####2、运行bootstrap.sh

进入fabric-sample目录下

```
cd /opt/goptah/src/github.com/hyperledger/fabric-samples
```

运行bootstrap.sh

```shell
./scripts/bootstrap.sh [version] [ca version] [thirdparty_version]
```

此处如果不需要特定版本，则输入默认即可

```
./scripts/bootstrap.sh
```

运行该脚本，将会下载fabric网络所需要的所有镜像

#### 2、运行First-network

```
# 进入fabric-sample目录下的first-network文件夹
cd /opt/goptah/src/github.com/hyperledger/fabric-samples/first-network
# 运行示例网络，下载docker镜像
./byfn.sh up
```



以下部分为成功输出，以作参考

```shell
root@ubuntu:/opt/goptah/src/github.com/hyperledger/fabric-samples/first-network# ./byfn.sh up
Starting for channel 'mychannel' with CLI timeout of '10' seconds and CLI delay of '3' seconds
Continue? [Y/n] y
proceeding ...
LOCAL_VERSION=1.2.0
DOCKER_IMAGE_VERSION=1.2.0
/opt/goptah/src/github.com/hyperledger/fabric-samples/first-network/../bin/cryptogen

##########################################################
##### Generate certificates using cryptogen tool #########
##########################################################
+ cryptogen generate --config=./crypto-config.yaml
org1.example.com
org2.example.com
+ res=0
+ set +x

/opt/goptah/src/github.com/hyperledger/fabric-samples/first-network/../bin/configtxgen
##########################################################
#########  Generating Orderer Genesis block ##############
##########################################################
+ configtxgen -profile TwoOrgsOrdererGenesis -outputBlock ./channel-artifacts/genesis.block
2018-08-31 21:03:07.471 PDT [common/tools/configtxgen] main -> WARN 001 Omitting the channel ID for configtxgen is deprecated.  Explicitly passing the channel ID will be required in the future, defaulting to 'testchainid'.
2018-08-31 21:03:07.471 PDT [common/tools/configtxgen] main -> INFO 002 Loading configuration
2018-08-31 21:03:07.483 PDT [common/tools/configtxgen/encoder] NewChannelGroup -> WARN 003 Default policy emission is deprecated, please include policy specificiations for the channel group in configtx.yaml
2018-08-31 21:03:07.483 PDT [common/tools/configtxgen/encoder] NewOrdererGroup -> WARN 004 Default policy emission is deprecated, please include policy specificiations for the orderer group in configtx.yaml
2018-08-31 21:03:07.483 PDT [common/tools/configtxgen/encoder] NewOrdererOrgGroup -> WARN 005 Default policy emission is deprecated, please include policy specificiations for the orderer org group OrdererOrg in configtx.yaml
2018-08-31 21:03:07.484 PDT [msp] getMspConfig -> INFO 006 Loading NodeOUs
2018-08-31 21:03:07.484 PDT [common/tools/configtxgen/encoder] NewOrdererOrgGroup -> WARN 007 Default policy emission is deprecated, please include policy specificiations for the orderer org group Org1MSP in configtx.yaml
2018-08-31 21:03:07.484 PDT [msp] getMspConfig -> INFO 008 Loading NodeOUs
2018-08-31 21:03:07.484 PDT [common/tools/configtxgen/encoder] NewOrdererOrgGroup -> WARN 009 Default policy emission is deprecated, please include policy specificiations for the orderer org group Org2MSP in configtx.yaml
2018-08-31 21:03:07.484 PDT [common/tools/configtxgen] doOutputBlock -> INFO 00a Generating genesis block
2018-08-31 21:03:07.485 PDT [common/tools/configtxgen] doOutputBlock -> INFO 00b Writing genesis block
+ res=0
+ set +x

#################################################################
### Generating channel configuration transaction 'channel.tx' ###
#################################################################
+ configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID mychannel
2018-08-31 21:03:07.525 PDT [common/tools/configtxgen] main -> INFO 001 Loading configuration
2018-08-31 21:03:07.535 PDT [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 002 Generating new channel configtx
2018-08-31 21:03:07.536 PDT [common/tools/configtxgen/encoder] NewApplicationGroup -> WARN 003 Default policy emission is deprecated, please include policy specificiations for the application group in configtx.yaml
2018-08-31 21:03:07.536 PDT [msp] getMspConfig -> INFO 004 Loading NodeOUs
2018-08-31 21:03:07.536 PDT [common/tools/configtxgen/encoder] NewApplicationOrgGroup -> WARN 005 Default policy emission is deprecated, please include policy specificiations for the application org group Org1MSP in configtx.yaml
2018-08-31 21:03:07.537 PDT [msp] getMspConfig -> INFO 006 Loading NodeOUs
2018-08-31 21:03:07.537 PDT [common/tools/configtxgen/encoder] NewApplicationOrgGroup -> WARN 007 Default policy emission is deprecated, please include policy specificiations for the application org group Org2MSP in configtx.yaml
2018-08-31 21:03:07.538 PDT [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 008 Writing new channel tx
+ res=0
+ set +x

#################################################################
#######    Generating anchor peer update for Org1MSP   ##########
#################################################################
+ configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org1MSPanchors.tx -channelID mychannel -asOrg Org1MSP
2018-08-31 21:03:07.580 PDT [common/tools/configtxgen] main -> INFO 001 Loading configuration
2018-08-31 21:03:07.589 PDT [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 002 Generating anchor peer update
2018-08-31 21:03:07.589 PDT [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 003 Writing anchor peer update
+ res=0
+ set +x

#################################################################
#######    Generating anchor peer update for Org2MSP   ##########
#################################################################
+ configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org2MSPanchors.tx -channelID mychannel -asOrg Org2MSP
2018-08-31 21:03:07.629 PDT [common/tools/configtxgen] main -> INFO 001 Loading configuration
2018-08-31 21:03:07.636 PDT [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 002 Generating anchor peer update
2018-08-31 21:03:07.636 PDT [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 003 Writing anchor peer update
+ res=0
+ set +x

Creating network "net_byfn" with the default driver
Creating volume "net_peer0.org2.example.com" with default driver
Creating volume "net_peer1.org2.example.com" with default driver
Creating volume "net_peer1.org1.example.com" with default driver
Creating volume "net_peer0.org1.example.com" with default driver
Creating volume "net_orderer.example.com" with default driver
Creating orderer.example.com
Creating peer0.org1.example.com
Creating peer1.org1.example.com
Creating peer1.org2.example.com
Creating peer0.org2.example.com
Creating cli

 ____    _____      _      ____    _____ 
/ ___|  |_   _|    / \    |  _ \  |_   _|
\___ \    | |     / _ \   | |_) |   | |  
 ___) |   | |    / ___ \  |  _ <    | |  
|____/    |_|   /_/   \_\ |_| \_\   |_|  

Build your first network (BYFN) end-to-end test

Channel name : mychannel
Creating channel...
+ peer channel create -o orderer.example.com:7050 -c mychannel -f ./channel-artifacts/channel.tx --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
+ res=0
+ set +x
2018-09-01 04:03:11.019 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-09-01 04:03:11.041 UTC [cli/common] readBlock -> INFO 002 Got status: &{NOT_FOUND}
2018-09-01 04:03:11.043 UTC [channelCmd] InitCmdFactory -> INFO 003 Endorser and orderer connections initialized
2018-09-01 04:03:11.248 UTC [cli/common] readBlock -> INFO 004 Received block: 0
===================== Channel 'mychannel' created ===================== 

Having all peers join the channel...
+ peer channel join -b mychannel.block
+ res=0
+ set +x
2018-09-01 04:03:11.346 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-09-01 04:03:11.381 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
===================== peer0.org1 joined channel 'mychannel' ===================== 
+ peer channel join -b mychannel.block

+ res=0
+ set +x
2018-09-01 04:03:14.462 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-09-01 04:03:14.498 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
===================== peer1.org1 joined channel 'mychannel' ===================== 

+ peer channel join -b mychannel.block
+ res=0
+ set +x
2018-09-01 04:03:17.593 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-09-01 04:03:17.634 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
===================== peer0.org2 joined channel 'mychannel' ===================== 

+ peer channel join -b mychannel.block
+ res=0
+ set +x
2018-09-01 04:03:20.751 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-09-01 04:03:20.774 UTC [channelCmd] executeJoin -> INFO 002 Successfully submitted proposal to join channel
===================== peer1.org2 joined channel 'mychannel' ===================== 

Updating anchor peers for org1...
+ peer channel update -o orderer.example.com:7050 -c mychannel -f ./channel-artifacts/Org1MSPanchors.tx --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
+ res=0
+ set +x
2018-09-01 04:03:23.908 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-09-01 04:03:23.925 UTC [channelCmd] update -> INFO 002 Successfully submitted channel update
===================== Anchor peers updated for org 'Org1MSP' on channel 'mychannel' ===================== 

Updating anchor peers for org2...
+ peer channel update -o orderer.example.com:7050 -c mychannel -f ./channel-artifacts/Org2MSPanchors.tx --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
+ res=0
+ set +x
2018-09-01 04:03:27.004 UTC [channelCmd] InitCmdFactory -> INFO 001 Endorser and orderer connections initialized
2018-09-01 04:03:27.019 UTC [channelCmd] update -> INFO 002 Successfully submitted channel update
===================== Anchor peers updated for org 'Org2MSP' on channel 'mychannel' ===================== 
+ peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/

Installing chaincode on peer0.org1...
+ res=0
+ set +x
2018-09-01 04:03:30.155 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2018-09-01 04:03:30.155 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2018-09-01 04:03:30.743 UTC [chaincodeCmd] install -> INFO 003 Installed remotely response:<status:200 payload:"OK" > 
===================== Chaincode is installed on peer0.org1 ===================== 

Install chaincode on peer0.org2...
+ peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/
+ res=0
+ set +x
2018-09-01 04:03:30.836 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2018-09-01 04:03:30.836 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2018-09-01 04:03:31.103 UTC [chaincodeCmd] install -> INFO 003 Installed remotely response:<status:200 payload:"OK" > 
===================== Chaincode is installed on peer0.org2 ===================== 

Instantiating chaincode on peer0.org2...
+ peer chaincode instantiate -o orderer.example.com:7050 --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n mycc -l golang -v 1.0 -c '{"Args":["init","a","100","b","200"]}' -P 'AND ('\''Org1MSP.peer'\'','\''Org2MSP.peer'\'')'
+ res=0
+ set +x
2018-09-01 04:03:31.189 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2018-09-01 04:03:31.189 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
===================== Chaincode is instantiated on peer0.org2 on channel 'mychannel' ===================== 

Querying chaincode on peer0.org1...
===================== Querying on peer0.org1 on channel 'mychannel'... ===================== 
Attempting to Query peer0.org1 ...3 secs
+ peer chaincode query -C mychannel -n mycc -c '{"Args":["query","a"]}'
+ res=0
+ set +x

100
===================== Query successful on peer0.org1 on channel 'mychannel' ===================== 
Sending invoke transaction on peer0.org1 peer0.org2...
+ peer chaincode invoke -o orderer.example.com:7050 --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n mycc --peerAddresses peer0.org1.example.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses peer0.org2.example.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"Args":["invoke","a","b","10"]}'
+ res=0
+ set +x
2018-09-01 04:04:08.703 UTC [chaincodeCmd] chaincodeInvokeOrQuery -> INFO 001 Chaincode invoke successful. result: status:200 
===================== Invoke transaction successful on peer0.org1 peer0.org2 on channel 'mychannel' ===================== 
+ peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/

Installing chaincode on peer1.org2...
+ res=0
+ set +x
2018-09-01 04:04:08.782 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 001 Using default escc
2018-09-01 04:04:08.782 UTC [chaincodeCmd] checkChaincodeCmdParams -> INFO 002 Using default vscc
2018-09-01 04:04:08.963 UTC [chaincodeCmd] install -> INFO 003 Installed remotely response:<status:200 payload:"OK" > 
===================== Chaincode is installed on peer1.org2 ===================== 

Querying chaincode on peer1.org2...
===================== Querying on peer1.org2 on channel 'mychannel'... ===================== 
Attempting to Query peer1.org2 ...3 secs
+ peer chaincode query -C mychannel -n mycc -c '{"Args":["query","a"]}'
+ res=0
+ set +x

90
===================== Query successful on peer1.org2 on channel 'mychannel' ===================== 

========= All GOOD, BYFN execution completed =========== 


 _____   _   _   ____   
| ____| | \ | | |  _ \  
|  _|   |  \| | | | | | 
| |___  | |\  | | |_| | 
|_____| |_| \_| |____/  


```

