---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../.gitbook/assets/andromeda.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** galileo-3 | **Version:** galileo-3-v1.1.0-beta1

## Set up your andromeda Validator
### (automatic)
The auto installation include snapshots or statesync any provider.

You can setup your andromeda validator in few minutes by using automated script below. It will prompt you to input your validator node name!

{% hint style='danger' %}
For auto installation requirements Ubuntu 20.04/22.04 LTS and root access. The rest of the **RHEL family** doesn't support it
{% endhint %}

```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/andromeda/galileo-3.sh > galileo-3.sh && chmod +x galileo-3.sh && ./galileo-3.sh
```
This version include binary cosmovisor on andromeda
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/andromeda/galileo-3-cosmovisor.sh > galileo-3-cosmovisor.sh && chmod +x galileo-3-cosmovisor.sh && ./galileo-3-cosmovisor.sh
```
This version include statesync on andromeda
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/andromeda/galileo-3-statesync.sh > galileo-3-statesync.sh && chmod +x galileo-3-statesync.sh && ./galileo-3-statesync.sh
```
## Post installation

When installation is finished please load variables into system
```
source $HOME/.bash_profile
```
