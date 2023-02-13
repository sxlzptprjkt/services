---
description: >-
  With our state sync services you will be able to catch up latest chain block
  in matter of minutes
---

# State Sync

<figure><img src="../../../.gitbook/assets/harkonnen.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** harkonnen-1 | **Version:** v0.0.1-goa

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

Our state-sync RPC server for harkonnen is :
```
https://rpc-goa-harkonnen.sxlzptprjkt.xyz:443
```

#### **Stop the service and reset the data**

```
sudo systemctl stop harkonnend
cp $HOME/.harkonnen/data/priv_validator_state.json $HOME/.harkonnen/priv_validator_state.json.backup
harkonnend tendermint unsafe-reset-all --home $HOME/.harkonnen --keep-addr-book
```

#### **Configure state sync information**

```
SNAP_RPC="https://rpc-goa-harkonnen.sxlzptprjkt.xyz:443"
STATESYNC_PEERS="3a3d0eaa086d8a62b7c00d1177977244bc7d3ebb@146.190.81.135:02656"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.harkonnen/config/config.toml
sed -i -e "s|^persistent_peers *=.*|persistent_peers = \"$STATESYNC_PEERS\"|" $HOME/.harkonnen/config/config.toml

mv $HOME/.harkonnen/priv_validator_state.json.backup $HOME/.harkonnen/data/priv_validator_state.json
```

#### **Start service and check logs**

```
sudo systemctl start harkonnend && sudo journalctl -fu harkonnend -o cat
```
