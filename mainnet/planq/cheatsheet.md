---
description: >-
  Cheat sheet for set of commands node operators. From key management to chain governance.
---

# Cheat Sheet

<figure><img src="../../.gitbook/assets/planq.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** planq_7070-2 | **Version:** v1.0.3

## Key management

#### Add new key

```
planqd keys add wallet
```

#### Recover existing key

```
planqd keys add wallet --recover
```

#### List all keys

```
planqd keys list
```

#### Delete key

```
planqd keys delete wallet
```

#### Export key to the file

```
planqd keys export wallet
```

#### Import key from the file

```
planqd keys import wallet wallet.backup
```

#### Query wallet balance

```
planqd q bank balances $(planqd keys show wallet -a)
```

## Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```
planqd tx staking create-validator \
--amount=1000000aplanq \
--pubkey=$(planqd tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=planq_7070-2 \
--commission-rate=0.01 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1000000 \
--from=wallet \
--gas="1000000" \
--gas-adjustment="1.15"
```

#### Edit existing validator

```
planqd tx staking edit-validator \
--new-moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=planq_7070-2 \
--commission-rate=0.05 \
--from=wallet \
--gas="1000000" \
--gas-adjustment="1.15"
```

#### Unjail validator

```
planqd tx slashing unjail --broadcast-mode=block --from wallet --chain-id planq_7070-2 --gas="1000000" --gas-adjustment="1.15"
```

#### Jail reason

```
planqd query slashing signing-info $(planqd tendermint show-validator)
```

#### List all active validators

```
planqd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```
planqd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```
planqd q staking validator $(planqd keys show wallet --bech val -a)
```

## Token management

#### Withdraw rewards from all validators

```
planqd tx distribution withdraw-all-rewards --from wallet --chain-id planq_7070-2 --gas="1000000" --gas-adjustment="1.15"
```

#### Withdraw commission and rewards from your validator

```
planqd tx distribution withdraw-rewards $(planqd keys show wallet --bech val -a) --commission --from wallet --chain-id planq_7070-2 --gas="1000000" --gas-adjustment="1.15"
```

#### Delegate tokens to yourself

```
planqd tx staking delegate $(planqd keys show wallet --bech val -a) 1000000aplanq --from wallet --chain-id planq_7070-2 --gas="1000000" --gas-adjustment="1.15"
```

#### Delegate tokens to validator

```
planqd tx staking delegate <TO_VALOPER_ADDRESS> 1000000aplanq --from wallet --chain-id planq_7070-2 --gas="1000000" --gas-adjustment="1.15"
```

#### Redelegate tokens to another validator

```
planqd tx staking redelegate $(planqd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000aplanq --from wallet --chain-id planq_7070-2 --gas="1000000" --gas-adjustment="1.15"
```

#### Unbond tokens from your validator

```
planqd tx staking unbond $(planqd keys show wallet --bech val -a) 1000000aplanq --from wallet --chain-id planq_7070-2 --gas="1000000" --gas-adjustment="1.15"
```

#### Send tokens to the wallet

```
planqd tx bank send wallet <TO_WALLET_ADDRESS> 1000000aplanq --from wallet --chain-id planq_7070-2 --gas="1000000" --gas-adjustment="1.15"
```

## Governance

#### List all proposals

```
planqd query gov proposals
```

#### View proposal by id

```
planqd query gov proposal 1
```

#### Vote 'Yes'

```
planqd tx gov vote 1 yes --from wallet --chain-id planq_7070-2 --gas="1000000" --gas-adjustment="1.15"
```

#### Vote 'No'

```
planqd tx gov vote 1 no --from wallet --chain-id planq_7070-2 --gas="1000000" --gas-adjustment="1.15"
```

#### Vote 'Abstain'

```
planqd tx gov vote 1 abstain --from wallet --chain-id planq_7070-2 --gas="1000000" --gas-adjustment="1.15"
```

#### Vote 'NoWithVeto'

```
planqd tx gov vote 1 nowithveto --from wallet --chain-id planq_7070-2 --gas="1000000" --gas-adjustment="1.15"
```

## Utility

#### Update ports

```
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.planqd/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.planqd/config/config.toml
```

#### Update Indexer

##### Disable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.planqd/config/config.toml
```

##### Enable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.planqd/config/config.toml
```

#### Update pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.planqd/config/config.toml
```

## Maintenance

#### Get validator info

```
planqd status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```
planqd status 2>&1 | jq .SyncInfo
```

#### Get node peer

```
echo $(planqd tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.planqd/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```
[[ $(planqd q staking validator $(planqd keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(planqd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```
curl -sS http://localhost:18657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0aplanq\"/" $HOME/.planqd/config/config.toml
```

#### Enable prometheus

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.planqd/config/config.toml
```

#### Reset chain data

```
planqd tendermint unsafe-reset-all --home --keep-addr-book
```

#### Remove node

{% hint style='danger' %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv_validator_key.json**!
{% endhint %}

```
cd $HOME
sudo systemctl stop planqd
sudo systemctl disable planqd
sudo rm /etc/systemd/system/planqd.service
sudo systemctl daemon-reload
rm -f $(which planqd)
rm -rf $HOME/.core
sed -i '/TESTCORE_/d' ~/.bash_profile
```

## Service Management

#### Reload service configuration

```
sudo systemctl daemon-reload
```

#### Enable service

```
sudo systemctl enable planqd
```

#### Disable service

```
sudo systemctl disable planqd
```

#### Start service

```
sudo systemctl start planqd
```

#### Stop service

```
sudo systemctl stop planqd
```

#### Restart service

```
sudo systemctl restart planqd
```

#### Check service status

```
sudo systemctl status planqd
```

#### Check service logs

```
sudo journalctl -u planqd -f --no-hostname -o cat
```
