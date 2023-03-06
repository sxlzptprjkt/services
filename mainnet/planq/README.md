# 🟢 Planq

<figure><img src="../../.gitbook/assets/planq.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** planq_7070-2 | **Version:** v1.0.4

{% hint style="info" %}
Planq is a proof-of-stake (PoS) network that enables value transfer between Ethereum and Cosmos ecosystems. It is developed using the Cosmos software development kit (SDK). Developers can seamlessly leverage Planq to run Ethereum smart contracts on app-specific Cosmos networks. In addition, the Cosmos Inter-Blockchain Communication (IBC) protocol empowers Planq to link decentralized applications (dApps) on various blockchains and benefit from joint liquidity and value transfer. Planq makes use of Tendermint Core's Byzantine Fault Tolerance to distinguish itself from other network application layers.
{% endhint %}

#### **Explorers** : [explorer.sxlzptprjkt.xyz/planq](https://explorer.sxlzptprjkt.xyz/planq)

#### **Public Endpoints**

* RPC : [rpc-planq.sxlzptprjkt.xyz](https://rpc-planq.sxlzptprjkt.xyz)
* API : [api-planq.sxlzptprjkt.xyz](https://api-planq.sxlzptprjkt.xyz)
* gRPC : `https://grpc-planq.sxlzptprjkt.xyz:443`
* RPC EVM : `https://rpc-evm-planq.sxlzptprjkt.xyz`

#### **Setting Networks For Metamask**
- Chain Name : `Planq Mainnet`
- Chain ID : `7070`
- RPC : `https://rpc-evm-planq.sxlzptprjkt.xyz`
- Symbol : `PLQ`
- Explorer : `https://evm.planq.network`

### **Addrbook**
```bash
curl -Ls https://snapshot.sxlzptprjkt.xyz/planq/addrbook.json > $HOME/.planqd/config/addrbook.json
```

#### **Genesis**
```bash
curl -Ls https://snapshot.sxlzptprjkt.xyz/planq/genesis.json > $HOME/.planqd/config/genesis.json
```

#### **State Sync Peer**
```
b611a4058ac5caf8b56c1012c695afc75aea4217@peers-planq.sxlzptprjkt.xyz:18656
```

#### **Seeds**
```
5966b4ef17da12ee63ef30e50512ad41d541195c@seeds-planq.sxlzptprjkt.xyz:18656
```

#### **Live Peers**
```bash
PEERS="$(curl -sS https://rpc-planq.sxlzptprjkt.xyz/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}' | sed -z 's|\n|,|g;s|.$||')"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" $HOME/.planqd/config/config.toml
```
