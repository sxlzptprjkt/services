---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../.gitbook/assets/planq.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet | **Chain ID:** planq_7070-2 | **Node Version:** v1.0.3

## Set up your Planq Validator
### (automatic)
You can setup your planq validator in few minutes by using automated script below. It will prompt you to input your validator node name!
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/planq/planq.sh > planq.sh && chmod +x planq.sh && ./planq.sh
```
This version include binary cosmovisor on planq network
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/planq/cosmovisor.sh > cosmovisor.sh && chmod +x cosmovisor.sh && ./cosmovisor.sh
```
## Post installation

When installation is finished please load variables into system
```
source $HOME/.bash_profile
```
