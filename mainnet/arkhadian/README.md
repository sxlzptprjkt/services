# 🟢 Arkhadian

<figure><img src="../../.gitbook/assets/arkh.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** arkh | **Version:** v2.0.0

{% hint style="info" %}
A blockchain is basically a shared database, which is why it is also known as a distributed ledger (in the sense of ledger) (although distributed ledgers can be based on other technologies). Blockchain differs from traditional database technology: instead of a single database managed by a single owner who shares data, in the blockchain network the network participants have their own copy of the database. The blockchain mechanism can ensure unanimous agreement on the correct content of data, ensure compliance of agreed copies of data, and ensure the subsequent absence of cheating by data tampering. This allows a number of people or entities collaborators or competitors to agree on a consensus on information and to immutably record this consensus of truth. For this reason, blockchain has been described as a "trust solution".
{% endhint %}

#### **Explorers** : [explorer.sxlzptprjkt.xyz/arkhadian](https://explorer.sxlzptprjkt.xyz/arkhadian)

#### **Public Endpoints**

* RPC : [rpc-arkh.sxlzptprjkt.xyz](https://rpc-arkh.sxlzptprjkt.xyz)
* API : [api-arkh.sxlzptprjkt.xyz](https://api-arkh.sxlzptprjkt.xyz)
* gRPC : `https://grpc-arkh.sxlzptprjkt.xyz:443`

### **Addrbook**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/arkh/addrbook.json > $HOME/.arkh/config/addrbook.json
```

#### **Genesis**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/arkh/genesis.json > $HOME/.arkh/config/genesis.json
```

#### **State Sync Peer**
```
b0786057a6bcc1313477fcceaea9c78356078c6d@46.101.144.90:25656
```

#### **Live Peers**
```
PEERS="35ab45eafaed0722f90622fc2c23d01739df25c5@207.154.243.48:18656,889e31730df026e6cec506e26a0791368f8073a2@162.19.236.117:26656,8326d9d921afc60a2c9e7b57a48c51f7f2ae7e81@137.184.180.170:18656,f7b5d20f636fe7c2ec504662834b35b0cc56a742@194.163.165.174:37656,687dd5f8cfb62f63dc4bb04a28cfdb3225bff2e5@95.216.75.119:13756,b39fec8beed5a72e77a10d213dc2c38ce9909e8b@45.141.122.178:35656,ce0cde42967aa085bca9d66c0e5695d6341c778a@165.22.76.250:18656,9b1bda1f94e73ae71d4e3188d85da10d0a763ac2@195.3.221.58:13756,1af8fdecd6e8f9ec1bfcc3288fe46ce45e4df963@144.76.97.251:39656,861ef652578905f27e7ea1f6d36b68fda08751f5@35.192.119.28:26656,cc830584e010e111d75cba359475d1e9fd091139@146.190.40.38:26656,9f7b574bf3a30ece3083dd6d0271d9ca617c8ddc@134.209.21.58:18656,f66afec8a1fb6c06f017a115433deee4bbf588aa@88.99.161.162:23656,9fcb65ca60bada22cfb25d46f6fc6dbe93740c26@195.201.83.242:13756,cccb1a885cffeb3de745996de8c3161fcd499dff@85.239.230.14:13756,82e425a51c75e60663d310deea144a50c5206f0b@167.99.234.217:13756,1f39ccff07110aa887e0f24cee17404d41084dd9@45.151.123.72:18656,ef7245f080d957b0505a8b670c37861faaf368f0@167.99.69.130:13756,c0cc8b6c9e42f2a2f12e2bc5b354baa51d176a66@173.212.222.167:32656,d747ddf72464065bc7d221b500b2c0e65ff34ff9@194.233.68.136:13756,71d76b7d5a90c9f289335032c3af6b1a0ecce2e9@89.117.50.187:25656,bb281b7b461cbd06dcda0220bd033207ce9b594e@213.133.103.188:13756,f8055e1cd617ce0fb7848cf759e540f1f06009e6@164.92.239.92:18656,7f14aaa9b8bac1b2762a7863400e427f82196976@146.190.40.115:18656,02c049b6683c3a22ee83a1ce888c06b188bbcf6c@165.232.126.250:18656,344372a4883988741b462223790e2b28e5be1d38@81.0.218.58:13756,4835144689fb4819bada135f92c1a83b2f84c0bb@3.138.135.123:26656,1570da59051aef97736db1c16d3cf04dbe8ee7fd@194.163.167.138:56756,b4f3bd0b9202be699635966978b44e5ea8ab9fba@34.173.89.239:26656,a4c02968458f3f51492bb1ee026b853e8d1c1428@46.4.75.21:13756"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" $HOME/.arkh/config/config.toml
```
