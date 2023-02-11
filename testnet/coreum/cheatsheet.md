---
description: >-
  Cheat sheet for set of commands node operators. From key management to chain governance.
---

# Cheat Sheet

<figure><img src="../../.gitbook/assets/coreum.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** coreum-testnet-1 | **Version:** v0.1.1

## Key management

#### Add new key

```
cored keys add wallet
```

#### Recover existing key

```
cored keys add wallet --recover
```

#### List all keys

```
cored keys list
```

#### Delete key

```
cored keys delete wallet
```

#### Export key to the file

```
cored keys export wallet
```

#### Import key from the file

```
cored keys import wallet wallet.backup
```

#### Query wallet balance

```
cored q bank balances $(cored keys show wallet -a)
```

## Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```
cored tx staking create-validator \
--amount=20000000000utestcore \
--pubkey=$(cored tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=coreum-testnet-1 \
--commission-rate=0.01 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=20000000000 \
--from=wallet \
--fees=100000utestcore
```

#### Edit existing validator

```
cored tx staking edit-validator \
--new-moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=coreum-testnet-1 \
--commission-rate=0.05 \
--from=wallet \
--fees=100000utestcore
```

#### Unjail validator

```
cored tx slashing unjail --broadcast-mode=block --from wallet --chain-id coreum-testnet-1 --fees 100000utestcore
```

#### Jail reason

```
cored query slashing signing-info $(cored tendermint show-validator)
```

#### List all active validators

```
cored q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```
cored q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```
cored q staking validator $(cored keys show wallet --bech val -a)
```

## Token management

#### Withdraw rewards from all validators

```
cored tx distribution withdraw-all-rewards --from wallet --chain-id coreum-testnet-1 --fees 100000utestcore
```

#### Withdraw commission and rewards from your validator

```
cored tx distribution withdraw-rewards $(cored keys show wallet --bech val -a) --commission --from wallet --chain-id coreum-testnet-1 --fees 100000utestcore
```

#### Delegate tokens to yourself

```
cored tx staking delegate $(cored keys show wallet --bech val -a) 1000000utestcore --from wallet --chain-id coreum-testnet-1 --fees 100000utestcore
```

#### Delegate tokens to validator

```
cored tx staking delegate <TO_VALOPER_ADDRESS> 1000000utestcore --from wallet --chain-id coreum-testnet-1 --fees 100000utestcore
```

#### Redelegate tokens to another validator

```
cored tx staking redelegate $(cored keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000utestcore --from wallet --chain-id coreum-testnet-1 --fees 100000utestcore
```

#### Unbond tokens from your validator

```
cored tx staking unbond $(cored keys show wallet --bech val -a) 1000000utestcore --from wallet --chain-id coreum-testnet-1 --fees 100000utestcore
```

#### Send tokens to the wallet

```
cored tx bank send wallet <TO_WALLET_ADDRESS> 1000000utestcore --from wallet --chain-id coreum-testnet-1 --fees 100000utestcore
```

## Governance

#### List all proposals

```
cored query gov proposals
```

#### View proposal by id

```
cored query gov proposal 1
```

#### Vote 'Yes'

```
cored tx gov vote 1 yes --from wallet --chain-id coreum-testnet-1 --fees 100000utestcore
```

#### Vote 'No'

```
cored tx gov vote 1 no --from wallet --chain-id coreum-testnet-1 --fees 100000utestcore
```

#### Vote 'Abstain'

```
cored tx gov vote 1 abstain --from wallet --chain-id coreum-testnet-1 --fees 100000utestcore
```

#### Vote 'NoWithVeto'

```
cored tx gov vote 1 nowithveto --from wallet --chain-id coreum-testnet-1 --fees 100000utestcore
```

## Utility

#### Update ports

```
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.core/coreum-testnet-1/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.core/coreum-testnet-1/config/config.toml
```

#### Update Indexer

##### Disable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.core/coreum-testnet-1/config/config.toml
```

##### Enable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.core/coreum-testnet-1/config/config.toml
```

#### Update pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.core/coreum-testnet-1/config/config.toml
```

## Maintenance

#### Get validator info

```
cored status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```
cored status 2>&1 | jq .SyncInfo
```

#### Get node peer

```
echo $(cored tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.core/coreum-testnet-1/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```
[[ $(cored q staking validator $(cored keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(cored status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```
curl -sS http://localhost:21657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0utestcore\"/" $HOME/.core/coreum-testnet-1/config/config.toml
```

#### Enable prometheus

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.core/coreum-testnet-1/config/config.toml
```

#### Reset chain data

```
cored tendermint unsafe-reset-all --keep-addr-book
```

#### Remove node

{% hint style='danger' %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv_validator_key.json**!
{% endhint %}

```
cd $HOME
sudo systemctl stop cored
sudo systemctl disable cored
sudo rm /etc/systemd/system/cored.service
sudo systemctl daemon-reload
rm -f $(which cored)
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
sudo systemctl enable cored
```

#### Disable service

```
sudo systemctl disable cored
```

#### Start service

```
sudo systemctl start cored
```

#### Stop service

```
sudo systemctl stop cored
```

#### Restart service

```
sudo systemctl restart cored
```

#### Check service status

```
sudo systemctl status cored
```

#### Check service logs

```
sudo journalctl -u cored -f --no-hostname -o cat
```
