---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../.gitbook/assets/osmosis.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet **| Chain ID:** osmosis-1 | **Version:** v1.4.0

## Set up your Osmosis Validator
### (automatic)
The auto installation include snapshots or statesync any provider.

You can setup your osmosis validator in few minutes by using automated script below. It will prompt you to input your validator node name!

{% hint style='danger' %}
For auto installation requirements Ubuntu 20.04/22.04 LTS and root access. The rest of the **RHEL family** doesn't support it
{% endhint %}

```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/osmosis/osmosis.sh > osmosis.sh && chmod +x osmosis.sh && ./osmosis.sh
```
This version include binary cosmovisor on osmosis network
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/osmosis/cosmovisor.sh > cosmovisor.sh && chmod +x cosmovisor.sh && ./cosmovisor.sh
```
## Post installation

When installation is finished please load variables into system
```
source $HOME/.bash_profile
```
