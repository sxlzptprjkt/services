# 🤝 Games Of Alliance (Terra)

<figure><img src="../../../.gitbook/assets/corrino.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** corrino-1 | **Version:** v0.1.0-goa

{% hint style="info" %}
Alliance is an open-source Cosmos SDK module that leverages interchain staking to form economic alliances among blockchains. By boosting the economic activity across Cosmos chains through creating bilateral, mutually beneficial alliances, Alliance aims to give rise to a new wave of innovation, user adoption, and cross-chain collaboration.
{% endhint %}

#### **Explorers** : [explorer.sxlzptprjkt.xyz/goa-corrino](https://explorer.sxlzptprjkt.xyz/goa-corrino)

#### **Public Endpoints**

* RPC : [rpc-goa-corrino.sxlzptprjkt.xyz](https://rpc-goa-corrino.sxlzptprjkt.xyz)
* API : [api-goa-corrino.sxlzptprjkt.xyz](https://api-goa-corrino.sxlzptprjkt.xyz)
* gRPC : [grpc-goa-corrino.sxlzptprjkt.xyz](https://grpc-goa-corrino.sxlzptprjkt.xyz)

#### **State Sync Peer**
```
abd7c8d502f7f27ed2bf11d0ca34c7e9e1186e79@146.190.83.6:03656
```

#### **Addrbook**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/goa/corrino/addrbook.json > $HOME/.corrino/config/addrbook.json
```

#### **Genesis**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/goa/corrino/genesis.json > $HOME/.corrino/config/genesis.json
```

#### **Live Peers**
```
PEERS="aa27dbadcaa1ff2c473b8380e28a9e17be7156e6@65.21.131.215:27656,5260976afec974fc0dea05be875841b126a6e322@54.196.186.174:41256,489ac19377a00b0da57a44bfb85366870c900285@52.91.39.40:41256,b59f1343587f64047ad331fd8ca8382887d34233@35.168.16.221:41256"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" $HOME/.corrino/config/config.toml
```
