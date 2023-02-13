---
description: >-
  Cheat sheet for set of commands node operators. From key management to chain governance.
---

# Cheat Sheet

<figure><img src="../../../.gitbook/assets/corrino.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** corrino-1 | **Version:** v0.0.1-goa

## Key management

#### Add new key

```
corrinod keys add wallet
```

#### Recover existing key

```
corrinod keys add wallet --recover
```

#### List all keys

```
corrinod keys list
```

#### Delete key

```
corrinod keys delete wallet
```

#### Export key to the file

```
corrinod keys export wallet
```

#### Import key from the file

```
corrinod keys import wallet wallet.backup
```

#### Query wallet balance

```
corrinod q bank balances $(corrinod keys show wallet -a)
```

## Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```
corrinod tx staking create-validator \
--amount=5000000ucor \
--pubkey=$(corrinod tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=corrino-1 \
--commission-rate=0.01 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1000000 \
--from=wallet \
--fees="1000ucor" \
--gas="1000000" \
--gas-adjustment="1.15"
```

#### Edit existing validator

```
corrinod tx staking edit-validator \
--new-moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=corrino-1 \
--commission-rate=0.05 \
--from=wallet \
--fees="1000ucor" \
--gas="1000000" \
--gas-adjustment="1.15"
```

#### Unjail validator

```
corrinod tx slashing unjail --broadcast-mode=block --from wallet --chain-id corrino-1 --fees="1000ucor" --gas="1000000" --gas-adjustment="1.15"
```

#### Jail reason

```
corrinod query slashing signing-info $(corrinod tendermint show-validator)
```

#### List all active validators

```
corrinod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```
corrinod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```
corrinod q staking validator $(corrinod keys show wallet --bech val -a)
```

## Token management

#### Withdraw rewards from all validators

```
corrinod tx distribution withdraw-all-rewards --from wallet --chain-id corrino-1 --fees="1000ucor" --gas="1000000" --gas-adjustment="1.15"
```

#### Withdraw commission and rewards from your validator

```
corrinod tx distribution withdraw-rewards $(corrinod keys show wallet --bech val -a) --commission --from wallet --chain-id corrino-1 --fees="1000ucor" --gas="1000000" --gas-adjustment="1.15"
```

#### Delegate tokens to yourself

```
corrinod tx staking delegate $(corrinod keys show wallet --bech val -a) 1000000ucor --from wallet --chain-id corrino-1 --fees="1000ucor" --gas="1000000" --gas-adjustment="1.15"
```

#### Delegate tokens to validator

```
corrinod tx staking delegate <TO_VALOPER_ADDRESS> 1000000ucor --from wallet --chain-id corrino-1 --fees="1000ucor" --gas="1000000" --gas-adjustment="1.15"
```

#### Redelegate tokens to another validator

```
corrinod tx staking redelegate $(corrinod keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000ucor --from wallet --chain-id corrino-1 --fees="1000ucor" --gas="1000000" --gas-adjustment="1.15"
```

#### Unbond tokens from your validator

```
corrinod tx staking unbond $(corrinod keys show wallet --bech val -a) 1000000ucor --from wallet --chain-id corrino-1 --fees="1000ucor" --gas="1000000" --gas-adjustment="1.15"
```

#### Send tokens to the wallet

```
corrinod tx bank send wallet <TO_WALLET_ADDRESS> 1000000ucor --from wallet --chain-id corrino-1 --fees="1000ucor" --gas="1000000" --gas-adjustment="1.15"
```

## Governance

#### List all proposals

```
corrinod query gov proposals
```

#### View proposal by id

```
corrinod query gov proposal 1
```

#### Vote 'Yes'

```
corrinod tx gov vote 1 yes --from wallet --chain-id corrino-1 --fees="1000ucor" --gas="1000000" --gas-adjustment="1.15"
```

#### Vote 'No'

```
corrinod tx gov vote 1 no --from wallet --chain-id corrino-1 --fees="1000ucor" --gas="1000000" --gas-adjustment="1.15"
```

#### Vote 'Abstain'

```
corrinod tx gov vote 1 abstain --from wallet --chain-id corrino-1 --fees="1000ucor" --gas="1000000" --gas-adjustment="1.15"
```

#### Vote 'NoWithVeto'

```
corrinod tx gov vote 1 nowithveto --from wallet --chain-id corrino-1 --fees="1000ucor" --gas="1000000" --gas-adjustment="1.15"
```

## Utility

#### Update ports

```
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.corrino/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.corrino/config/config.toml
```

#### Update Indexer

##### Disable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.corrino/config/config.toml
```

##### Enable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.corrino/config/config.toml
```

#### Update pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.corrino/config/config.toml
```

## Maintenance

#### Get validator info

```
corrinod status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```
corrinod status 2>&1 | jq .SyncInfo
```

#### Get node peer

```
echo $(corrinod tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.corrino/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```
[[ $(corrinod q staking validator $(corrinod keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(corrinod status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```
curl -sS http://localhost:03657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0ucor\"/" $HOME/.corrino/config/config.toml
```

#### Enable prometheus

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.corrino/config/config.toml
```

#### Reset chain data

```
corrinod tendermint unsafe-reset-all --home $HOME/.corrino --keep-addr-book
```

#### Remove node

{% hint style='danger' %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv_validator_key.json**!
{% endhint %}

```
cd $HOME
sudo systemctl stop corrinod
sudo systemctl disable corrinod
sudo rm -f /etc/systemd/system/corrinod.service
sudo systemctl daemon-reload
rm -f $(which corrinod)
rm -rf $HOME/.corrino
sed -i '/CORRINO_/d' ~/.bash_profile
```

## Service Management

#### Reload service configuration

```
sudo systemctl daemon-reload
```

#### Enable service

```
sudo systemctl enable corrinod
```

#### Disable service

```
sudo systemctl disable corrinod
```

#### Start service

```
sudo systemctl start corrinod
```

#### Stop service

```
sudo systemctl stop corrinod
```

#### Restart service

```
sudo systemctl restart corrinod
```

#### Check service status

```
sudo systemctl status corrinod
```

#### Check service logs

```
sudo journalctl -u corrinod -f --no-hostname -o cat
```
