---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../.gitbook/assets/ojo.png" alt=""><figcaption></figcaption></figure>

**Network:** Devnet | **Chain ID:** ojo-devnet | **Version:** v0.1.2

## Set up your ojo Validator
### (automatic)
The auto installation include snapshots or statesync any provider.

You can setup your ojo validator in few minutes by using automated script below. It will prompt you to input your validator node name!

{% hint style='danger' %}
For auto installation requirements Ubuntu 20.04/22.04 LTS and root access. The rest of the **RHEL family** doesn't support it
{% endhint %}

```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/devnet/ojo/ojo-devnet.sh > ojo-devnet.sh && chmod +x ojo-devnet.sh && ./ojo-devnet.sh
```
This version include binary cosmovisor on ojo
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/devnet/ojo/ojo-devnet-cosmovisor.sh > ojo-devnet-cosmovisor.sh && chmod +x ojo-devnet-cosmovisor.sh && ./ojo-devnet-cosmovisor.sh
```
This version include statesync on ojo
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/devnet/ojo/ojo-devnet-statesync.sh > ojo-devnet-statesync.sh && chmod +x ojo-devnet-statesync.sh && ./ojo-devnet-statesync.sh
```
## Post installation

When installation is finished please load variables into system
```
source $HOME/.bash_profile
```
