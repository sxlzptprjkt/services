---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../.gitbook/assets/lumenx.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** LumenX | **Version:** v1.3.3

## Set up your Arkhadian Validator
### (automatic)
The auto installation include snapshots or statesync any provider.

You can setup your lumenx validator in few minutes by using automated script below. It will prompt you to input your validator node name!

{% hint style='danger' %}
For auto installation requirements Ubuntu 20.04/22.04 LTS and root access. The rest of the **RHEL family** doesn't support it
{% endhint %}

```bash
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/lumenx/lumenx.sh > lumenx.sh && chmod +x lumenx.sh && ./lumenx.sh
```
This version include binary cosmovisor on lumenx
```bash
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/lumenx/lumenx-cosmovisor.sh > lumenx-cosmovisor.sh && chmod +x lumenx-cosmovisor.sh && ./lumenx-cosmovisor.sh.sh
```
This version include statesync on lumenx
```bash
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/lumenx/lumenx-statesync.sh > lumenx-statesync.sh && chmod +x lumenx-statesync.sh && ./lumenx-statesync.sh
```
## Post installation

When installation is finished please load variables into system
```bash
source $HOME/.bash_profile
```
