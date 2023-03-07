---
description: Prepare for and the upcomming chain upgrade using Cosmovisor.
---

# Upgrade

<figure><img src="../../.gitbook/assets/planq.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** planq_7070-2 | **Version:** v1.0.5

{% hint style="info" %}
Since we are using Cosmovisor, it makes it very easy to prepare for upcomming upgrade. You just have to build new binaries and move it into cosmovisor upgrades directory.
{% endhint %}

| Executable | Node Version |
| ----| ------------ |
| **planqd**|v1.0.4 > v1.0.5|

{% hint style="info" %}
if you get an error looking at logs on nodes try updating available new version.
{% endhint %}

## Make a stop node
```bash
sudo systemctl stop planqd
```

#### Remove a executable if you use cosmovisor installer
```bash
sudo rm -f $HOME/.planqd/cosmovisor/genesis/bin/planqd
```

#### Remove a executable if you use basic/statesync installer
```bash
sudo rm -f /usr/bin/planqd
```

#### Before upgrading you should check if your (GO)lang avaliable if not follow the command below

```bash
ver="1.19.5"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
source ~/.bash_profile
go version
```

## Updating binary and start service

```bash
cd $HOME
rm -rf planq
git clone https://github.com/planq-network/planq
cd planq
git checkout v1.0.5
make install
```

#### Move binary to executable if you use cosmovisor

```bash
sudo mv $HOME/go/bin/planqd $HOME/.planqd/cosmovisor/genesis/bin/
```

#### Move binary to executable if you use basic/statesync

```bash
sudo mv $HOME/go/bin/planqd /usr/bin/
```

#### Make a running node

```bash
sudo systemctl start planqd
```

## Checking logs
```bash
sudo journalctl -fu planqd -o cat
```
