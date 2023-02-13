---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../../.gitbook/assets/ordos.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** ordos-1 | **Version:** v0.0.1-goa

## Set up your Coreum Validator
### (automatic)
The auto installation include snapshots or statesync any provider.

You can setup your planq validator in few minutes by using automated script below. It will prompt you to input your validator node name!

{% hint style='danger' %}
For auto installation requirements Ubuntu 20.04/22.04 LTS and root access. The rest of the **RHEL family** doesn't support it
{% endhint %}

This version include snapshots on ordos
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/goa/ordos/ordos-snapshot.sh > ordos-snapshot.sh && chmod +x ordos-snapshot.sh && ./ordos-snapshot.sh
```
This version include statesync on ordos
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/goa/ordos/ordos-statesync.sh > ordos-statesync.sh && chmod +x ordos-statesync.sh && ./ordos-statesync.sh
```
## Post installation

When installation is finished please load variables into system
```
source $HOME/.bash_profile
```
