---
description: >-
  Setting up your validator node has never been so easy. Get your validator
  running in minutes by following step by step instructions.
---

# Installation

<figure><img src="../../.gitbook/assets/osmosis.png" alt=""><figcaption></figcaption></figure>

**Network:** Mainnet **| Chain ID:** osmosis-1 | **Node Version:** v1.4.0

## Set up your Osmosis Validator
### (automatic)
You can setup your osmosis validator in few minutes by using automated script below. It will prompt you to input your validator node name!
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/osmosis/osmosis.sh > osmosis.sh && chmod +x osmosis.sh && ./osmosis.sh
```
This version include binary cosmovisor on osmosis network
```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/osmosis/cosmovisor.sh > cosmovisor.sh && chmod +x cosmovisor.sh && ./cosmovisor.sh
```
## Post installation

When installation is finished please load variables into system
```
source $HOME/.bash_profile
```
