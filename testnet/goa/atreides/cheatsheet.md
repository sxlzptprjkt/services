---
description: >-
  Cheat sheet for set of commands node operators. From key management to chain governance.
---

# Cheat Sheet

<figure><img src="../../../.gitbook/assets/atreides.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** atreides-1 | **Version:** v0.1.0-goa

## Key management

#### Add new key

```
atreidesd keys add wallet
```

#### Recover existing key

```
atreidesd keys add wallet --recover
```

#### List all keys

```
atreidesd keys list
```

#### Delete key

```
atreidesd keys delete wallet
```

#### Export key to the file

```
atreidesd keys export wallet
```

#### Import key from the file

```
atreidesd keys import wallet wallet.backup
```

#### Query wallet balance

```
atreidesd q bank balances $(atreidesd keys show wallet -a)
```

## Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```
atreidesd tx staking create-validator \
--amount=5000000uatr \
--pubkey=$(atreidesd tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=atreides-1 \
--commission-rate=0.01 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1000000 \
--from=wallet \
--fees="1000uatr" \
--gas="1000000" \
--gas-adjustment="1.15"
```

#### Edit existing validator

```
atreidesd tx staking edit-validator \
--new-moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=atreides-1 \
--commission-rate=0.05 \
--from=wallet \
--fees="1000uatr" \
--gas="1000000" \
--gas-adjustment="1.15"
```

#### Unjail validator

```
atreidesd tx slashing unjail --broadcast-mode=block --from wallet --chain-id atreides-1 --fees="1000uatr" --gas="1000000" --gas-adjustment="1.15"
```

#### Jail reason

```
atreidesd query slashing signing-info $(atreidesd tendermint show-validator)
```

#### List all active validators

```
atreidesd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```
atreidesd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```
atreidesd q staking validator $(atreidesd keys show wallet --bech val -a)
```

## Token management

#### Withdraw rewards from all validators

```
atreidesd tx distribution withdraw-all-rewards --from wallet --chain-id atreides-1 --fees="1000uatr" --gas="1000000" --gas-adjustment="1.15"
```

#### Withdraw commission and rewards from your validator

```
atreidesd tx distribution withdraw-rewards $(atreidesd keys show wallet --bech val -a) --commission --from wallet --chain-id atreides-1 --fees="1000uatr" --gas="1000000" --gas-adjustment="1.15"
```

#### Delegate tokens to yourself

```
atreidesd tx staking delegate $(atreidesd keys show wallet --bech val -a) 1000000uatr --from wallet --chain-id atreides-1 --fees="1000uatr" --gas="1000000" --gas-adjustment="1.15"
```

#### Delegate tokens to validator

```
atreidesd tx staking delegate <TO_VALOPER_ADDRESS> 1000000uatr --from wallet --chain-id atreides-1 --fees="1000uatr" --gas="1000000" --gas-adjustment="1.15"
```

#### Redelegate tokens to another validator

```
atreidesd tx staking redelegate $(atreidesd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uatr --from wallet --chain-id atreides-1 --fees="1000uatr" --gas="1000000" --gas-adjustment="1.15"
```

#### Unbond tokens from your validator

```
atreidesd tx staking unbond $(atreidesd keys show wallet --bech val -a) 1000000uatr --from wallet --chain-id atreides-1 --fees="1000uatr" --gas="1000000" --gas-adjustment="1.15"
```

#### Send tokens to the wallet

```
atreidesd tx bank send wallet <TO_WALLET_ADDRESS> 1000000uatr --from wallet --chain-id atreides-1 --fees="1000uatr" --gas="1000000" --gas-adjustment="1.15"
```

## Governance

#### List all proposals

```
atreidesd query gov proposals
```

#### View proposal by id

```
atreidesd query gov proposal 1
```

#### Vote 'Yes'

```
atreidesd tx gov vote 1 yes --from wallet --chain-id atreides-1 --fees="1000uatr" --gas="1000000" --gas-adjustment="1.15"
```

#### Vote 'No'

```
atreidesd tx gov vote 1 no --from wallet --chain-id atreides-1 --fees="1000uatr" --gas="1000000" --gas-adjustment="1.15"
```

#### Vote 'Abstain'

```
atreidesd tx gov vote 1 abstain --from wallet --chain-id atreides-1 --fees="1000uatr" --gas="1000000" --gas-adjustment="1.15"
```

#### Vote 'NoWithVeto'

```
atreidesd tx gov vote 1 nowithveto --from wallet --chain-id atreides-1 --fees="1000uatr" --gas="1000000" --gas-adjustment="1.15"
```

## Utility

#### Update ports

```
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.atreides/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.atreides/config/config.toml
```

#### Update Indexer

##### Disable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.atreides/config/config.toml
```

##### Enable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.atreides/config/config.toml
```

#### Update pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.atreides/config/config.toml
```

## Maintenance

#### Get validator info

```
atreidesd status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```
atreidesd status 2>&1 | jq .SyncInfo
```

#### Get node peer

```
echo $(atreidesd tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.atreides/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```
[[ $(atreidesd q staking validator $(atreidesd keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(atreidesd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```
curl -sS http://localhost:04657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0uatr\"/" $HOME/.atreides/config/config.toml
```

#### Enable prometheus

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.atreides/config/config.toml
```

#### Reset chain data

```
atreidesd tendermint unsafe-reset-all --home $HOME/.atreides --keep-addr-book
```

#### Remove node

{% hint style='danger' %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv_validator_key.json**!
{% endhint %}

```
cd $HOME
sudo systemctl stop atreidesd
sudo systemctl disable atreidesd
sudo rm -f /etc/systemd/system/atreidesd.service
sudo systemctl daemon-reload
rm -f $(which atreidesd)
rm -rf $HOME/.atreides
sed -i '/ATREIDES_/d' ~/.bash_profile
```

## Service Management

#### Reload service configuration

```
sudo systemctl daemon-reload
```

#### Enable service

```
sudo systemctl enable atreidesd
```

#### Disable service

```
sudo systemctl disable atreidesd
```

#### Start service

```
sudo systemctl start atreidesd
```

#### Stop service

```
sudo systemctl stop atreidesd
```

#### Restart service

```
sudo systemctl restart atreidesd
```

#### Check service status

```
sudo systemctl status atreidesd
```

#### Check service logs

```
sudo journalctl -u atreidesd -f --no-hostname -o cat
```
