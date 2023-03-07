# 🟢 Ojo

<figure><img src="../../.gitbook/assets/ojo.png" alt=""><figcaption></figcaption></figure>

**Network:** Devnet | **Chain ID:** ojo-devnet | **Version:** v0.1.2

{% hint style="info" %}
Ojo is a decentralized security-first oracle network built to support the Cosmos Ecosystem. Ojo will source price data from a diverse catalog of on and off-chain sources and use advanced security mechanisms to guarantee the integrity of the data it provides.
{% endhint %}

[**Website**](https://ojo.network) | [**Discord**](https://discord.gg/8S6JBjvN) | [**Twitter**](https://twitter.com/ojo_network)

#### **Explorers** : [explorer.sxlzptprjkt.xyz/ojo-devnet](https://explorer.sxlzptprjkt.xyz/ojo-devnet)

#### **Public Endpoints**

* RPC : [rpc-ojo-devnet.sxlzptprjkt.xyz](https://rpc-ojo-devnet.sxlzptprjkt.xyz)
* API : [api-ojo-devnet.sxlzptprjkt.xyz](https://api-ojo-devnet.sxlzptprjkt.xyz)
* gRPC : `https://grpc-ojo-devnet.sxlzptprjkt.xyz:443`

#### **Addrbook**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/devnet/ojo/addrbook.json > $HOME/.ojo/config/addrbook.json
```

#### **Genesis**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/devnet/ojo/genesis.json > $HOME/.ojo/config/genesis.json
```

#### **State Sync Peer**
```
0ccc4bd8386fbec1421e3c19c24124eeb00b3293@peers-ojo-devnet.sxlzptprjkt.xyz:28656
```

#### **Seeds**
```
5a36595613f189a3c1096729897fb02be0a8c15e@seeds-ojo-devnet.sxlzptprjkt.xyz:28656
```

#### **Live Peers**
```
PEERS="$(curl -sS https://rpc-ojo-devnet.sxlzptprjkt.xyz/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}' | sed -z 's|\n|,|g;s|.$||')"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" $HOME/.ojo/config/config.toml
```
