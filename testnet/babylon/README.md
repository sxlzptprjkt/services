# 🟢 Babylon

<figure><img src="../../.gitbook/assets/babylon.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** bbn-test1 | **Version:** v0.5.0

{% hint style="info" %}
Babylon leverages cutting-edge cryptographic technologies and advanced Cosmos SDK features to send succinct, verifiable, and adversary-slashing checkpoints to the Bitcoin network for timestamping. The size of each checkpoint is merely about 150 bytes, but it can cover hundreds of Babylon blocks and all the transactions therein. Babylon also executes lightweight vigilante programs that bridge Babylon and BTC and monitor their consistency.
{% endhint %}

#### **Explorers** : [explorer.sxlzptprjkt.xyz/babylon](https://explorer.sxlzptprjkt.xyz/babylon)

#### **Public Endpoints**

* RPC : [rpc-babylon.sxlzptprjkt.xyz](https://rpc-babylon.sxlzptprjkt.xyz)
* API : [api-babylon.sxlzptprjkt.xyz](https://api-babylon.sxlzptprjkt.xyz)
* gRPC : [grpc-babylon.sxlzptprjkt.xyz](https://grpc-babylon.sxlzptprjkt.xyz)

#### **Genesis**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/babylon/genesis.json > $HOME/.ordos/config/genesis.json
```

#### **State Sync Peer**
```
4ffd7f9202c58df4afec210f22da732023e476c8@46.101.144.90:24656
```

#### **Live Peers**
```
PEERS="f71a8fe9aa8f0e423dea7c98463f5d9a47549284@184.174.32.227:26656,a8051774e809d8dc14673bb245abc0fc48a3f684@5.9.122.49:14656,07d1b69e4dc56d46dabe8f5eb277fcde0c6c9d1e@23.88.5.169:17656,a4f76dddb6bdb195a0e49be82a3fd789d98631df@65.109.85.170:55656,42dd05c43fa9e51cfabc6a2ab0afa9044b123cc6@34.201.34.29:26656,0229c552a8f2331d04e287948e181d4497ae374f@144.76.67.53:2570,81f15406e9a669efcb4d536c2eb12a4e74108d58@65.108.232.238:14656,cd9d96f554e7298a8d1f1a94489f7a51520f01ff@142.132.152.46:47656,1d0c78d6945ac4007dafef2a130e532c07b806d2@65.108.105.48:20656,c1406917c620090ae59f7301c7b3c9d1864d91cb@85.10.192.146:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" $HOME/.babylond/config/config.toml
```
