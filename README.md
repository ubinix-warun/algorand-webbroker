# Algorand WebBroker

Not polling the indexer, Merge block event on Indexer. <br/>
WebBroker's support subscribe blockEvent via Websocket and Webhook.

<img src="https://raw.githubusercontent.com/ubinix-warun/algorand-webbroker/main/docs/assets/diagram.png" width="80%">

* [https://github.com/ubinix-warun/algorand-indexer](https://github.com/ubinix-warun/algorand-indexer/tree/develop-pubsocket) -- extend The indexer to subscribe blockEvent.
* [https://github.com/ubinix-warun/algorand-sandbox](https://github.com/ubinix-warun/algorand-sandbox/tree/develop-pubsocket) -- sandbox toolkit for quickstart node/indexer and pg.

Inspired by [PyTEAL Offchain Worker](https://github.com/ubinix-warun/pyteal-offchain-worker)

### [Demo]()
### [Sandbox up]()

# Quickstart

* Create sandbox docker, algod and indexer.

```
Pulling sandbox... POC: Dev PubSocket)
...
Checkout sandbox... (Branch: Dev PubSocket)
Branch 'block-pubsub' set up to track remote branch 'block-pubsub' from 'origin'.
Switched to a new branch 'block-pubsub'
...

algod version
12885622786
3.11.2.stable [rel/stable] (commit #99b37ac0)
go-algorand is licensed with AGPLv3.0
source code available at https://github.com/algorand/go-algorand

Indexer version
2.14.2-dev.unknown compiled at 2022-11-02T09:39:37+0000 from git hash 67ae39bf6c5971e63da41e5a2c27737884e00430 (modified)

```

* Install Insomnia and import [API File]() for demo api.

<img src="https://raw.githubusercontent.com/ubinix-warun/algorand-webbroker/main/docs/assets/screen_createTopicWs.png" width="80%">

### Scenario 1 -- CreateTopic via WebSocket

* Generate token for Websocket and set topic type is "*" (all message).

```

> Request HTTP (POST) to  /generate/websocket with X-API-KEY="<ENV>" 

{
	"topic": {
		"type": "*"
	}
}

< Read "Token" from The response, then use 

{
	"topic": {
		"type": "*",
		"param": ""
	},
	"Target": "",
	"Token": "df0izngG5cYvjU2Y"
}

```

* Subscribe BlockEvent via WebSocket

```

> Connect Websocket to /ws/:token with X-API-KEY="<ENV>" 
< Block Event will Streaming

```

Current version, the topic type is content [ "*", "SENDER", "RECEIVER", "APPID" ].



# Credit

* [Algorand](https://developer.algorand.org/) - Building trusted infrastructure for the Borderless economy. 
* [Echo Framework](https://echo.labstack.com/) and [Gorilla Web Toolkit](https://www.gorillatoolkit.org/)
* [auction-demo](https://github.com/algorand/auction-demo/) - This demo is an on-chain NFT auction using smart contracts on the Algorand blockchain.
* [chainlink-polkadot](https://github.com/smartcontractkit/chainlink-polkadot/tree/master/pallet-chainlink) - This pallet allows your substrate built parachain/blockchain to interract with chainlink. 


