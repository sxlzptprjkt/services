---
description: >-
  Cheat sheet for set of commands node operators. From key management to chain governance.
---

# Cheat Sheet

<figure><img src="../../.gitbook/assets/sao.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** sao-testnet0 | **Version:** testnet0

## Key management

#### Add new key

```
saod keys add wallet
```

#### Recover existing key

```
saod keys add wallet --recover
```

#### List all keys

```
saod keys list
```

#### Delete key

```
saod keys delete wallet
```

#### Export key to the file

```
saod keys export wallet
```

#### Import key from the file

```
saod keys import wallet wallet.backup
```

#### Query wallet balance

```
saod q bank balances $(saod keys show wallet -a)
```

## Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```
saod tx staking create-validator \
--amount=1000000sao \
--pubkey=$(saod tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=sao-testnet0 \
--commission-rate=0.10 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet \
--fees=550sao
```

#### Edit existing validator

```
saod tx staking edit-validator \
--new-moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=sao-testnet0 \
--commission-rate=0.05 \
--from=wallet \
--fees=550sao
```

#### Unjail validator

```
saod tx slashing unjail --broadcast-mode=block --from wallet --chain-id sao-testnet0 --fees=550sao
```

#### Jail reason

```
saod query slashing signing-info $(saod tendermint show-validator)
```

#### List all active validators

```
saod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```
saod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```
saod q staking validator $(saod keys show wallet --bech val -a)
```

## Token management

#### Withdraw rewards from all validators

```
saod tx distribution withdraw-all-rewards --from wallet --chain-id sao-testnet0 --fees=550sao
```

#### Withdraw commission and rewards from your validator

```
saod tx distribution withdraw-rewards $(saod keys show wallet --bech val -a) --commission --from wallet --chain-id sao-testnet0 --fees=550sao
```

#### Delegate tokens to yourself

```
saod tx staking delegate $(saod keys show wallet --bech val -a) 1000000sao --from wallet --chain-id sao-testnet0 --fees=550sao
```

#### Delegate tokens to validator

```
saod tx staking delegate <TO_VALOPER_ADDRESS> 1000000sao --from wallet --chain-id sao-testnet0 --fees=550sao
```

#### Redelegate tokens to another validator

```
saod tx staking redelegate $(saod keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000sao --from wallet --chain-id sao-testnet0 --fees=550sao
```

#### Unbond tokens from your validator

```
saod tx staking unbond $(saod keys show wallet --bech val -a) 1000000sao --from wallet --chain-id sao-testnet0 --fees=550sao
```

#### Send tokens to the wallet

```
saod tx bank send wallet <TO_WALLET_ADDRESS> 1000000sao --from wallet --chain-id sao-testnet0 --fees=550sao
```

## Governance

#### List all proposals

```
saod query gov proposals
```

#### View proposal by id

```
saod query gov proposal 1
```

#### Vote 'Yes'

```
saod tx gov vote 1 yes --from wallet --chain-id sao-testnet0 --fees=550sao
```

#### Vote 'No'

```
saod tx gov vote 1 no --from wallet --chain-id sao-testnet0 --fees=550sao
```

#### Vote 'Abstain'

```
saod tx gov vote 1 abstain --from wallet --chain-id sao-testnet0 --fees=550sao
```

#### Vote 'NoWithVeto'

```
saod tx gov vote 1 nowithveto --from wallet --chain-id sao-testnet0 --fees=550sao
```

## Utility

#### Update ports

```
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.sao/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.sao/config/config.toml
```

#### Update Indexer

##### Disable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.sao/config/config.toml
```

##### Enable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.sao/config/config.toml
```

#### Update pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.sao/config/config.toml
```

## Maintenance

#### Get validator info

```
saod status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```
saod status 2>&1 | jq .SyncInfo
```

#### Get node peer

```
echo $(saod tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.sao/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```
[[ $(saod q staking validator $(saod keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(saod status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```
curl -sS http://localhost:27657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0sao\"/" $HOME/.sao/config/config.toml
```

#### Enable prometheus

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.sao/config/config.toml
```

#### Reset chain data

```
saod tendermint unsafe-reset-all --keep-addr-book
```

#### Remove node

{% hint style='danger' %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv_validator_key.json**!
{% endhint %}

```
cd $HOME
sudo systemctl stop saod
sudo systemctl disable saod
sudo rm /etc/systemd/system/saod.service
sudo systemctl daemon-reload
rm -f $(which saod)
rm -rf $HOME/.sao
sed -i '/SAO_/d' ~/.bash_profile
```

## Service Management

#### Reload service configuration

```
sudo systemctl daemon-reload
```

#### Enable service

```
sudo systemctl enable saod
```

#### Disable service

```
sudo systemctl disable saod
```

#### Start service

```
sudo systemctl start saod
```

#### Stop service

```
sudo systemctl stop saod
```

#### Restart service

```
sudo systemctl restart saod
```

#### Check service status

```
sudo systemctl status saod
```

#### Check service logs

```
sudo journalctl -fu saod -o cat
```
