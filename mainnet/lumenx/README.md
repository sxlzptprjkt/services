# 🟢 Lumenx

<figure><img src="../../.gitbook/assets/lumenx.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** LumenX | **Version:** v1.3.3

{% hint style="info" %}
LumenX is the blockchain built using the Cosmos SDK. LumenX will be interact with other sovereign blockchains using a protocol called IBC that enables Inter-Blockchain Communication.
{% endhint %}

#### **Explorers** : [explorer.sxlzptprjkt.xyz/lumenx](https://explorer.sxlzptprjkt.xyz/lumenx)

#### **Public Endpoints**

* RPC : [rpc-lumenx.sxlzptprjkt.xyz](https://rpc-lumenx.sxlzptprjkt.xyz)
* API : [api-lumenx.sxlzptprjkt.xyz](https://api-lumenx.sxlzptprjkt.xyz)
* gRPC : `https://grpc-lumenx.sxlzptprjkt.xyz:443`

### **Addrbook**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/lumenx/addrbook.json > $HOME/.lumenx/config/addrbook.json
```

#### **Genesis**
```
curl -Ls https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/lumenx/genesis.json > $HOME/.lumenx/config/genesis.json
```

#### **State Sync Peer**
```
39674b41ec5ffaf275977a147163c544e3fda03a@peers-lumenx.sxlzptprjkt.xyz:26656
```

#### **Seeds**
```
ff14d88ffa802336e37632f4deac3eac638a4e95@seeds-lumenx.sxlzptprjkt.xyz:26656
```

#### **Live Peers**
```
PEERS="5e6f8000527d83896d610d9486e78237f1e91868@89.163.215.6:22656,f5a517c682466dac525ce87ea9c2f2cbc8c4f002@38.242.233.215:26656,43c4eb952a35df720f2cb4b86a73b43f682d6cb1@37.187.149.93:26696,1d94c81f6b25a51be173d22523f6267113bfcbec@45.134.226.70:26656,9a49635f0ecb7ba93fc9eba952cbe58767557010@185.215.180.70:26656,3b584334f64ab60f92388ea22bc870dcacf4c157@157.90.179.182:56656,cd6febf26168c82df99c8209ee82fecbb21ccfff@5.9.61.78:56656,dc32e90bf2321b220bc2346fa01425117372107a@136.243.136.241:22656,e91a86a4bec23993f584f346208e7b47285eb632@65.21.226.230:27656,7e8d80b7e68c8d5acdde2322af53c4170e3de86a@128.199.133.207:26656,2e8e6f4754f33f93ad7c9d5de7f51c4bf181c4d8@51.89.195.66:11656,12070c60d3819740e8be7556917b9a283b72c0d6@65.108.9.164:13856,3c7c6c284806053c21b0e0dbfd3ca59797eab1d7@65.108.7.44:51656,3e1f365d136936a1a41cb47e2501dd358c189979@62.171.145.215:26666,8246854d88bbba7acec7b4d86c9b418c90816f1f@88.99.161.162:24656,8c1dac06d455b5895f6d90d879b03449cdc14a41@194.163.167.138:58656,39d8e366837505e3a31948d761cc08ac8ed4a44b@188.165.232.199:26666,d605a4e19a75568297220e43148aa613724091d0@213.133.103.188:13856,b9aee01d4a878d0cf6beff20cabc9d4659cdd441@65.108.44.100:27656,e3989262b8dff3596f3b1d5e44372e9326362552@192.99.4.66:26666,0f47b636ebcbfd1fd6b3cfd39f4bd15aeab73079@157.245.49.140:26656,0da8132a62468581db52774e9a513eb032179edd@45.94.58.246:17656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" $HOME/.lumenx/config/config.toml
```
