```bash
sudo apt update && sudo apt upgrade -y && \
sudo apt install curl tar wget clang pkg-config libssl-dev libleveldb-dev jq build-essential bsdmainutils git make ncdu htop screen unzip bc fail2ban htop -y
```

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

```bash
git clone https://github.com/okp4/okp4d && cd okp4d
git checkout v3.0.0
make install
```

```bash
okp4d version --long | grep -e version -e commit
```

version: v3.0.0  
commit: 2b9893990a9708b9c9f4fa524fb90662337a7db8

```bash
okp4d init <name_node> --chain-id okp4-nemeton-1
```
```bash
okp4d keys add <name_wallet>
```
or
```bash
okp4d keys add <name_wallet> --recover
```
```bash
okp4d add-genesis-account <address_or_key_name> 10000200000uknow
```
```bash
okp4d gentx <name_wallet> 10000000000uknow \
--chain-id okp4-nemeton-1 \
--commission-rate=0.05 \
--commission-max-rate=0.2 \
--commission-max-change-rate=0.01 \
--pubkey $(okp4d tendermint show-validator) \
--moniker <"name_moniker">
```
