## Setup your node Dec 14th - Jan 1st, 3pm UTC


> ### Description: 
> It is time to make the okp4-nemeton-1 network alive; you have to set up your node and join the network. 
> The technical documentation regarding node setup and network join information is here: https://docs.okp4.network/nodes/introduction.   
> 
> ### Rewards:
> 2000 points.    
> 
> ### Judging criteria: 
> Your validator is up and running.
> 
> ### How to submit:
> The validator's presence in the consensus will be automatically checked.
#
## Prepare
### Update if needed and install packages
```bash
sudo apt update && sudo apt upgrade -y && \
sudo apt install curl tar wget clang pkg-config libssl-dev libleveldb-dev jq build-essential bsdmainutils git make ncdu htop screen unzip bc fail2ban htop -y
```

#### Disable access by password 
```bash
sed -i -E 's/#?PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config && \
sudo systemctl restart ssh.service
```

#### Intall fail2ban 
```bash
cd /etc/fail2ban/ && \
sudo cp jail.conf jail.local
````
### Installing GO v1.19.3
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

## All variables
```bash
MONIKER="<moniker>"
WALLET="wallet"
WEBSITE="<Your website>"
IDENTITY="<Your ID https://keybase.io>"
DETAILS="<description>"
SECURITY_CONTACT="<your email>"
```
```bash
TIKER=okp4d && \
CHAIN=okp4-nemeton-1 && \
TOKEN=uknow && \
PROJECT=okp4d && \
CONFIG=.okp4d && \
PROJECT_REPOSITORY_PATH="https://github.com/okp4/okp4d.git" && \
PEERS="9c462b1c0ba63115bd70c3bd4f2935fcb93721d0@65.21.170.3:42656,ee4c5d9a8ac7401f996ef9c4d79b8abda9505400@144.76.97.251:12656,2e85c1d08cfca6982c74ef2b67251aa459dd9b2f@65.109.85.170:43656,264256d32511c512a0a9d4098310a057c9999fd1@okp4.sergo.dev:12233,4ea26ce893d8f4f89a7b49b9bd77e0fbd914e029@65.109.88.162:36656,8d8fdad759361a57121903632adbd66ad072b1ab@okp4-testnet.nodejumper.io:29656,e3c602b146121c88d350bd7e0f6ce8977e1aacff@161.97.122.216:26656,3c805c2dead7b7a3a1d3ba2399d4d62153322413@65.108.2.41:36656,9d1482bc31fb4578a5c7f7f65c4e0aaf2dfc2336@213.239.215.77:34656,a7f1dcf7441761b0e0e1f8c6fdc79d3904c22c01@[2a02:c206:2093:4875::1]:36656,a7f1dcf7441761b0e0e1f8c6fdc79d3904c22c01@38.242.150.63:36656,99f6675049e22a0216af0e2447e7a4c5021874cd@142.132.132.200:28656,9392c27a9a561c31e7a920dc6f577d663c473ef8@154.12.225.88:26656" && \
DENOM="1000000" && \
NODE="http://localhost:26657" && \
GENESIS_JSON_PATH="https://raw.githubusercontent.com/okp4/networks/main/chains/nemeton-1/genesis.json"
```
```bash
echo "export MONIKER=$MONIKER" >> $HOME/.bash_profile && \
echo "export WALLET=$WALLET" >> $HOME/.bash_profile && \
echo "export WEBSITE=$WEBSITE" >> $HOME/.bash_profile && \
echo "export IDENTITY=$IDENTITY" >> $HOME/.bash_profile && \
echo "export DETAILS=$DETAILS" >> $HOME/.bash_profile && \
echo "export SECURITY_CONTACT=$SECURITY_CONTACT" >> $HOME/.bash_profile && \
echo "export TIKER=$TIKER" >> $HOME/.bash_profile && \
echo "export CHAIN=$CHAIN" >> $HOME/.bash_profile && \
echo "export TOKEN=$TOKEN" >> $HOME/.bash_profile && \
echo "export PROJECT=$PROJECT" >> $HOME/.bash_profile && \
echo "export CONFIG=$CONFIG" >> $HOME/.bash_profile && \
echo "export PROJECT_REPOSITORY_PATH=$PROJECT_REPOSITORY_PATH" >> $HOME/.bash_profile && \
echo "export PEERS=$PEERS" >> $HOME/.bash_profile && \
echo "export DENOM=$DENOM" >> $HOME/.bash_profile && \
echo "export NODE=$NODE" >> $HOME/.bash_profile && \
echo "export GENESIS_JSON_PATH=$GENESIS_JSON_PATH" >> $HOME/.bash_profile && \
source $HOME/.bash_profile
```
