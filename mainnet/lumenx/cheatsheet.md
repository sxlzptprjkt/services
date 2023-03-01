---
description: >-
  Cheat sheet for set of commands node operators. From key management to chain governance.
---

# Cheat Sheet

<figure><img src="../../.gitbook/assets/lumenx.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** LumenX | **Version:** v1.3.3

## Key management

#### Add new key

```
lumenxd keys add wallet
```

#### Recover existing key

```
lumenxd keys add wallet --recover
```

#### List all keys

```
lumenxd keys list
```

#### Delete key

```
lumenxd keys delete wallet
```

#### Export key to the file

```
lumenxd keys export wallet
```

#### Import key from the file

```
lumenxd keys import wallet wallet.backup
```

#### Query wallet balance

```
lumenxd q bank balances $(lumenxd keys show wallet -a)
```

## Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```
lumenxd tx staking create-validator \
--amount=100000000000ulumen \
--pubkey=$(lumenxd tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=LumenX \
--commission-rate=0.10 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet \
--fees=5000ulumen
```

#### Edit existing validator

```
lumenxd tx staking edit-validator \
--new-moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=LumenX \
--commission-rate=0.05 \
--from=wallet \
--fees=5000ulumen
```

#### Unjail validator

```
lumenxd tx slashing unjail --broadcast-mode=block --from wallet --chain-id LumenX --fees=5000ulumen
```

#### Jail reason

```
lumenxd query slashing signing-info $(lumenxd tendermint show-validator)
```

#### List all active validators

```
lumenxd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```
lumenxd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```
lumenxd q staking validator $(lumenxd keys show wallet --bech val -a)
```

## Token management

#### Withdraw rewards from all validators

```
lumenxd tx distribution withdraw-all-rewards --from wallet --chain-id LumenX --fees=5000ulumen
```

#### Withdraw commission and rewards from your validator

```
lumenxd tx distribution withdraw-rewards $(lumenxd keys show wallet --bech val -a) --commission --from wallet --chain-id LumenX --fees=5000ulumen
```

#### Delegate tokens to yourself

```
lumenxd tx staking delegate $(lumenxd keys show wallet --bech val -a) 1000000ulumen --from wallet --chain-id LumenX --fees=5000ulumen
```

#### Delegate tokens to validator

```
lumenxd tx staking delegate <TO_VALOPER_ADDRESS> 1000000ulumen --from wallet --chain-id LumenX --fees=5000ulumen
```

#### Redelegate tokens to another validator

```
lumenxd tx staking redelegate $(lumenxd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000ulumen --from wallet --chain-id LumenX --fees=5000ulumen
```

#### Unbond tokens from your validator

```
lumenxd tx staking unbond $(lumenxd keys show wallet --bech val -a) 1000000ulumen --from wallet --chain-id LumenX --fees=5000ulumen
```

#### Send tokens to the wallet

```
lumenxd tx bank send wallet <TO_WALLET_ADDRESS> 1000000ulumen --from wallet --chain-id LumenX --fees=5000ulumen
```

## Governance

#### List all proposals

```
lumenxd query gov proposals
```

#### View proposal by id

```
lumenxd query gov proposal 1
```

#### Vote 'Yes'

```
lumenxd tx gov vote 1 yes --from wallet --chain-id LumenX --fees=5000ulumen
```

#### Vote 'No'

```
lumenxd tx gov vote 1 no --from wallet --chain-id LumenX --fees=5000ulumen
```

#### Vote 'Abstain'

```
lumenxd tx gov vote 1 abstain --from wallet --chain-id LumenX --fees=5000ulumen
```

#### Vote 'NoWithVeto'

```
lumenxd tx gov vote 1 nowithveto --from wallet --chain-id LumenX --fees=5000ulumen
```

## Utility

#### Update ports

```
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.lumenx/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.lumenx/config/config.toml
```

#### Update Indexer

##### Disable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.lumenx/config/config.toml
```

##### Enable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.lumenx/config/config.toml
```

#### Update pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.lumenx/config/config.toml
```

## Maintenance

#### Get validator info

```
lumenxd status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```
lumenxd status 2>&1 | jq .SyncInfo
```

#### Get node peer

```
echo $(lumenxd tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.lumenx/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```
[[ $(lumenxd q staking validator $(lumenxd keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(lumenxd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```
curl -sS http://localhost:26657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025ulumen\"/" $HOME/.lumenx/config/config.toml
```

#### Enable prometheus

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.lumenx/config/config.toml
```

#### Reset chain data

```
lumenxd tendermint unsafe-reset-all --home $HOME/.lumenx --keep-addr-book
```

#### Remove node

{% hint style='danger' %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv_validator_key.json**!
{% endhint %}

```
cd $HOME
sudo systemctl stop lumenxd
sudo systemctl disable lumenxd
sudo rm /etc/systemd/system/lumenxd.service
sudo systemctl daemon-reload
rm -f $(which lumenxd)
rm -rf $HOME/.lumenx
sed -i '/LUMEN_/d' ~/.bash_profile
```

## Service Management

#### Reload service configuration

```
sudo systemctl daemon-reload
```

#### Enable service

```
sudo systemctl enable lumenxd
```

#### Disable service

```
sudo systemctl disable lumenxd
```

#### Start service

```
sudo systemctl start lumenxd
```

#### Stop service

```
sudo systemctl stop lumenxd
```

#### Restart service

```
sudo systemctl restart lumenxd
```

#### Check service status

```
sudo systemctl status lumenxd
```

#### Check service logs

```
sudo journalctl -u lumenxd -f --no-hostname -o cat
```
