---
description: >-
  Cheat sheet for set of commands node operators. From key management to chain governance.
---

# Cheat Sheet

<figure><img src="../../.gitbook/assets/andromeda.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** galileo-3 | **Version:** galileo-3-v1.1.0-beta1

## Key management

#### Add new key

```
andromedad keys add wallet
```

#### Recover existing key

```
andromedad keys add wallet --recover
```

#### List all keys

```
andromedad keys list
```

#### Delete key

```
andromedad keys delete wallet
```

#### Export key to the file

```
andromedad keys export wallet
```

#### Import key from the file

```
andromedad keys import wallet wallet.backup
```

#### Query wallet balance

```
andromedad q bank balances $(andromedad keys show wallet -a)
```

## Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```
andromedad tx staking create-validator \
--amount=2000000uandr \
--pubkey=$(andromedad tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=galileo-3 \
--commission-rate=0.10 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet \
--gas-adjustment=1.15 \
--gas=auto \
--gas-prices=0.0000uandr
```

#### Edit existing validator

```
andromedad tx staking edit-validator \
--new-moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=galileo-3 \
--commission-rate=0.05 \
--from=wallet \
--gas-adjustment=1.15 \
--gas=auto \
--gas-prices=0.0000uandr
```

#### Unjail validator

```
andromedad tx slashing unjail --broadcast-mode=block --from wallet --chain-id galileo-3 --gas-adjustment=1.15 --gas=auto --gas-prices=0.0000uandr
```

#### Jail reason

```
andromedad query slashing signing-info $(andromedad tendermint show-validator)
```

#### List all active validators

```
andromedad q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```
andromedad q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```
andromedad q staking validator $(andromedad keys show wallet --bech val -a)
```

## Token management

#### Withdraw rewards from all validators

```
andromedad tx distribution withdraw-all-rewards --from wallet --chain-id galileo-3 --gas-adjustment=1.15 --gas=auto --gas-prices=0.0000uandr
```

#### Withdraw commission and rewards from your validator

```
andromedad tx distribution withdraw-rewards $(andromedad keys show wallet --bech val -a) --commission --from wallet --chain-id galileo-3 --gas-adjustment=1.15 --gas=auto --gas-prices=0.0000uandr
```

#### Delegate tokens to yourself

```
andromedad tx staking delegate $(andromedad keys show wallet --bech val -a) 1000000uandr --from wallet --chain-id galileo-3 --gas-adjustment=1.15 --gas=auto --gas-prices=0.0000uandr
```

#### Delegate tokens to validator

```
andromedad tx staking delegate <TO_VALOPER_ADDRESS> 1000000uandr --from wallet --chain-id galileo-3 --gas-adjustment=1.15 --gas=auto --gas-prices=0.0000uandr
```

#### Redelegate tokens to another validator

```
andromedad tx staking redelegate $(andromedad keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uandr --from wallet --chain-id galileo-3 --gas-adjustment=1.15 --gas=auto --gas-prices=0.0000uandr
```

#### Unbond tokens from your validator

```
andromedad tx staking unbond $(andromedad keys show wallet --bech val -a) 1000000uandr --from wallet --chain-id galileo-3 --gas-adjustment=1.15 --gas=auto --gas-prices=0.0000uandr
```

#### Send tokens to the wallet

```
andromedad tx bank send wallet <TO_WALLET_ADDRESS> 1000000uandr --from wallet --chain-id galileo-3 --gas-adjustment=1.15 --gas=auto --gas-prices=0.0000uandr
```

## Governance

#### List all proposals

```
andromedad query gov proposals
```

#### View proposal by id

```
andromedad query gov proposal 1
```

#### Vote 'Yes'

```
andromedad tx gov vote 1 yes --from wallet --chain-id galileo-3 --gas-adjustment=1.15 --gas=auto --gas-prices=0.0000uandr
```

#### Vote 'No'

```
andromedad tx gov vote 1 no --from wallet --chain-id galileo-3 --gas-adjustment=1.15 --gas=auto --gas-prices=0.0000uandr
```

#### Vote 'Abstain'

```
andromedad tx gov vote 1 abstain --from wallet --chain-id galileo-3 --gas-adjustment=1.15 --gas=auto --gas-prices=0.0000uandr
```

#### Vote 'NoWithVeto'

```
andromedad tx gov vote 1 nowithveto --from wallet --chain-id galileo-3 --gas-adjustment=1.15 --gas=auto --gas-prices=0.0000uandr
```

## Utility

#### Update ports

```
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.andromedad/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.andromedad/config/config.toml
```

#### Update Indexer

##### Disable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.andromedad/config/config.toml
```

##### Enable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.andromedad/config/config.toml
```

#### Update pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.andromedad/config/config.toml
```

## Maintenance

#### Get validator info

```
andromedad status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```
andromedad status 2>&1 | jq .SyncInfo
```

#### Get node peer

```
echo $(andromedad tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.andromedad/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```
[[ $(andromedad q staking validator $(andromedad keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(andromedad status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```
curl -sS http://localhost:22657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0uandr\"/" $HOME/.andromedad/config/config.toml
```

#### Enable prometheus

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.andromedad/config/config.toml
```

#### Reset chain data

```
andromedad tendermint unsafe-reset-all --keep-addr-book
```

#### Remove node

{% hint style='danger' %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv_validator_key.json**!
{% endhint %}

```
cd $HOME
sudo systemctl stop andromedad
sudo systemctl disable andromedad
sudo rm /etc/systemd/system/andromedad.service
sudo systemctl daemon-reload
rm -f $(which andromedad)
rm -rf $HOME/.core
sed -i '/ANDR_/d' ~/.bash_profile
```

## Service Management

#### Reload service configuration

```
sudo systemctl daemon-reload
```

#### Enable service

```
sudo systemctl enable andromedad
```

#### Disable service

```
sudo systemctl disable andromedad
```

#### Start service

```
sudo systemctl start andromedad
```

#### Stop service

```
sudo systemctl stop andromedad
```

#### Restart service

```
sudo systemctl restart andromedad
```

#### Check service status

```
sudo systemctl status andromedad
```

#### Check service logs

```
sudo journalctl -u andromedad -f --no-hostname -o cat
```
