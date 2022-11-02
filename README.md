# Algorand WebBroker

Challenge with [Bounty](https://gitcoin.co/issue/29380) - [Algorand Greehouse Hack #2](https://gitcoin.co/hackathon/greenhouse). <br/>
>> We are looking to build a solution that will enable subscribing to events on the blockchain and all the subscribers will be notified in real time of the various events they subscribe to.

No polling the indexer, Merge block event on indexer. <br/>
WebBroker's support subscribe blockEvent via Websocket and Webhook.

<img src="https://raw.githubusercontent.com/ubinix-warun/algorand-webbroker/main/docs/assets/diagram2.png" width="80%">

* [https://github.com/ubinix-warun/algorand-indexer](https://github.com/ubinix-warun/algorand-indexer/tree/develop-pubsocket) -- extend the indexer to subscribe blockEvent.
* [https://github.com/ubinix-warun/algorand-sandbox](https://github.com/ubinix-warun/algorand-sandbox/tree/develop-pubsocket) -- sandbox toolkit for quickstart node/indexer and pg.

<br/>

Inspired by [PyTEAL Offchain Worker](https://github.com/ubinix-warun/pyteal-offchain-worker) and [Algo Checker from Algorand Greehouse Hack #1](https://github.com/ubinix-warun/react-algochecker).

-----------------

### [Sandbox up]() -- 12min
### [Demo]() -- 5min

# Quickstart

* Create sandbox docker, algod and indexer.

```
Pulling sandbox... POC: Dev PubSocket)
...

Checkout sandbox... (Branch: Dev PubSocket)
Branch 'develop-pubsocket' set up to track remote branch 'block-pubsub' from 'origin'.
Switched to a new branch 'develop-pubsocket'
...

algod version
12885622786
3.11.2.stable [rel/stable] (commit #99b37ac0)
go-algorand is licensed with AGPLv3.0
source code available at https://github.com/algorand/go-algorand

Indexer version
2.14.2-dev.unknown compiled at 2022-11-02T09:39:37+0000 from git hash 67ae39bf6c5971e63da41e5a2c27737884e00430 (modified)

```

* Install Insomnia and import [API File](https://raw.githubusercontent.com/ubinix-warun/algorand-webbroker/main/docs/Insomnia-All_2022-11-02.json) for demo api.

<img src="https://raw.githubusercontent.com/ubinix-warun/algorand-webbroker/main/docs/assets/screen_SubReceiverSender_Zoom.png" width="80%">

Current version, the topic type is content [ "*", "SENDER", "RECEIVER", "APPID" ].

<details>
  <summary><b><h3>Scenario 1 -- CreateTopic via WebSocket (All)</h3></b></summary>
  
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

* Run aution-demo (PyTEAL)

```
python3 -m venv venv
. venv/bin/activate

pip3 install -r requirements.txt

python3 example.py 
Generating temporary accounts...
Alice (seller account): 4IALSKYNMFQP3SZIV32STRZKKBH4IZZGDOO4ZF4CQ4HENYBRIKQGKDJK5Q
Bob (auction creator account): CF2KGOH2LUTB6ZBPU4IW3CDVEMPKB7VKT4HIRFRFGUUOWBAELHMGP4VRFE
Carla (bidder account) Q3YRLDW7TNQBM5DX46QGOQ2F5PO6WM7F4N3ENJTIZWRHRKPRD3X6BRHH3M 

Alice is generating an example NFT...
The NFT ID is 23
Alice's balances: {0: 99999000, 23: 1} 

Bob is creating an auction that lasts 30 seconds to auction off the NFT...
Done. The auction app ID is 24 and the escrow account is NSPP3V5XFPOB7NEVTYVHFZNE3AOUC4FZPDMEHA4XOBYN6OUJZIBVHTUQKI 

Alice is setting up and funding NFT auction...
Done

Alice's balances: {0: 99998099, 23: 0}
Auction escrow balances: {0: 202000, 23: 1} 

Carla wants to bid on NFT, her balances: {0: 100000100}
Carla is placing bid for 1000000 microAlgos
Carla is opting into NFT with ID 23
Done

Waiting 17 seconds for the auction to finish

Alice is closing out the auction

The auction escrow now holds the following: {0: 0}
Alice's balances after auction:  {0: 101197099, 23: 0}  Algos
Carla's balances after auction:  {0: 98997100, 23: 1}  Algos

```

</details>

<details>
  <summary><b><h3>Scenario 2 -- CreateTopic via WebSocket (RECEIVER) </h3></b></summary>
  
* Generate token for Websocket and set topic type is "RECEIVER" (Txn.).

```

> Request HTTP (POST) to  /generate/websocket with X-API-KEY="<ENV>" 

{
	"topic": {
		"type": "RECEIVER",
		"param": "QTGWOZ72DQ4SBCQGQC3T56VVROA3LXVRZFQS3JCX47KYEKG3YSQGXRGKZY"
	}
}

< Read "Token" from The response, then use 

{
	"topic": {
		"type": "RECEIVER",
		"param": "QTGWOZ72DQ4SBCQGQC3T56VVROA3LXVRZFQS3JCX47KYEKG3YSQGXRGKZY"
	},
	"Target": "",
	"Token": "Rzbf8rmt62qcaeOp"
}

```

* Subscribe BlockEvent via WebSocket

```

> Connect Websocket to /ws/Rzbf8rmt62qcaeOp with X-API-KEY="<ENV>" 
< Block Event will Streaming

```

* Run Transfer ALGO

```

./sandbox goal clerk send -a 123456789 -f P2AMOMKGLVYEJENIDN3LSS5TR3OHHBFRJHP3NYWYCPIMGTJW6CQH6K4YDY -t QTGWOZ72DQ4SBCQGQC3T56VVROA3LXVRZFQS3JCX47KYEKG3YSQGXRGKZY

```

</details>

<details>
  <summary><b><h3>Scenario 3 -- CreateTopic via Webhook (SENDER)</h3></b></summary>
  
* Generate token for Webhook and set topic type is "SENDER" (Txn.).

```

> Request HTTP (POST) to  /generate/webhook with X-API-KEY="<ENV>" 

{
	"topic": {
		"type": "SENDER",
		"param": "P2AMOMKGLVYEJENIDN3LSS5TR3OHHBFRJHP3NYWYCPIMGTJW6CQH6K4YDY"
	},
	"target": "http://172.17.0.1:8080/webhook"
}

< Read "Token" from The response, then use 

{
	"topic": {
		"type": "SENDER",
		"param": "P2AMOMKGLVYEJENIDN3LSS5TR3OHHBFRJHP3NYWYCPIMGTJW6CQH6K4YDY"
	},
	"Target": "http://172.17.0.1:8080/webhook",
	"Token": "cjg5UdOBQSvqEZc9"
}

```

* Run Hook-Receiver on backends/hook-receiver

```
npm -i
node index.js

> wait hooking

```

* Run Transfer ALGO (Again)

```

./sandbox goal clerk send -a 123456789 -f P2AMOMKGLVYEJENIDN3LSS5TR3OHHBFRJHP3NYWYCPIMGTJW6CQH6K4YDY -t QTGWOZ72DQ4SBCQGQC3T56VVROA3LXVRZFQS3JCX47KYEKG3YSQGXRGKZY

```

</details>


# Credit

* [Algorand](https://developer.algorand.org/) - Building trusted infrastructure for the Borderless economy. 
* [Echo Framework](https://echo.labstack.com/) and [Gorilla Web Toolkit](https://www.gorillatoolkit.org/) - High performance and Great Toolkit.
* [auction-demo](https://github.com/algorand/auction-demo/) - This demo is an on-chain NFT auction using smart contracts on the Algorand blockchain.
* [chainlink-polkadot](https://github.com/smartcontractkit/chainlink-polkadot/tree/master/pallet-chainlink) - This pallet allows your substrate built parachain/blockchain to interract with chainlink. 


