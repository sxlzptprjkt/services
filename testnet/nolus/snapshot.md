---
description: Catch the latest block faster by using our daily snapshots.
---

# Snapshot

<figure><img src="../../.gitbook/assets/nolus.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** nolus-rila | **Version:** v0.1.43

{% hint style='info' %}
Snapshots allows a new node to join the network by recovering application state from a backup file. 
Snapshot contains compressed copy of chain data directory. To keep backup files as small as plausible, 
snapshot server is periodically beeing state-synced.
{% endhint %}

Snapshots are taken automatically every 12 hours starting at **00:00 UTC**


| BLOCK | DOWNLOAD |
|-------|----------|
| 1206255 | [snapshot (1.15 GB)](https://snapshot.sxlzptprjkt.xyz/nolus-testnet/nolus-rila_snapshot_latest.tar.lz4) |

## Instructions

### Stop the service and reset the data

```bash
sudo systemctl stop nolusd
cp $HOME/.nolus/data/priv_validator_state.json $HOME/.nolus/priv_validator_state.json.backup
rm -rf $HOME/.nolus/data
```

### Download latest snapshot

```bash
curl -L https://snapshot.sxlzptprjkt.xyz/nolus-testnet/nolus-rila_snapshot_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.nolus
mv $HOME/.nolus/priv_validator_state.json.backup $HOME/.nolus/data/priv_validator_state.json
```

### Restart the service and check the log

```bash
sudo systemctl start nolusd && sudo journalctl -fu nolusd -o cat
```
