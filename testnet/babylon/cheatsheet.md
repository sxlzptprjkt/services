---
description: >-
  Cheat sheet for set of commands node operators. From key management to chain governance.
---

# Cheat Sheet

<figure><img src="../../.gitbook/assets/babylon.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** bbn-test1 | **Version:** v0.5.0

## Key management

#### Add new key

```
babylond keys add wallet
```

#### Recover existing key

```
babylond keys add wallet --recover
```

#### List all keys

```
babylond keys list
```

#### Delete key

```
babylond keys delete wallet
```

#### Export key to the file

```
babylond keys export wallet
```

#### Import key from the file

```
babylond keys import wallet wallet.backup
```

#### Query wallet balance

```
babylond q bank balances $(babylond keys show wallet -a)
```

## Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Before create validator you have create a BLS key

```
babylond create-bls-key $(babylond keys show wallet -a)
```

#### Create new validator

```
babylond tx checkpointing create-validator \
--amount=50ubbn \
--pubkey=$(babylond tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=bbn-test1 \
--commission-rate=0.10 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet \
--fees=20ubbn
```

#### Edit existing validator

```
babylond tx staking edit-validator \
--new-moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=bbn-test1 \
--commission-rate=0.05 \
--from=wallet \
--fees=20ubbn
```

#### Unjail validator

```
babylond tx slashing unjail --broadcast-mode=block --from wallet --chain-id bbn-test1 --fees=20ubbn
```

#### Jail reason

```
babylond query slashing signing-info $(babylond tendermint show-validator)
```

#### List all active validators

```
babylond q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```
babylond q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```
babylond q staking validator $(babylond keys show wallet --bech val -a)
```

## Token management

#### Withdraw rewards from all validators

```
babylond tx distribution withdraw-all-rewards --from wallet --chain-id bbn-test1 --fees=20ubbn
```

#### Withdraw commission and rewards from your validator

```
babylond tx distribution withdraw-rewards $(babylond keys show wallet --bech val -a) --commission --from wallet --chain-id bbn-test1 --fees=20ubbn
```

#### Delegate tokens to yourself

```
babylond tx staking delegate $(babylond keys show wallet --bech val -a) 100ubbn --from wallet --chain-id bbn-test1 --fees=20ubbn
```

#### Delegate tokens to validator

```
babylond tx staking delegate <TO_VALOPER_ADDRESS> 100ubbn --from wallet --chain-id bbn-test1 --fees=20ubbn
```

#### Redelegate tokens to another validator

```
babylond tx staking redelegate $(babylond keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 100ubbn --from wallet --chain-id bbn-test1 --fees=20ubbn
```

#### Unbond tokens from your validator

```
babylond tx staking unbond $(babylond keys show wallet --bech val -a) 100ubbn --from wallet --chain-id bbn-test1 --fees=20ubbn
```

#### Send tokens to the wallet

```
babylond tx bank send wallet <TO_WALLET_ADDRESS> 100ubbn --from wallet --chain-id bbn-test1 --fees=20ubbn
```

## Governance

#### List all proposals

```
babylond query gov proposals
```

#### View proposal by id

```
babylond query gov proposal 1
```

#### Vote 'Yes'

```
babylond tx gov vote 1 yes --from wallet --chain-id bbn-test1 --fees=20ubbn
```

#### Vote 'No'

```
babylond tx gov vote 1 no --from wallet --chain-id bbn-test1 --fees=20ubbn
```

#### Vote 'Abstain'

```
babylond tx gov vote 1 abstain --from wallet --chain-id bbn-test1 --fees=20ubbn
```

#### Vote 'NoWithVeto'

```
babylond tx gov vote 1 nowithveto --from wallet --chain-id bbn-test1 --fees=20ubbn
```

## Utility

#### Update ports

```
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.babylond/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.babylond/config/config.toml
```

#### Update Indexer

##### Disable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.babylond/config/config.toml
```

##### Enable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.babylond/config/config.toml
```

#### Update pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.babylond/config/config.toml
```

## Maintenance

#### Get validator info

```
babylond status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```
babylond status 2>&1 | jq .SyncInfo
```

#### Get node peer

```
echo $(babylond tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.babylond/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```
[[ $(babylond q staking validator $(babylond keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(babylond status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```
curl -sS http://localhost:24657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0001ubbn\"/" $HOME/.babylond/config/config.toml
```

#### Enable prometheus

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.babylond/config/config.toml
```

#### Reset chain data

```
babylond tendermint unsafe-reset-all --keep-addr-book
```

#### Remove node

{% hint style='danger' %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv_validator_key.json**!
{% endhint %}

```
cd $HOME
sudo systemctl stop babylond
sudo systemctl disable babylond
sudo rm /etc/systemd/system/babylond.service
sudo systemctl daemon-reload
rm -f $(which babylond)
rm -rf $HOME/.core
sed -i '/BBN_/d' ~/.bash_profile
```

## Service Management

#### Reload service configuration

```
sudo systemctl daemon-reload
```

#### Enable service

```
sudo systemctl enable babylond
```

#### Disable service

```
sudo systemctl disable babylond
```

#### Start service

```
sudo systemctl start babylond
```

#### Stop service

```
sudo systemctl stop babylond
```

#### Restart service

```
sudo systemctl restart babylond
```

#### Check service status

```
sudo systemctl status babylond
```

#### Check service logs

```
sudo journalctl -u babylond -f --no-hostname -o cat
```
