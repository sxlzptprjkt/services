---
description: >-
  Cheat sheet for set of commands node operators. From key management to chain governance.
---

# Cheat Sheet

<figure><img src="../../.gitbook/assets/ojo.png" alt=""><figcaption></figcaption></figure>

**Network:** Devnet | **Chain ID:** ojo-devnet | **Version:** v0.1.2

## Key management

#### Add new key

```
ojod keys add wallet
```

#### Recover existing key

```
ojod keys add wallet --recover
```

#### List all keys

```
ojod keys list
```

#### Delete key

```
ojod keys delete wallet
```

#### Export key to the file

```
ojod keys export wallet
```

#### Import key from the file

```
ojod keys import wallet wallet.backup
```

#### Query wallet balance

```
ojod q bank balances $(ojod keys show wallet -a)
```

## Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```
ojod tx staking create-validator \
--amount=10000000000uojo \
--pubkey=$(ojod tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=ojo-devnet \
--commission-rate=0.10 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet
```

#### Edit existing validator

```
ojod tx staking edit-validator \
--new-moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=ojo-devnet \
--commission-rate=0.05 \
--from=wallet
```

#### Unjail validator

```
ojod tx slashing unjail --broadcast-mode=block --from wallet --chain-id ojo-devnet 
```

#### Jail reason

```
ojod query slashing signing-info $(ojod tendermint show-validator)
```

#### List all active validators

```
ojod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```
ojod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```
ojod q staking validator $(ojod keys show wallet --bech val -a)
```

## Token management

#### Withdraw rewards from all validators

```
ojod tx distribution withdraw-all-rewards --from wallet --chain-id ojo-devnet 
```

#### Withdraw commission and rewards from your validator

```
ojod tx distribution withdraw-rewards $(ojod keys show wallet --bech val -a) --commission --from wallet --chain-id ojo-devnet 
```

#### Delegate tokens to yourself

```
ojod tx staking delegate $(ojod keys show wallet --bech val -a) 1000000uojo --from wallet --chain-id ojo-devnet 
```

#### Delegate tokens to validator

```
ojod tx staking delegate <TO_VALOPER_ADDRESS> 1000000uojo --from wallet --chain-id ojo-devnet 
```

#### Redelegate tokens to another validator

```
ojod tx staking redelegate $(ojod keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uojo --from wallet --chain-id ojo-devnet 
```

#### Unbond tokens from your validator

```
ojod tx staking unbond $(ojod keys show wallet --bech val -a) 1000000uojo --from wallet --chain-id ojo-devnet 
```

#### Send tokens to the wallet

```
ojod tx bank send wallet <TO_WALLET_ADDRESS> 1000000uojo --from wallet --chain-id ojo-devnet 
```

## Governance

#### List all proposals

```
ojod query gov proposals
```

#### View proposal by id

```
ojod query gov proposal 1
```

#### Vote 'Yes'

```
ojod tx gov vote 1 yes --from wallet --chain-id ojo-devnet 
```

#### Vote 'No'

```
ojod tx gov vote 1 no --from wallet --chain-id ojo-devnet 
```

#### Vote 'Abstain'

```
ojod tx gov vote 1 abstain --from wallet --chain-id ojo-devnet 
```

#### Vote 'NoWithVeto'

```
ojod tx gov vote 1 nowithveto --from wallet --chain-id ojo-devnet 
```

## Utility

#### Update ports

```
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.ojo/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.ojo/config/config.toml
```

#### Update Indexer

##### Disable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.ojo/config/config.toml
```

##### Enable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.ojo/config/config.toml
```

#### Update pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.ojo/config/config.toml
```

## Maintenance

#### Get validator info

```
ojod status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```
ojod status 2>&1 | jq .SyncInfo
```

#### Get node peer

```
echo $(ojod tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.ojo/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```
[[ $(ojod q staking validator $(ojod keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(ojod status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```
curl -sS http://localhost:28657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0uojo\"/" $HOME/.ojo/config/config.toml
```

#### Enable prometheus

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.ojo/config/config.toml
```

#### Reset chain data

```
ojod tendermint unsafe-reset-all --keep-addr-book
```

#### Remove node

{% hint style='danger' %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv_validator_key.json**!
{% endhint %}

```
cd $HOME
sudo systemctl stop ojod
sudo systemctl disable ojod
sudo rm /etc/systemd/system/ojod.service
sudo systemctl daemon-reload
rm -f $(which ojod)
rm -rf $HOME/.ojo
sed -i '/OJO_/d' ~/.bash_profile
```

## Service Management

#### Reload service configuration

```
sudo systemctl daemon-reload
```

#### Enable service

```
sudo systemctl enable ojod
```

#### Disable service

```
sudo systemctl disable ojod
```

#### Start service

```
sudo systemctl start ojod
```

#### Stop service

```
sudo systemctl stop ojod
```

#### Restart service

```
sudo systemctl restart ojod
```

#### Check service status

```
sudo systemctl status ojod
```

#### Check service logs

```
sudo journalctl -fu ojod -o cat
```
