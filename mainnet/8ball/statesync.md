---
description: >-
  With our state sync services you will be able to catch up latest chain block
  in matter of minutes
---

# State Sync

<figure><img src="../../.gitbook/assets/8ball.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** eightball-1 | **Version:** v0.34.24

{% hint style="info" %}
State Sync allows a new node to join the network by fetching a snapshot of the application state at a recent height instead of fetching and replaying all historical blocks. Since the application state is generally much smaller than the blocks, and restoring it is much faster than replaying blocks, this can reduce the time to sync with the network from days to minutes.
{% endhint %}

### Instruction

### **Our state-sync server setup**
Our `app.toml` settings related to state-sync is as follows. This is for you information only. You do not need to follow the same setup on your node

```
# Prune Type
pruning = "custom"

# Prune Strategy
pruning-keep-every = 0

# State-Sync Snapshot Strategy
snapshot-interval = 1000
snapshot-keep-recent = 2
```

Our state-sync RPC server for 8ball is :
```
https://rpc-8ball.sxlzptprjkt.xyz:443
```

#### **Stop the service and reset the data**

```
sudo systemctl stop 8ball
cp $HOME/.8ball/data/priv_validator_state.json $HOME/.8ball/priv_validator_state.json.backup
8ball tendermint unsafe-reset-all --home $HOME/.8ball --keep-addr-book
```

#### **Configure state sync information**

```
SNAP_RPC="https://rpc-coreum.sxlzptprjkt.xyz:443"
STATESYNC_PEERS="1745c13af10d168eca9e08dd4078818298a64ad9@146.190.83.6:23656"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.8ball/config/config.toml
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$STATESYNC_PEERS\"|" $HOME/.8ball/config/config.toml

mv $HOME/.8ball/priv_validator_state.json.backup $HOME/.8ball/data/priv_validator_state.json
```

#### **Start service and check logs**

```
sudo systemctl start 8ball && sudo journalctl -fu 8ball -o cat
```
