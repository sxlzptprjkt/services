# 🟢 8Ball

<figure><img src="../../.gitbook/assets/8ball.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** eightball-1 | **Version:** v0.34.24

{% hint style="info" %}
N/A
{% endhint %}

#### **Explorers** : [explorer.sxlzptprjkt.xyz/8ball](https://explorer.sxlzptprjkt.xyz/8ball)

#### **Public Endpoints**

* RPC : [rpc-8ball.sxlzptprjkt.xyz](https://rpc-8ball.sxlzptprjkt.xyz)
* API : [api-8ball.sxlzptprjkt.xyz](https://api-8ball.sxlzptprjkt.xyz)
* gRPC : `https://grpc-8ball.sxlzptprjkt.xyz:443`

#### **State Sync Peer**
```
1745c13af10d168eca9e08dd4078818298a64ad9@146.190.83.6:23656
```

#### **Live Peers**
```
PEERS="fb1aa0a42ceadeafaecb6dfa07215006b21ea1c1@154.26.138.73:28656,fca96d0a1d7357afb226a49c4c7d9126118c37e9@80.78.22.222:26656,aa918e17c8066cd3b031f490f0019c1a95afe7e3@80.78.25.193:26656,e3eea6f0f41913f98c6c70de76fc9b0a54665f08@80.78.23.54:26656,1e4cdc78d0fd43a3c0480258c2fac4da1d70d1f0@35.238.89.136:286,df8091366fb3fdc27d771f194d015cb5706b9208@148.113.143.52:26656,98b49fea92b266ed8cfb0154028c79f81d16a825@80.78.26.202:26656,8241ecaf89c23047c187aca3bf45de06284e7925@212.227.160.56:27656,411f21da93ea5660cf05d02914f8d1b437407f28@185.193.127.112:26656,f86f14e88d18b11aa668c58ded1ac53561eb0727@209.127.202.184:28656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" $HOME/.8ball/config/config.toml
```
