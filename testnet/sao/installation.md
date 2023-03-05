---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../.gitbook/assets/sao.png" alt=""><figcaption></figcaption></figure>

**Network:** Testnet | **Chain ID:** sao-testnet0 | **Version:** testnet0

## Set up your sao Validator
### (automatic)
The auto installation include snapshots or statesync any provider.

You can setup your sao validator in few minutes by using automated script below. It will prompt you to input your validator node name!

{% hint style='danger' %}
For auto installation requirements Ubuntu 20.04/22.04 LTS and root access. The rest of the **RHEL family** doesn't support it
{% endhint %}

```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/sao/sao-testnet0.sh > sao-testnet0.sh && chmod +x sao-testnet0.sh && ./sao-testnet0.sh
```
This version include binary cosmovisor on sao
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/sao/sao-testnet0-cosmovisor.sh > sao-testnet0-cosmovisor.sh && chmod +x sao-testnet0-cosmovisor.sh && ./sao-testnet0-cosmovisor.sh
```
This version include statesync on sao
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/testnet/sao/sao-testnet0-statesync.sh > sao-testnet0-statesync.sh && chmod +x sao-testnet0-statesync.sh && ./sao-testnet0-statesync.sh
```
## Post installation

When installation is finished please load variables into system
```
source $HOME/.bash_profile
```
