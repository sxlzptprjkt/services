# 🤝 Games Of Alliance (Terra)

<figure><img src="../../../.gitbook/assets/harkonnen.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** harkonnen-1 | **Version:** v0.1.0-goa

{% hint style="info" %}
Alliance is an open-source Cosmos SDK module that leverages interchain staking to form economic alliances among blockchains. By boosting the economic activity across Cosmos chains through creating bilateral, mutually beneficial alliances, Alliance aims to give rise to a new wave of innovation, user adoption, and cross-chain collaboration.
{% endhint %}

#### **Explorers** : [explorer.sxlzptprjkt.xyz/goa-harkonnen](https://explorer.sxlzptprjkt.xyz/goa-harkonnen)

#### **Public Endpoints**

* RPC : [rpc-goa-harkonnen.sxlzptprjkt.xyz](https://rpc-goa-harkonnen.sxlzptprjkt.xyz)
* API : [api-goa-harkonnen.sxlzptprjkt.xyz](https://api-goa-harkonnen.sxlzptprjkt.xyz)
* gRPC : `https://grpc-goa-harkonnen.sxlzptprjkt.xyz:443`

#### **State Sync Peer**
```
3a3d0eaa086d8a62b7c00d1177977244bc7d3ebb@146.190.81.135:02656
```

#### **Addrbook**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/goa/harkonnen/addrbook.json > $HOME/.ordos/config/addrbook.json
```

#### **Genesis**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/goa/harkonnen/genesis.json > $HOME/.ordos/config/genesis.json
```

#### **Live Peers**
```
PEERS="a9e7be36771d6094aed4751c128dc00ae9e17560@91.107.224.161:26656,5e3c91218db80ae1952f24975d5f1ceef4100e8c@142.132.199.211:26652,f91ab308620f773459dbffa7be2a9cf8df280df4@65.108.205.47:27656,15e474a5163a3e63d4030c14e6e42cfd6e4d5afc@35.168.16.221:41156,a3292681e6f2e263efffc09fb7bef49fb21b1f15@52.91.39.40:41156,b0e33436e296bf69fd7bc5a344ce4b852796ef29@65.109.89.5:35656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" $HOME/.harkonnen/config/config.toml
```
