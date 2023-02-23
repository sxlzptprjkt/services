# 🟢 Coreum

<figure><img src="../../.gitbook/assets/coreum.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** coreum-testnet-1 | **Version:** v0.1.1

{% hint style="info" %}
Coreum is a fast Layer-1 Blockchain with smart contract, set as a core infrastructure for future decentralized apps and DeFi.
{% endhint %}

#### **Explorers** : [explorer.sxlzptprjkt.xyz/coreum](https://explorer.sxlzptprjkt.xyz/coreum)

#### **Public Endpoints**

* RPC : [rpc-coreum.sxlzptprjkt.xyz](https://rpc-coreum.sxlzptprjkt.xyz)
* API : [api-coreum.sxlzptprjkt.xyz](https://api-coreum.sxlzptprjkt.xyz)
* gRPC : `https://grpc-coreum.sxlzptprjkt.xyz:443`

#### **State Sync Peer**
```
479773376706c0643289a365e84e440cced10bb9@146.190.81.135:21656
```

#### **Live Peers**
```
PEERS="bcf4358ce13d9952ecdb01591f7106ce4dcab8d3@152.32.191.216:26656,051a07f1018cfdd6c24bebb3094179a6ceda2482@138.201.123.234:26656,1a3a573c53a4b90ab04eb47d160f4d3d6aa58000@35.233.117.165:26656,39a34cd4f1e908a88a726b2444c6a407f67e4229@158.160.59.199:26656,4b8d541efbb343effa1b5079de0b17d2566ac0fd@34.172.70.24:26656,7c0d4ce5ad561c3453e2e837d85c9745b76f7972@35.238.77.191:26656,5add70ec357311d07d10a730b4ec25107399e83c@5.196.7.58:26656,27450dc5adcebc84ccd831b42fcd73cb69970881@35.239.146.40:26656,abbeb588ad88176a8d7592cd8706ebbf7ef20cfe@185.241.151.197:26656,b2978432c0126f28a6be7d62892f8ded1e48d227@34.70.241.13:26656,cc6d4220633104885b89e2e0545e04b8162d69b5@75.119.134.20:26656,69d7028b7b3c40f64ea43208ecdd43e88c797fd6@34.69.126.231:26656,ef213d5b861dd96cb98e53fa14a40accacc3145a@104.248.125.89:26656,0aa5fa2507ada8a555d156920c0b09f0d633b0f9@34.173.227.148:26656"
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$PEERS\"|" $HOME/.core/coreum-testnet-1/config/config.toml
```
