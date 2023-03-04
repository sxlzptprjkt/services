---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../.gitbook/assets/planq.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** planq_7070-2 | **Version:** v1.0.3

## Set up your Planq Validator
### (automatic)
The auto installation include snapshots or statesync any provider.

You can setup your planq validator in few minutes by using automated script below. It will prompt you to input your validator node name!

{% hint style='danger' %}
For auto installation requirements Ubuntu 20.04/22.04 LTS and root access. The rest of the **RHEL family** doesn't support it
{% endhint %}

```bash
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/planq/planq.sh > planq.sh && chmod +x planq.sh && ./planq.sh
```
This version include binary cosmovisor on planq network
```bash
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/planq/planq-cosmovisor.sh > planq-cosmovisor.sh && chmod +x planq-cosmovisor.sh && ./planq-cosmovisor.sh
```
This version include statesync on planq network
```bash
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/mainnet/planq/planq-statesync.sh > planq-statesync.sh && chmod +x planq-statesync.sh && ./planq-statesync.sh
```
## Post installation

When installation is finished please load variables into system
```bash
source $HOME/.bash_profile
```
