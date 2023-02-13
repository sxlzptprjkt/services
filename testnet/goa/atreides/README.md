# 🤝 Games Of Alliance (Terra)

<figure><img src="../../../.gitbook/assets/atreides.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** atreides-1 | **Version:** v0.0.1-goa

{% hint style="info" %}
Alliance is an open-source Cosmos SDK module that leverages interchain staking to form economic alliances among blockchains. By boosting the economic activity across Cosmos chains through creating bilateral, mutually beneficial alliances, Alliance aims to give rise to a new wave of innovation, user adoption, and cross-chain collaboration.
{% endhint %}

#### **Explorers** : [explorer.sxlzptprjkt.xyz/goa-atreides](https://explorer.sxlzptprjkt.xyz/goa-atreides)

#### **Public Endpoints**

* RPC : [rpc-goa-atreides.sxlzptprjkt.xyz](https://rpc-goa-atreides.sxlzptprjkt.xyz)
* API : [api-goa-atreides.sxlzptprjkt.xyz](https://api-goa-atreides.sxlzptprjkt.xyz)
* gRPC : [grpc-goa-atreides.sxlzptprjkt.xyz](https://grpc-goa-atreides.sxlzptprjkt.xyz)

#### **State Sync Peer**
```
58c31664ab2888515ead08fac36f92d36ee843c9@146.190.83.6:04656
```

#### **Addrbook**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/goa/atreides/addrbook.json > $HOME/.atreides/config/addrbook.json
```

#### **Genesis**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/goa/atreides/genesis.json > $HOME/.atreides/config/genesis.json
```

#### **Live Peers**
```
PEERS="328d58b2ca2c615dec7b99ec6f4fb2b4b1ecd79e@142.132.199.211:26651,1719e57fb35f15ee3ee79ba2b85b4852cf9eedf3@161.35.34.119:26656,424aff0dc8e542fa93128e68388f3ead3637463c@142.132.151.99:15660,7eb0e1b726bec4befd41288bdf2f3d67c2fa5f8e@162.55.83.146:26656,9825649a2baa23919c31d0a95fce8387b8285c94@136.243.103.32:29656,c90eb44fb67dc4e703db9711aea2b0e3bdac797d@168.119.50.205:26656,827d01410f3ec45ac108ce591faff1c410be5078@65.108.127.50:21156,eb41aad6d63d5a991573d2f9f3c2cfdb7fbc32e0@65.109.93.35:26656,36b2547e91dbaa1a6196217f25b767a8630fb0b2@54.196.186.174:41456,19efe6c44ec7e09f9a0f3b918177c6036c9eecc9@141.95.124.134:15660"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" $HOME/.atreides/config/config.toml
```
