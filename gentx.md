
## Submit Gentx (From Dec 1st to Dec 12th)
### Update if needed and install packages
```bash
sudo apt update && sudo apt upgrade -y && \
sudo apt install curl tar wget clang pkg-config libssl-dev libleveldb-dev jq build-essential bsdmainutils git make ncdu htop screen unzip bc fail2ban htop -y
```
### Installing GO v1.18.3
```bash
cd $HOME && \
ver="1.18.3" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```


### Installing the binaries:

```bash
git clone https://github.com/okp4/okp4d && cd okp4d
git checkout v3.0.0
make install
okp4d version --long | grep -e version -e commit
# version: v3.0.0
# commit: 2b9893990a9708b9c9f4fa524fb90662337a7db8
```



### Initializing the node to create the necessary configuration files:
```bash 
okp4d init <name_node> --chain-id okp4-nemeton-1
```

### Create or restore a wallet
```bash
okp4d keys add <name_wallet>
# Don't forget to save the seed phrase
```
or
```bash
okp4d keys add <name_wallet> --recover
```

### Add an account to the local genesis file
```bash
okp4d add-genesis-account <address_or_key_name> 10000200000uknow
```


### Creation of Gentx 
```bash
okp4d gentx <name_wallet> 10000000000uknow \
--chain-id okp4-nemeton-1 \
--commission-rate=0.05 \
--commission-max-rate=0.2 \
--commission-max-change-rate=0.01 \
--pubkey $(okp4d tendermint show-validator) \
--moniker <"name_moniker">
```
### Don't forget to save priv_validator_key!!!

### View Gentx
```bash
cat ~/.okp4d/config/gentx/gentx-*
```

### Make issue [here](https://github.com/okp4/networks/issues)
### Send the issue number to the discord Anik#9282
