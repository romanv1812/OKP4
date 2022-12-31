[<img src='https://user-images.githubusercontent.com/83868103/210116122-8e1f35b7-1578-4856-b02c-734154fc40c9.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4)
[<img src='https://user-images.githubusercontent.com/83868103/210116164-088cf8f8-868e-477f-9659-3c184dc22868.png' alt='gentx'  width='16.2%'>](https://nemeton.okp4.network/leaderboard#leaderboard)
[<img src='https://user-images.githubusercontent.com/83868103/210116187-9d459a76-e956-481a-92b6-6c11e6e97db2.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4)
[<img src='https://user-images.githubusercontent.com/83868103/210116210-6997e3df-0e06-47c4-9ffd-c1c3b692431c.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/rewards.md)
[<img src='https://user-images.githubusercontent.com/83868103/210116228-414bb6b0-f66f-4207-bb6f-12774d803daa.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/FAQ.md)
[<img src='https://user-images.githubusercontent.com/83868103/210116267-bfff3f45-b653-4d5a-a58e-d9e046f79e96.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/Terms.md)
## Submit Gentx (From Dec 1st to Dec 12th)
> ### Description: 
> Before starting the network, we must to register your validator in the genesis.json.   
> The gentx creation and registration procedure are detailed here: https://github.com/okp4/networks/tree/main/chains/nemeton-1.   
> Your gentx shall be submitted through an issue on the https://github.com/okp4/networks/ GitHub repository.   
> This task is required to make you visible on the [Leaderboard](https://nemeton.okp4.network/leaderboard#leaderboard).   
> 
> ### Rewards:
> 1000 points.    
> 
> ### Judging criteria: 
> You will receive the points once the OKP4 team has integrated your gentx in the genesis.
> 
> ### How to submit:
> Send the issue number in a private message to Anik#9282 on Discord.
#
### Update if needed and install packages:
```bash
sudo apt update && sudo apt upgrade -y && \
sudo apt install curl tar wget clang pkg-config libssl-dev libleveldb-dev jq build-essential bsdmainutils git make ncdu htop screen unzip bc fail2ban htop -y
```
### Installing GO v1.19.3:
```bash
cd $HOME && \
ver="1.19.3" && \
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

### Create or restore a wallet:
```bash
okp4d keys add <name_wallet>
# Don't forget to save the seed phrase
```
or
```bash
okp4d keys add <name_wallet> --recover
```

### Add an account to the local genesis file:
```bash
okp4d add-genesis-account <address_or_key_name> 10000200000uknow
```


### Creation of Gentx:
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

### View Gentx:
```bash
cat ~/.okp4d/config/gentx/gentx-*
```

### Make issue [here](https://github.com/okp4/networks/issues)
### Send the issue number to the discord Anik#9282

[<img src='https://user-images.githubusercontent.com/83868103/210116122-8e1f35b7-1578-4856-b02c-734154fc40c9.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4)
[<img src='https://user-images.githubusercontent.com/83868103/210116164-088cf8f8-868e-477f-9659-3c184dc22868.png' alt='gentx'  width='16.2%'>](https://nemeton.okp4.network/leaderboard#leaderboard)
[<img src='https://user-images.githubusercontent.com/83868103/210116187-9d459a76-e956-481a-92b6-6c11e6e97db2.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4)
[<img src='https://user-images.githubusercontent.com/83868103/210116210-6997e3df-0e06-47c4-9ffd-c1c3b692431c.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/rewards.md)
[<img src='https://user-images.githubusercontent.com/83868103/210116228-414bb6b0-f66f-4207-bb6f-12774d803daa.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/FAQ.md)
[<img src='https://user-images.githubusercontent.com/83868103/210116267-bfff3f45-b653-4d5a-a58e-d9e046f79e96.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/Terms.md)
