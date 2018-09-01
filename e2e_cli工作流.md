# e2e_cli工作流 v1.2

[原文链接](https://github.com/hyperledger/fabric/blob/release-1.2/examples/e2e_cli/end-to-end.rst)

end-to-end 验证系统提供了一个简单的fabric网络实例，这个实例由2个组织、每个组织2个节点，和一个基于kafka的order服务。

该验证体系使用了两个重要的基本工具，其创建了具有数字签名及访问控制的交易网络。

* cryptogen——生成用于在网络中作身份验证及权限控制的x509证书。
* configtxgen——生成用于引导orderer和创建channel必要的配置文件。

每个工具都通过yaml文件来配置，我们可以通过cryptogen工具指定拓扑网络、通过configtxgen工具指定证书。当这套工具成功运行时，我们已经能启动我们的网络了。更多关于工具和构造器的细节将于下文阐述。那么现在，我们准备开始。

## 准备

- [Git client](https://git-scm.com/downloads)
- [Docker](https://www.docker.com/products/overview) - v1.12 或更高版本
- [Docker Compose](https://docs.docker.com/compose/overview/) - v1.8 或更高版本
- [Homebrew ](https://brew.sh/) - 仅OSX系统需要
- [Xcode](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) - 仅OSX系统需要 (可能需要一个小时以上)
- [Docker Toolbox](https://docs.docker.com/toolbox/toolbox_install_windows/) - 仅Windows用户
- [Go](https://golang.org/) - 1.9 或更高版本

在Windows运行环境上，你还需要如下内容作为更好的代替品：

- [Git Bash](https://git-scm.com/downloads)

注意

Windows7或更老版本，您通常会安装Docker Toolbox，就经验来说，这是个糟糕的开发环境。你可能找不到适合的 `make`命令来成功运行本实例。

### 环境变量 $GOPATH 设置

请确认 [GOPATH](https://github.com/golang/go/wiki/GOPATH) 环境变量配置正确，这是确保编码正确的关键。

现在，创建一下文件目录，并 `cd` 进入目录：

```shell
mkdir -p $GOPATH/src/github.com/hyperledger
cd $GOPATH/src/github.com/hyperledger
```

* 将Fabric项目克隆到此目录

```shell
git clone http://gerrit.hyperledger.org/r/fabric
```

或通过github的镜像库拉取

```shell
git clone https://github.com/hyperledger/fabric.git
```

* 如果你是OSX系统，请运行如下代码

```shell
brew install gnu-tar --with-default-names
brew install libtool
```

### 构建二进制文件

* 现在制作官方专用二进制文件 `cryptogen`和`configtxgen` 

```shell
cd $GOPATH/src/github.com/hyperledger/fabric
# ensure sure you are in the /fabric directory where the Makefile resides
make release
```

这一步生成官方专用二进制文件，我们在 `fabric/release ` 目录下下可以找到。

译者按：

此处也可使用fabric-sample项目中下载好的二进制文件包`bin/`

其目录结构如下：

```shell
bin
├── configtxgen
├── configtxlator
├── cryptogen
├── discover
├── fabric-ca-client
├── get-docker-images.sh
├── idemixgen
├── orderer
└── peer

```

* 下一步，生成 Fabric 镜像。通常情况，这一步需要5~10分钟，请耐心等待

```shell
# make sure you are in the /fabric directory
make docker
```

在终端执行 `docker images` 命令 。

如果镜像编译成功，你将会看到类似于如下内容：

```shell
REPOSITORY                     TAG                   IMAGE ID            CREATED             SIZE
hyperledger/fabric-couchdb     latest                e2df4dd39ca9        38 minutes ago      1.51 GB
hyperledger/fabric-couchdb     amd64-1.1.0           e2df4dd39ca9        38 minutes ago      1.51 GB
hyperledger/fabric-kafka       latest                08af4d797266        40 minutes ago      1.3 GB
hyperledger/fabric-kafka       amd64-1.1.0           08af4d797266        40 minutes ago      1.3 GB
hyperledger/fabric-zookeeper   latest                444e9e695367        40 minutes ago      1.31 GB
hyperledger/fabric-zookeeper   amd64-1.1.0           444e9e695367        40 minutes ago      1.31 GB
hyperledger/fabric-testenv     latest                8678d3101930        41 minutes ago      1.41 GB
hyperledger/fabric-testenv     amd64-1.1.0           8678d3101930        41 minutes ago      1.41 GB
hyperledger/fabric-buildenv    latest                60911392c82e        41 minutes ago      1.33 GB
hyperledger/fabric-buildenv    amd64-1.1.0           60911392c82e        41 minutes ago      1.33 GB
hyperledger/fabric-orderer     latest                2afab937b9cc        41 minutes ago      182 MB
hyperledger/fabric-orderer     amd64-1.1.0           2afab937b9cc        41 minutes ago      182 MB
hyperledger/fabric-peer        latest                9560e58e8089        41 minutes ago      185 MB
hyperledger/fabric-peer        amd64-1.1.0           9560e58e8089        41 minutes ago      185 MB
hyperledger/fabric-javaenv     latest                881ca5219fad        42 minutes ago      1.43 GB
hyperledger/fabric-javaenv     amd64-1.1.0           881ca5219fad        42 minutes ago      1.43 GB
hyperledger/fabric-ccenv       latest                28af77ffe9e9        43 minutes ago      1.29 GB
hyperledger/fabric-ccenv       amd64-1.1.0           28af77ffe9e9        43 minutes ago      1.29 GB
hyperledger/fabric-baseimage   amd64-0.4.8           f4751a503f02        3 months ago        1.27 GB
hyperledger/fabric-baseos      amd64-0.4.8           c3a4cf3b3350        3 months ago        161 MB
```

译者按：

这里的镜像是基于fabric-1.1.0版当做示例，开发者根据自己版本不同会有差别，这是正常的。

如果 `fabric-testenv ` 镜像编译失败，那开发者们可以使用 `make clean` 命令 之后使用 `make docker` 命令。

## Cryptogen 工具

我们使用cryptogen工具创建密文材料(x509证书)，用于所有网络实体。

证书基于标准PKI实现，通过连接公共信任锚点来实现验证。 

### cryptogen如何运作

Cryptogen工具需要通过一个文件——`crypto-config.yaml `—— 囊括了网络拓扑，同时允许我们为组织和属于这些组织的组件生成一个证书库。 每个组织都有一个唯一的根证书（`ca cert`），它将特定的组件（peer节点 和 orderers节点）绑定到那个Org。Fabric内部的事务和通信由实体的私钥（`keystore `）签署，然后通过公钥（`signcerts`）进行验证。您将在这个文件中注意到一个“count”变量。我们使用它来指定每个组织的 peer 节点数；在我们的例子中，每个组织有两个 peer 节点。这个实例的其余部分是非常浅显易懂的。 

当我们运行cryptogen工具之后，证书将安放在`crypto-config `文件目录中。



## 配置交易生成器

Configtxgen工具用来创建4个神器：

- orderer genesis block,（最终生成的文件名为：orderer.block，用来给Orderer节点做排序服务的）
- channel configuration transaction,（用来配置和创建channel的配置文件）
- and two anchor peer transactions - one for each Peer Org.（生成的信息存放在orderer.block初始块文件中，anchor peer暂时理解为初始化时的默认节点）

orderer区块orderer服务的创世区块，并且channel交易文件在channel创建时向orderer广播。锚点交易，正如其名，指定在这个channel上每个组织的锚点。 

————————未完待续————————

这部分，应该需要自己封装fabric-sdk来实现，思路是：

把使用的chaincode名称存到交易信息中，

通过hash找到区块信息，在区块信息中拿到交易ID，查到交易信息中的chaincode名称，调取对应的chaincode查看内容