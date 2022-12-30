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

### Navigation:
* [Prepare](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#prepare)
* [All variables](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#all-variables)
* [Build and configuration](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#build-and-configuration)
* [Change PORT](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#change-port)
* [Memory optimization](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#memory-optimization)
* [Start node](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#start-node)
* [Create a validator](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#create-a-validator)
* [Snapshot](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#snapshot)
* [Update node](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#update-node)
* [Useful commands](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#useful-commands)
* [Delete node](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#delete-node)
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
#### Installing GO v1.19.3
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
# Input your data
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
## Build and configuration
```bash 
# Build binary 
git clone $PROJECT_REPOSITORY_PATH $PROJECT && \
cd $PROJECT && \
make install && \
$TIKER version --long
```
```bash 
# Initialisation (input SID from previous step) | ONE COMMAND
okp4d init $MONIKER --chain-id okp4-nemeton-1 && \
okp4d config chain-id okp4-nemeton-1 && \
okp4d config keyring-backend test && \
okp4d config node $NODE
```
```bash
# Add wallet
okp4d keys add $WALLET
# or 
okp4d keys add $WALLET --recover
```
```bash
# Set variables 
VALOPER=$(okp4d keys show $WALLET --bech val -a) && \
ADDRESS=$(okp4d keys show $WALLET --address) && \
echo "export VALOPER=$VALOPER" >> $HOME/.bash_profile && \
echo "export ADDRESS=$ADDRESS" >> $HOME/.bash_profile && \
source $HOME/.bash_profile
```
```bash 
# Peers and seeds
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/; s/^seeds *=.*/seeds = \"$SEEDS\"/" $HOME/$CONFIG/config/config.toml
```

## Change PORT
```bash
NODES_NUM="0"
```
```bash
sed -i.bak -e "\
s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:$((NODES_NUM+26))658\"%; \
s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://0.0.0.0:$((NODES_NUM+26))657\"%; \
s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:$((NODES_NUM+6))060\"%; \
s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:$((NODES_NUM+26))656\"%; \
s%^external_address = \"\"%external_address = \"`echo $(wget -qO- eth0.me):$((NODES_NUM+26))656`\"%; \
s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":$((NODES_NUM+26))660\"%" $HOME/$CONFIG/config/config.toml
```
```bash
sed -i.bak -e "\
s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:$((NODES_NUM+1))317\"%; \
s%^address = \":8080\"%address = \":$((NODES_NUM+8))080\"%; \
s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:$((NODES_NUM+9))090\"%; \
s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:$((NODES_NUM+9))091\"%" $HOME/$CONFIG/config/app.toml
```
```bash
echo "export NODE=http://localhost:$((NODES_NUM+26))657" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
okp4d config node $NODE
```
## Memory optimization
```bash 
# Memory optimization. Removes unused data from the database.
indexer="null" && \
min_retain_blocks=1 && \
snapshot_interval="100" && \
pruning="custom" && \
pruning_keep_recent="100" && \
pruning_keep_every="0" && \
pruning_interval="10" && \
min_retain_blocks="1" && \
inter_block_cache="false" && \
sed -i.bak -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/$CONFIG/config/config.toml && \
sed -i.bak -e "s/^min-retain-blocks *=.*/min-retain-blocks = \"$min_retain_blocks\"/" $HOME/$CONFIG/config/app.toml && \
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" $HOME/$CONFIG/config/app.toml && \
sed -i.bak -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/$CONFIG/config/app.toml && \
sed -i.bak -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/$CONFIG/config/app.toml && \
sed -i.bak -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/$CONFIG/config/app.toml && \
sed -i.bak -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/$CONFIG/config/app.toml && \
sed -i.bak -e "s/^min-retain-blocks *=.*/min-retain-blocks = \"$min_retain_blocks\"/" $HOME/$CONFIG/config/app.toml && \
sed -i.bak -e "s/^inter-block-cache *=.*/inter-block-cache = \"$inter_block_cache\"/" $HOME/$CONFIG/config/app.toml
```

# Start node
```bash 
# Create service 
sudo tee /etc/systemd/system/okp4d.service > /dev/null <<EOF
[Unit]
Description=$PROJECT Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which okp4d) start
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```
```bash
# Start service 
sudo systemctl daemon-reload && \
sudo systemctl enable okp4d && \
sudo systemctl restart okp4d && \
sudo journalctl -u okp4d -f -o cat
```
```bash
# Check synchronization of your node, if the result is false, the node is synchronized
curl -s $NODE/status | jq .result.sync_info.catching_up
```
## Create a validator
```bash 
okp4d tx staking create-validator \
  --amount=1000000uknow \
  --pubkey=$(okp4d tendermint show-validator) \
  --moniker=$MONIKER \
  --chain-id=okp4-nemeton-1 \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation=$DENOM \
  --fees=0uknow \
  --from=$WALLET \
  --identity=$IDENTITY \
  --website=$WEBSITE \
  --details=$DETAILS \
  --security-contact=$SECURITY_CONTACT \
  -y
  ```
## Snapshot
```bash
sudo systemctl stop okp4d && \
okp4d tendermint unsafe-reset-all --home $HOME/$CONFIG --keep-addr-book
```
```bash
# RPC example: 5.161.106.127:26657
SNAP_RPC=""
```
```bash
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT-100)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)
```
```bash
# Check data
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
# Output example (numbers will be different):
# 376080 374080 F0C78FD4AE4DB5E76A298206AE3C602FF30668C521D753BB7C435771AEA47189
```
```bash
# Peer example: 22cd56c20132817d609025f42c5e263e70157e64@5.161.106.127:26656
peers=""
sed -i.bak -e  "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/$CONFIG/config/config.toml
```
```bash
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/$CONFIG/config/config.toml
```
```bash
sudo systemctl restart okp4d && sudo journalctl -u okp4d -f -o cat
```
## Update node
```bash
TAG_NAME=""
```
```bash
sudo systemctl stop okp4d && \
cd $PROJECT && \
git pull; \
git checkout tags/$TAG_NAME && \
make clean; \
make install && \
sudo systemctl restart okp4d && \
journalctl -u okp4d -f -o cat
```
## Useful commands

### Node status
```bash
# Service logs
journalctl -u okp4d -f -o cat
```
```bash
# Service status
systemctl status okp4d
```
```bash
# Check node status
curl -s $NODE/status
```
```bash
# Check synchronization of your node, if the result is false, the node is synchronized
curl -s $NODE/status | jq .result.sync_info.catching_up
```
```bash
# Check consensus 
curl -s $NODE/consensus_state  | jq '.result.round_state.height_vote_set[0].prevotes_bit_array'
```
```bash
# Connected peers
curl -s $NODE/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr | split(":")[2])"' | wc -l
```

### Validator info
```bash
# Get validator address 
echo $VALOPER
```
```bash
# Get wallet address
echo $ADDRESS
```
```bash
# Jail, tombstoned, start_height, index_offset
okp4d q slashing signing-info $(okp4d tendermint show-validator)
```
```bash
# Get peer (e.g. 72cc19c8435d662677b2ea627e649f39b5bc8abb@5.161.70.110:26656
echo "$(okp4d tendermint show-node-id)@$(curl ifconfig.me):$(curl -s $NODE/status | jq -r '.result.node_info.listen_addr' | cut -d':' -f3)"
```
```bash
## Wallet
# Get balance
okp4d q bank balances $ADDRESS
```

### Voting
```bash
# Vote
okp4d tx gov vote <PROPOSAL_ID> <yes|no> --from $WALLET --fees 5000uknow -y
``
```bash
# Check all voted proposals
okp4d q gov proposals --voter $ADDRESS
```

### Actions
```bash
# Edit validator
okp4d tx staking edit-validator --website="<YOUR_WEBSITE>" --details="<YOUR_DESCRIPTION>" --moniker="<YOUR_NEW_MONIKER>" --from=$WALLET --fees 5000uknow
```
```bash
# Unjail
okp4d tx slashing unjail --from $WALLET --fees 5000uknow
```
```bash
# Bond more tokens (if you want increase your validator stake you should bond more to your valoper address):
okp4d tx staking delegate $VALOPER <TOKENS_COUNT>uknow--from $WALLET --fees 5000uknow -y
```
```bash
# Undelegate
okp4d tx staking unbond $VALOPER <TOKENS_COUNT>uknow --from $WALLET --fees 5000uknow -y
```
```bash
# Send tokens. 1 token = 1000000 (Cosmos)
okp4d tx bank send $WALLET <WALLET_TO> <TOKENS_COUNT>uknow --fees 5000uknow
# e.g. okp4d tx bank send $WALLET cosmos10h3t6rtrjwxqlw0jgwc540rthuclhvrzhndkeg 1000000uknow --gas auto
```
```bash
# Change peers and seeds
peers="<PEERS>"
seeds="<SEEDS>"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/; s/^seeds *=.*/seeds = \"$seeds\"/" $HOME/$PROJECT_CONFIG/config/config.toml
```
```bash
# Reset private validator file to genesis state and delete addrbook.json
okp4d tendermint unsafe-reset-all --home $HOME/$CONFIG
```

### Genesis
```bash
# Add genesis account
okp4d add-genesis-account $(archwayd keys show $ARCHWAY_WALLET -a) 1001000$TOKEN
```
```bash
# Create gentx
okp4d gentx $WALLET 1000000uknow \
  --commission-rate=0.1 \
  --commission-max-rate=0.2 \
  --commission-max-change-rate=0.1 \
  --pubkey $(okp4d tendermint show-validator) \
  --chain-id=okp4-nemeton-1 \
  --moniker="$MONIKER"
```

### All validators info
```bash
# List of all active validators 
okp4d q staking validators -o json --limit=1000 \
| jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' \
| jq -r '.tokens + " - " + .description.moniker' \
| sort -gr | nl
```
```bash
# List of all inactive validators 
okp4d q staking validators -o json --limit=1000 \
| jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' \
| jq -r '.tokens + " - " + .description.moniker' \
| sort -gr | nl
```

### Another useful commands
```bash
# Root -> your node
su -l $USER_NAME
```
```bash
# Check internet connection
curl -sL yabs.sh | bash -s -- -fg
```
```bash
# Server load
sudo htop
```
```bash
# File structure
ncdu
```
## Delete node
```bash
sudo systemctl stop okp4d && \
sudo systemctl disable okp4d; \
sudo rm /etc/systemd/system/okp4d.service; \
sudo systemctl daemon-reload && \
cd $HOME && \
rm -rf $CONFIG $PROJECT; \
sudo rm $(which okp4d)
```
