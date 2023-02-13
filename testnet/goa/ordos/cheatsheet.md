---
description: >-
  Cheat sheet for set of commands node operators. From key management to chain governance.
---

# Cheat Sheet

<figure><img src="../../../.gitbook/assets/ordos.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** ordos-1 | **Version:** v0.0.1-goa

## Key management

#### Add new key

```
ordosd keys add wallet
```

#### Recover existing key

```
ordosd keys add wallet --recover
```

#### List all keys

```
ordosd keys list
```

#### Delete key

```
ordosd keys delete wallet
```

#### Export key to the file

```
ordosd keys export wallet
```

#### Import key from the file

```
ordosd keys import wallet wallet.backup
```

#### Query wallet balance

```
ordosd q bank balances $(ordosd keys show wallet -a)
```

## Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```
ordosd tx staking create-validator \
--amount=5000000uord \
--pubkey=$(ordosd tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=ordos-1 \
--commission-rate=0.01 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1000000 \
--from=wallet \
--fees=500uord
```

#### Edit existing validator

```
ordosd tx staking edit-validator \
--new-moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=ordos-1 \
--commission-rate=0.05 \
--from=wallet \
--fees=500uord
```

#### Unjail validator

```
ordosd tx slashing unjail --broadcast-mode=block --from wallet --chain-id ordos-1 --fees=500uord
```

#### Jail reason

```
ordosd query slashing signing-info $(ordosd tendermint show-validator)
```

#### List all active validators

```
ordosd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```
ordosd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```
ordosd q staking validator $(ordosd keys show wallet --bech val -a)
```

## Token management

#### Withdraw rewards from all validators

```
ordosd tx distribution withdraw-all-rewards --from wallet --chain-id ordos-1 --fees=500uord
```

#### Withdraw commission and rewards from your validator

```
ordosd tx distribution withdraw-rewards $(ordosd keys show wallet --bech val -a) --commission --from wallet --chain-id ordos-1 --fees=500uord
```

#### Delegate tokens to yourself

```
ordosd tx staking delegate $(ordosd keys show wallet --bech val -a) 1000000uord --from wallet --chain-id ordos-1 --fees=500uord
```

#### Delegate tokens to validator

```
ordosd tx staking delegate <TO_VALOPER_ADDRESS> 1000000uord --from wallet --chain-id ordos-1 --fees=500uord
```

#### Redelegate tokens to another validator

```
ordosd tx staking redelegate $(ordosd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uord --from wallet --chain-id ordos-1 --fees=500uord
```

#### Unbond tokens from your validator

```
ordosd tx staking unbond $(ordosd keys show wallet --bech val -a) 1000000uord --from wallet --chain-id ordos-1 --fees=500uord
```

#### Send tokens to the wallet

```
ordosd tx bank send wallet <TO_WALLET_ADDRESS> 1000000uord --from wallet --chain-id ordos-1 --fees=500uord
```

## Governance

#### List all proposals

```
ordosd query gov proposals
```

#### View proposal by id

```
ordosd query gov proposal 1
```

#### Vote 'Yes'

```
ordosd tx gov vote 1 yes --from wallet --chain-id ordos-1 --fees=500uord
```

#### Vote 'No'

```
ordosd tx gov vote 1 no --from wallet --chain-id ordos-1 --fees=500uord
```

#### Vote 'Abstain'

```
ordosd tx gov vote 1 abstain --from wallet --chain-id ordos-1 --fees=500uord
```

#### Vote 'NoWithVeto'

```
ordosd tx gov vote 1 nowithveto --from wallet --chain-id ordos-1 --fees=500uord
```

## Utility

#### Update ports

```
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.ordosd/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.ordosd/config/config.toml
```

#### Update Indexer

##### Disable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.ordosd/config/config.toml
```

##### Enable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.ordosd/config/config.toml
```

#### Update pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.ordosd/config/config.toml
```

## Maintenance

#### Get validator info

```
ordosd status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```
ordosd status 2>&1 | jq .SyncInfo
```

#### Get node peer

```
echo $(ordosd tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.ordosd/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```
[[ $(ordosd q staking validator $(ordosd keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(ordosd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```
curl -sS http://localhost:01657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0uord\"/" $HOME/.ordosd/config/config.toml
```

#### Enable prometheus

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.ordosd/config/config.toml
```

#### Reset chain data

```
ordosd tendermint unsafe-reset-all --home $HOME/.ordosd --keep-addr-book
```

#### Remove node

{% hint style='danger' %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv_validator_key.json**!
{% endhint %}

```
cd $HOME
sudo systemctl stop ordosd
sudo systemctl disable ordosd
sudo rm -f /etc/systemd/system/ordosd.service
sudo systemctl daemon-reload
rm -f $(which ordosd)
rm -rf $HOME/.ordos
sed -i '/ORDOS_/d' ~/.bash_profile
```

## Service Management

#### Reload service configuration

```
sudo systemctl daemon-reload
```

#### Enable service

```
sudo systemctl enable ordosd
```

#### Disable service

```
sudo systemctl disable ordosd
```

#### Start service

```
sudo systemctl start ordosd
```

#### Stop service

```
sudo systemctl stop ordosd
```

#### Restart service

```
sudo systemctl restart ordosd
```

#### Check service status

```
sudo systemctl status ordosd
```

#### Check service logs

```
sudo journalctl -u ordosd -f --no-hostname -o cat
```
