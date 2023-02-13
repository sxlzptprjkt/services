---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../../.gitbook/assets/atreides.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** atreides-1 | **Version:** v0.0.1-goa

## Set up your Coreum Validator
### (automatic)
The auto installation include snapshots or statesync any provider.

You can setup your atreides validator in few minutes by using automated script below. It will prompt you to input your validator node name!

{% hint style='danger' %}
For auto installation requirements Ubuntu 20.04/22.04 LTS and root access. The rest of the **RHEL family** doesn't support it
{% endhint %}

This basic version start block 1 on atreides
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/goa/atreides/atreides.sh > atreides.sh && chmod +x atreides.sh && ./atreides.sh
```
This version include statesync on atreides
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/goa/atreides/atreides-statesync.sh > atreides-statesync.sh && chmod +x atreides-statesync.sh && ./atreides-statesync.sh
```
## Post installation

When installation is finished please load variables into system
```
source $HOME/.bash_profile
```
