---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../.gitbook/assets/8ball.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** eightball-1 | **Version:** v0.34.24

## Set up your 8ball Validator
### (automatic)
The auto installation include snapshots or statesync any provider.

You can setup your 8ball validator in few minutes by using automated script below. It will prompt you to input your validator node name!

{% hint style='danger' %}
For auto installation requirements Ubuntu 20.04/22.04 LTS and root access. The rest of the **RHEL family** doesn't support it
{% endhint %}

```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/8ball/8ball.sh > 8ball.sh && chmod +x 8ball.sh && ./8ball.sh
```
This version include binary cosmovisor on 8ball
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/8ball/8ball-cosmovisor.sh > 8ball-cosmovisor.sh && chmod +x 8ball-cosmovisor.sh && ./8ball-cosmovisor.sh
```
This version include statesync on 8ball
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/8ball/8ball-statesync.sh > 8ball-statesync.sh && chmod +x 8ball-statesync.sh && ./8ball-statesync.sh
```
## Post installation

When installation is finished please load variables into system
```
source $HOME/.bash_profile
```
