---
description: >-
  Cheat sheet for set of commands node operators. From key management to chain governance.
---

# Cheat Sheet

<figure><img src="../../.gitbook/assets/8ball.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** eightball-1 | **Version:** v0.34.24

## Key management

#### Add new key

```
8ball keys add wallet
```

#### Recover existing key

```
8ball keys add wallet --recover
```

#### List all keys

```
8ball keys list
```

#### Delete key

```
8ball keys delete wallet
```

#### Export key to the file

```
8ball keys export wallet
```

#### Import key from the file

```
8ball keys import wallet wallet.backup
```

#### Query wallet balance

```
8ball q bank balances $(8ball keys show wallet -a)
```

## Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```
8ball tx staking create-validator \
--amount=100000000uebl \
--pubkey=$(8ball tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=eightball-1 \
--commission-rate=0.10 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet \
--fees=5000uebl
```

#### Edit existing validator

```
8ball tx staking edit-validator \
--new-moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=eightball-1 \
--commission-rate=0.05 \
--from=wallet \
--fees=5000uebl
```

#### Unjail validator

```
8ball tx slashing unjail --broadcast-mode=block --from wallet --chain-id eightball-1 --fees=5000uebl
```

#### Jail reason

```
8ball query slashing signing-info $(8ball tendermint show-validator)
```

#### List all active validators

```
8ball q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```
8ball q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```
8ball q staking validator $(8ball keys show wallet --bech val -a)
```

## Token management

#### Withdraw rewards from all validators

```
8ball tx distribution withdraw-all-rewards --from wallet --chain-id eightball-1 --fees=5000uebl
```

#### Withdraw commission and rewards from your validator

```
8ball tx distribution withdraw-rewards $(8ball keys show wallet --bech val -a) --commission --from wallet --chain-id eightball-1 --fees=5000uebl
```

#### Delegate tokens to yourself

```
8ball tx staking delegate $(8ball keys show wallet --bech val -a) 1000000uebl --from wallet --chain-id eightball-1 --fees=5000uebl
```

#### Delegate tokens to validator

```
8ball tx staking delegate <TO_VALOPER_ADDRESS> 1000000uebl --from wallet --chain-id eightball-1 --fees=5000uebl
```

#### Redelegate tokens to another validator

```
8ball tx staking redelegate $(8ball keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uebl --from wallet --chain-id eightball-1 --fees=5000uebl
```

#### Unbond tokens from your validator

```
8ball tx staking unbond $(8ball keys show wallet --bech val -a) 1000000uebl --from wallet --chain-id eightball-1 --fees=5000uebl
```

#### Send tokens to the wallet

```
8ball tx bank send wallet <TO_WALLET_ADDRESS> 1000000uebl --from wallet --chain-id eightball-1 --fees=5000uebl
```

## Governance

#### List all proposals

```
8ball query gov proposals
```

#### View proposal by id

```
8ball query gov proposal 1
```

#### Vote 'Yes'

```
8ball tx gov vote 1 yes --from wallet --chain-id eightball-1 --fees=5000uebl
```

#### Vote 'No'

```
8ball tx gov vote 1 no --from wallet --chain-id eightball-1 --fees=5000uebl
```

#### Vote 'Abstain'

```
8ball tx gov vote 1 abstain --from wallet --chain-id eightball-1 --fees=5000uebl
```

#### Vote 'NoWithVeto'

```
8ball tx gov vote 1 nowithveto --from wallet --chain-id eightball-1 --fees=5000uebl
```

## Utility

#### Update ports

```
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.8ball/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.8ball/config/config.toml
```

#### Update Indexer

##### Disable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.8ball/config/config.toml
```

##### Enable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.8ball/config/config.toml
```

#### Update pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.8ball/config/config.toml
```

## Maintenance

#### Get validator info

```
8ball status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```
8ball status 2>&1 | jq .SyncInfo
```

#### Get node peer

```
echo $(8ball tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.8ball/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```
[[ $(8ball q staking validator $(8ball keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(8ball status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```
curl -sS http://localhost:23657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0uebl\"/" $HOME/.8ball/config/config.toml
```

#### Enable prometheus

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.8ball/config/config.toml
```

#### Reset chain data

```
8ball tendermint unsafe-reset-all --keep-addr-book
```

#### Remove node

{% hint style='danger' %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv_validator_key.json**!
{% endhint %}

```
cd $HOME
sudo systemctl stop 8ball
sudo systemctl disable 8ball
sudo rm /etc/systemd/system/8ball.service
sudo systemctl daemon-reload
rm -f $(which 8ball)
rm -rf $HOME/.8ball
sed -i '/EBL_/d' ~/.bash_profile
```

## Service Management

#### Reload service configuration

```
sudo systemctl daemon-reload
```

#### Enable service

```
sudo systemctl enable 8ball
```

#### Disable service

```
sudo systemctl disable 8ball
```

#### Start service

```
sudo systemctl start 8ball
```

#### Stop service

```
sudo systemctl stop 8ball
```

#### Restart service

```
sudo systemctl restart 8ball
```

#### Check service status

```
sudo systemctl status 8ball
```

#### Check service logs

```
sudo journalctl -u 8ball -f --no-hostname -o cat
```
