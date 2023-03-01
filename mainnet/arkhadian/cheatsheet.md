---
description: >-
  Cheat sheet for set of commands node operators. From key management to chain governance.
---

# Cheat Sheet

<figure><img src="../../.gitbook/assets/arkh.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** arkh | **Version:** v2.0.0

## Key management

#### Add new key

```
arkhd keys add wallet
```

#### Recover existing key

```
arkhd keys add wallet --recover
```

#### List all keys

```
arkhd keys list
```

#### Delete key

```
arkhd keys delete wallet
```

#### Export key to the file

```
arkhd keys export wallet
```

#### Import key from the file

```
arkhd keys import wallet wallet.backup
```

#### Query wallet balance

```
arkhd q bank balances $(arkhd keys show wallet -a)
```

## Validator management

{% hint style="info" %}
Please make sure you have adjusted **moniker**, **identity**, **details** and **website** to match your values.
{% endhint %}

#### Create new validator

```
arkhd tx staking create-validator \
--amount=1000000arkh \
--pubkey=$(arkhd tendermint show-validator) \
--moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL" \
--chain-id=arkh \
--commission-rate=0.01 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=wallet \
--fees=5000arkh
```

#### Edit existing validator

```
arkhd tx staking edit-validator \
--new-moniker="YOUR_MONIKER_NAME" \
--identity="YOUR_KEYBASE_ID" \
--details="YOUR_DETAILS" \
--website="YOUR_WEBSITE_URL"
--chain-id=arkh \
--commission-rate=0.05 \
--from=wallet \
--fees=5000arkh
```

#### Unjail validator

```
arkhd tx slashing unjail --broadcast-mode=block --from wallet --chain-id arkh --fees=5000arkh
```

#### Jail reason

```
arkhd query slashing signing-info $(arkhd tendermint show-validator)
```

#### List all active validators

```
arkhd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List all inactive validators

```
arkhd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### View validator details

```
arkhd q staking validator $(arkhd keys show wallet --bech val -a)
```

## Token management

#### Withdraw rewards from all validators

```
arkhd tx distribution withdraw-all-rewards --from wallet --chain-id arkh --fees=5000arkh
```

#### Withdraw commission and rewards from your validator

```
arkhd tx distribution withdraw-rewards $(arkhd keys show wallet --bech val -a) --commission --from wallet --chain-id arkh --fees=5000arkh
```

#### Delegate tokens to yourself

```
arkhd tx staking delegate $(arkhd keys show wallet --bech val -a) 1000000arkh --from wallet --chain-id arkh --fees=5000arkh
```

#### Delegate tokens to validator

```
arkhd tx staking delegate <TO_VALOPER_ADDRESS> 1000000arkh --from wallet --chain-id arkh --fees=5000arkh
```

#### Redelegate tokens to another validator

```
arkhd tx staking redelegate $(arkhd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000arkh --from wallet --chain-id arkh --fees=5000arkh
```

#### Unbond tokens from your validator

```
arkhd tx staking unbond $(arkhd keys show wallet --bech val -a) 1000000arkh --from wallet --chain-id arkh --fees=5000arkh
```

#### Send tokens to the wallet

```
arkhd tx bank send wallet <TO_WALLET_ADDRESS> 1000000arkh --from wallet --chain-id arkh --fees=5000arkh
```

## Governance

#### List all proposals

```
arkhd query gov proposals
```

#### View proposal by id

```
arkhd query gov proposal 1
```

#### Vote 'Yes'

```
arkhd tx gov vote 1 yes --from wallet --chain-id arkh --fees=5000arkh
```

#### Vote 'No'

```
arkhd tx gov vote 1 no --from wallet --chain-id arkh --fees=5000arkh
```

#### Vote 'Abstain'

```
arkhd tx gov vote 1 abstain --from wallet --chain-id arkh --fees=5000arkh
```

#### Vote 'NoWithVeto'

```
arkhd tx gov vote 1 nowithveto --from wallet --chain-id arkh --fees=5000arkh
```

## Utility

#### Update ports

```
CUSTOM_PORT=10
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}660\"%" $HOME/.arkh/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}317\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}091\"%" $HOME/.arkh/config/config.toml
```

#### Update Indexer

##### Disable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.arkh/config/config.toml
```

##### Enable indexer

```
sed -i -e 's|^indexer *=.*|indexer = "kv"|' $HOME/.arkh/config/config.toml
```

#### Update pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.arkh/config/config.toml
```

## Maintenance

#### Get validator info

```
arkhd status 2>&1 | jq .ValidatorInfo
```

#### Get sync info

```
arkhd status 2>&1 | jq .SyncInfo
```

#### Get node peer

```
echo $(arkhd tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.arkh/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Check if validator key is correct

```
[[ $(arkhd q staking validator $(arkhd keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(arkhd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

#### Get live peers

```
curl -sS http://localhost:25657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Set minimum gas price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025arkh\"/" $HOME/.arkh/config/config.toml
```

#### Enable prometheus

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.arkh/config/config.toml
```

#### Reset chain data

```
arkhd unsafe-reset-all --home $HOME/.arkh
```

#### Remove node

{% hint style='danger' %}
Please, before proceeding with the next step! All chain data will be lost! Make sure you have backed up your **priv_validator_key.json**!
{% endhint %}

```
cd $HOME
sudo systemctl stop arkhd
sudo systemctl disable arkhd
sudo rm -f /etc/systemd/system/arkhd.service
sudo systemctl daemon-reload
rm -f $(which arkhd)
rm -rf $HOME/.arkh
sed -i '/ARKH_/d' ~/.bash_profile
```

## Service Management

#### Reload service configuration

```
sudo systemctl daemon-reload
```

#### Enable service

```
sudo systemctl enable arkhd
```

#### Disable service

```
sudo systemctl disable arkhd
```

#### Start service

```
sudo systemctl start arkhd
```

#### Stop service

```
sudo systemctl stop arkhd
```

#### Restart service

```
sudo systemctl restart arkhd
```

#### Check service status

```
sudo systemctl status arkhd
```

#### Check service logs

```
sudo journalctl -u arkhd -f --no-hostname -o cat
```
