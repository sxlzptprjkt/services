---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../.gitbook/assets/babylon.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** bbn-test1 | **Version:** v0.5.0

## Set up your babylon Validator
### (automatic)
The auto installation include snapshots or statesync any provider.

You can setup your babylon validator in few minutes by using automated script below. It will prompt you to input your validator node name!

{% hint style='danger' %}
For auto installation requirements Ubuntu 20.04/22.04 LTS and root access. The rest of the **RHEL family** doesn't support it
{% endhint %}

```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/babylon/bbn-test1.sh > bbn-test1.sh && chmod +x bbn-test1.sh && ./bbn-test1.sh
```
This version include binary cosmovisor on babylon
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/babylon/bbn-test1-cosmovisor.sh > bbn-test1-cosmovisor.sh && chmod +x bbn-test1-cosmovisor.sh && ./bbn-test1-cosmovisor.sh
```
This version include statesync on babylon
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/babylon/bbn-test1-statesync.sh > bbn-test1-statesync.sh && chmod +x bbn-test1-statesync.sh && ./bbn-test1-statesync.sh
```
## Post installation

When installation is finished please load variables into system
```
source $HOME/.bash_profile
```
