
## Creating Identities [Tool : `cryptogen`]

```
../bin/cryptogen generate --config=./crypto-config.yaml

```

Output

```
org1.example.com
org2.example.com

```
> The certs and keys (i.e. the MSP material) will be output into a directory - `crypto-config`

## Create orderer genesis block [Tool : `configtxgen`]

Tell the configtxgen tool where to look for the configtx.yaml file that it needs to ingest.

```
export FABRIC_CFG_PATH=$PWD
```

invoke the configtxgen tool to create the orderer genesis block


```
../bin/configtxgen -profile TwoOrgsOrdererGenesis -channelID byfn-sys-channel -outputBlock ./channel-artifacts/genesis.block

```

Output

```

2019-03-27 21:21:19.709 IST [common.tools.configtxgen] main -> INFO 001 Loading configuration
2019-03-27 21:21:19.739 IST [common.tools.configtxgen.localconfig] completeInitialization -> INFO 002 orderer type: solo
2019-03-27 21:21:19.739 IST [common.tools.configtxgen.localconfig] Load -> INFO 003 Loaded configuration: /home/vishswasb/work/learn/hl101/fabric-samples/first-network/configtx.yaml
2019-03-27 21:21:19.764 IST [common.tools.configtxgen.localconfig] completeInitialization -> INFO 004 orderer type: solo
2019-03-27 21:21:19.764 IST [common.tools.configtxgen.localconfig] LoadTopLevel -> INFO 005 Loaded configuration: /home/vishswasb/work/learn/hl101/fabric-samples/first-network/configtx.yaml
2019-03-27 21:21:19.765 IST [common.tools.configtxgen] doOutputBlock -> INFO 006 Generating genesis block
2019-03-27 21:21:19.767 IST [common.tools.configtxgen] doOutputBlock -> INFO 007 Writing genesis block

```

> The orderer genesis block and the subsequent artifacts we are about to create will be output into the `channel-artifacts` directory at the root of this project. The channelID in the above command is the name of the system channel.


##  Create a Channel Configuration Transaction

```
export CHANNEL_NAME=mychannel  && ../bin/configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID $CHANNEL_NAME

```

Output

```
2019-03-27 21:30:07.448 IST [common.tools.configtxgen] main -> INFO 001 Loading configuration
2019-03-27 21:30:07.473 IST [common.tools.configtxgen.localconfig] Load -> INFO 002 Loaded configuration: /home/vishswasb/work/learn/hl101/fabric-samples/first-network/configtx.yaml
2019-03-27 21:30:07.498 IST [common.tools.configtxgen.localconfig] completeInitialization -> INFO 003 orderer type: solo
2019-03-27 21:30:07.498 IST [common.tools.configtxgen.localconfig] LoadTopLevel -> INFO 004 Loaded configuration: /home/vishswasb/work/learn/hl101/fabric-samples/first-network/configtx.yaml
2019-03-27 21:30:07.498 IST [common.tools.configtxgen] doOutputChannelCreateTx -> INFO 005 Generating new channel configtx
2019-03-27 21:30:07.499 IST [common.tools.configtxgen] doOutputChannelCreateTx -> INFO 006 Writing new channel tx


```

## Define the anchor peer for Org1 on the channel

```
../bin/configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org1MSPanchors.tx -channelID $CHANNEL_NAME -asOrg Org1MSP

```

Output

```
2019-03-27 21:32:34.125 IST [common.tools.configtxgen] main -> INFO 001 Loading configuration
2019-03-27 21:32:34.152 IST [common.tools.configtxgen.localconfig] Load -> INFO 002 Loaded configuration: /home/vishswasb/work/learn/hl101/fabric-samples/first-network/configtx.yaml
2019-03-27 21:32:34.179 IST [common.tools.configtxgen.localconfig] completeInitialization -> INFO 003 orderer type: solo
2019-03-27 21:32:34.179 IST [common.tools.configtxgen.localconfig] LoadTopLevel -> INFO 004 Loaded configuration: /home/vishswasb/work/learn/hl101/fabric-samples/first-network/configtx.yaml
2019-03-27 21:32:34.179 IST [common.tools.configtxgen] doOutputAnchorPeersUpdate -> INFO 005 Generating anchor peer update
2019-03-27 21:32:34.179 IST [common.tools.configtxgen] doOutputAnchorPeersUpdate -> INFO 006 Writing anchor peer update

```

## Define the anchor peer for Org2 on the same channel

```
../bin/configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org2MSPanchors.tx -channelID $CHANNEL_NAME -asOrg Org2MSP
```

## Start The Network





