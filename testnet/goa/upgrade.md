# 🤝 Games Of Alliance

{% hint style="info" %}
if you get an error looking at logs on nodes try updating available new version.
{% endhint %}

| Executable | Node Version |
| ----| ------------ |
| [**ordosd**](ordos/) |v0.0.1-goa > v0.1.0-goa|
| [**harkonnend**](harkonnen/) |v0.0.1-goa > v0.1.0-goa|
| [**corrinod**](corrino/) |v0.0.1-goa > v0.1.0-goa|
| [**atreidesd**](atreides/) |v0.0.1-goa > v0.1.0-goa|

## Stop service and remove old executable

As an example we will use `ordosd`

#### Make a stop node :
```
sudo systemctl stop ordosd
```

#### Example path :
```
root@sxlzptprjkt:~# which ordosd
/usr/bin/ordosd < this path which we will remove
```

#### Remove a executable :
```
sudo rm -f /usr/bin/ordosd
```

#### Before upgrading you should check if your (GO)lang avaliable if not follow the command below

```
ver="1.19.5"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
source ~/.bash_profile
go version
```

## Updating binary and start service

```
cd $HOME
rm -rf alliance
git clone https://github.com/terra-money/alliance
cd alliance
git checkout v0.1.0-goa
make build-alliance ACC_PREFIX=ordos
```

#### Move binary to executable :

```
sudo mv build/ordosd /usr/bin/
```

#### Make a running node :

```
sudo systemctl start ordosd
```

#### Checking logs :
```
sudo journalctl -fu ordosd -o cat
```
