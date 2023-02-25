---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../.gitbook/assets/arkh.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** arkh | **Version:** v2.0.0

## Set up your Arkhadian Validator
### (automatic)
The auto installation include snapshots or statesync any provider.

You can setup your arkhadian validator in few minutes by using automated script below. It will prompt you to input your validator node name!

{% hint style='danger' %}
For auto installation requirements Ubuntu 20.04/22.04 LTS and root access. The rest of the **RHEL family** doesn't support it
{% endhint %}

```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/arkh/arkh.sh > arkh.sh && chmod +x arkh.sh && ./arkh.sh
```
This version include binary cosmovisor on arkh
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/arkh/arkh-cosmovisor.sh > arkh-cosmovisor.sh && chmod +x arkh-cosmovisor.sh && ./arkh-cosmovisor.sh.sh
```
This version include statesync on arkh
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/arkh/arkh-statesync.sh > arkh-statesync.sh && chmod +x arkh-statesync.sh && ./arkh-statesync.sh
```
## Post installation

When installation is finished please load variables into system
```
source $HOME/.bash_profile
```
