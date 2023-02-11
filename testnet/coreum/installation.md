---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../.gitbook/assets/coreum.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** coreum-testnet-1 | **Version:** v0.1.1

## Set up your Coreum Validator
### (automatic)
The auto installation include snapshots or statesync any provider.

You can setup your planq validator in few minutes by using automated script below. It will prompt you to input your validator node name!

{% hint style='danger' %}
For auto installation requirements Ubuntu 20.04/22.04 LTS and root access. The rest of the **RHEL family** doesn't support it
{% endhint %}

```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/coreum/testnet-1.sh > testnet-1.sh && chmod +x testnet-1.sh && ./testnet-1.sh
```
This version include binary cosmovisor on coreum
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/coreum/testnet-1-cosmovisor.sh > testnet-1-cosmovisor.sh && chmod +x testnet-1-cosmovisor.sh && ./testnet-1-cosmovisor.sh
```
This version include statesync on coreum
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/coreum/testnet-1-statesync.sh > testnet-1-statesync.sh && chmod +x testnet-1-statesync.sh && ./testnet-1-statesync.sh
```
## Post installation

When installation is finished please load variables into system
```
source $HOME/.bash_profile
```
