[<img src='https://user-images.githubusercontent.com/83868103/210090468-0b75d872-4ded-46e9-94c9-eac2fd5f0f33.png' alt='linktr.ee/okp4'  width='100%'>](https://linktr.ee/okp4)
# Navigation
[Обычная ссылка в строке](https://www.google.com)
* [Information about the project](https://github.com/romanv1812/OKP4/edit/main/README.md#okp4-protocol)
* [Nemeton Program](https://github.com/romanv1812/OKP4/edit/main/README.md#nemeton-program)
* [Guides](https://github.com/romanv1812/OKP4/edit/main/README.md#guides)
* 4
* 5
* 6
* 7
* 8
# OKP4 Protocol

OKP4 is a domain-specific layer-1 dedicated to trust-minimized data sharing.
The blockchain orchestrates assets shared by participants into the Dataverse: data, algorithms, software, storage and computation to enable a new generation of applications.
Any contributor earns rewards thanks to these new value chains.

Join Data Spaces. Or create ones on your own terms.

[<img src='https://user-images.githubusercontent.com/83868103/210096401-114ed818-47fe-4d85-964c-7d77718d7bb8.png' alt='discord'  width='32.5%'>](https://discordapp.com/users/303453296755212288)
[<img src='https://user-images.githubusercontent.com/83868103/210096453-62220e2f-9954-4575-8a33-2065d8c88a5b.png' alt='discord'  width='32.5%'>](https://discordapp.com/users/303453296755212288)
[<img src='https://user-images.githubusercontent.com/83868103/210096484-315293d0-cd17-45d0-9500-70d0188e12c5.png' alt='discord'  width='32.5%'>](https://discordapp.com/users/303453296755212288)

# Nemeton Program
**Nodes**  
Submit your gentx on time

1 000 Pts

Setup your node

2 000 Pts

Community
Tweet about the OKP4 testnet

500 Pts

Challenges
Uptime challenge

2 500 Pts

Submit an original content related to validation

10 000 Pts

# Guides
  
# Server preparation: 



```bash
sudo apt update && sudo apt upgrade -y && \
sudo apt install curl tar wget clang pkg-config libssl-dev libleveldb-dev jq build-essential bsdmainutils git make ncdu htop screen unzip bc fail2ban htop -y
```

```bash
cd $HOME && \
ver="1.19.2" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```
