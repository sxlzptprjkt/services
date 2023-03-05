# 🟢 Sao

<figure><img src="../../.gitbook/assets/sao.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** sao-testnet0 | **Version:** testnet0

{% hint style="info" %}
SAO Network is a secure and decentralized Web3 storage infrastructure based on Cosmos SDK and IPFS protocol. It aims to facilitate the adoption of Web3 storage, support the growing demand for Web3 applications and allow for a more decentralized way of storing and accessing data.
{% endhint %}

#### **Explorers** : [explorer.sxlzptprjkt.xyz/sao](https://explorer.sxlzptprjkt.xyz/sao)

#### **Public Endpoints**

* RPC : [rpc-sao.sxlzptprjkt.xyz](https://rpc-sao.sxlzptprjkt.xyz)
* API : [api-sao.sxlzptprjkt.xyz](https://api-sao.sxlzptprjkt.xyz)
* gRPC : `https://grpc-sao.sxlzptprjkt.xyz:443`

#### **Addrbook**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/sao/addrbook.json > $HOME/.sao/config/addrbook.json
```

#### **Genesis**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/sao/genesis.json > $HOME/.sao/config/genesis.json
```

#### **State Sync Peer**
```
2aad459c0dd3a81b1d5eb297986c8d8309ad20e3@peers-sao.sxlzptprjkt.xyz:27656
```

#### **Live Peers**
```
PEERS="$(curl -sS https://rpc-sao.sxlzptprjkt.xyz/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}' | sed -z 's|\n|,|g;s|.$||')"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" $HOME/.sao/config/config.toml
```
