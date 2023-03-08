---
description: >-
  The price feeder tool is an extension of Ojo's x/oracle module, both of which
  are based on Terra's x/oracle module and oracle-feeder.
---

# Price Feeder

<figure><img src="../../.gitbook/assets/ojo.png" alt=""><figcaption></figcaption></figure>

**Network:** Devnet | **Chain ID:** ojo-devnet | **Version:** v0.1.1 (price-feeder)

## Set up your ojo price feeder

{% hint style="info" %}
To run pricefeeder you validator should be in active set. Otherwise price feeder will not vote on periods.
{% endhint %}

**Create new wallet for pricefeeder and save `24 word mnemonic phrase`**

```
ojod keys add pricefeeder-wallet --keyring-backend os
```
After [**installation**](installation.md) you can setup your ojo price feeder using automated script below. It will prompt you to input your main wallet / pricefeeder name!

```
curl -sL https://raw.githubusercontent.com/sxlzptprjkt/resource/master/devnet/ojo/ojo-devnet-pricefeeder.sh > ojo-devnet-pricefeeder.sh && chmod +x ojo-devnet-pricefeeder.sh && ./ojo-devnet-pricefeeder.sh
```

Check linked pricefeeder address

```
ojod query oracle feeder-delegation <YOUR_VALOPER_ADDRESS>
```
