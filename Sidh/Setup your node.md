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

# устанавливаем необходимые утилиты
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
File2Ban - подробнее здесь и здесь

# устанавливаем и копируем конфиг, который будет иметь больший приоритет
apt install fail2ban -y && \
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local && \
nano /etc/fail2ban/jail.local
# раскомментировать и добавить свой IP: ignoreip = 127.0.0.1/8 ::1 <ip>
systemctl restart fail2ban

# проверяем status 
systemctl status fail2ban
# проверяем, какие jails активны (по умолчанию только sshd)
fail2ban-client status
# проверяем статистику по sshd
fail2ban-client status sshd
# смотрим логи
tail /var/log/fail2ban.log
# останавливаем работу и удаляем с автозагрузки
systemctl stop fail2ban && systemctl disable fail2ban
Устанавливаем GO

ver="1.19" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version

Новая установка ноды
ВАЖНО — в командах ниже все, что в <> меняем на свое значение и убираем сами <>

Устанавливаем бинарники

git clone https://github.com/okp4/okp4d && cd okp4d
git checkout v3.0.0
make install

okp4d version --long | grep -e version -e commit
# version: v3.0.0
# commit: 2b9893990a9708b9c9f4fa524fb90662337a7db8
Инициализируем ноду, чтобы создать необходимые файлы конфигурации

okp4d init <name_moniker> --chain-id okp4-nemeton-1
Скачиваем Genesis

wget -O $HOME/.okp4d/config/genesis.json "https://raw.githubusercontent.com/okp4/networks/main/chains/nemeton-1/genesis.json"

# Проверим генезис
sha256sum ~/.okp4d/config/genesis.json
# 2ec25f81cc2abecbc0da3de45b052ea3314d0d658b1b7f4c7b6a48d09254c742
Проверяем, что состояние валидатора на начальном этапе

cd && cat .okp4d/data/priv_validator_state.json
{
  "height": "0",
  "round": 0,
  "step": 0
}

# если нет, то выполняем команду
okp4d tendermint unsafe-reset-all --home $HOME/.okp4d
Скачиваем Addr book

wget -O $HOME/.okp4d/config/addrbook.json "https://share.utsa.tech/okp4/addrbook.json"

Настраиваем конфигурацию ноды
# правим конфиг, благодаря чему мы можем больше не использовать флаг chain-id для каждой команды CLI в client.toml
okp4d config chain-id okp4-nemeton-1

# при необходимости настраиваем keyring-backend в client.toml 
okp4d config keyring-backend os

# настраиваем минимальную цену за газ в app.toml
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025uknow\"/;" ~/.okp4d/config/app.toml

# добавляем seeds/bpeers/peers в config.toml
external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.okp4d/config/config.toml

peers="9c462b1c0ba63115bd70c3bd4f2935fcb93721d0@65.21.170.3:42656,ee4c5d9a8ac7401f996ef9c4d79b8abda9505400@144.76.97.251:12656,2e85c1d08cfca6982c74ef2b67251aa459dd9b2f@65.109.85.170:43656,264256d32511c512a0a9d4098310a057c9999fd1@okp4.sergo.dev:12233,4ea26ce893d8f4f89a7b49b9bd77e0fbd914e029@65.109.88.162:36656,8d8fdad759361a57121903632adbd66ad072b1ab@okp4-testnet.nodejumper.io:29656,e3c602b146121c88d350bd7e0f6ce8977e1aacff@161.97.122.216:26656,3c805c2dead7b7a3a1d3ba2399d4d62153322413@65.108.2.41:36656,9d1482bc31fb4578a5c7f7f65c4e0aaf2dfc2336@213.239.215.77:34656,a7f1dcf7441761b0e0e1f8c6fdc79d3904c22c01@[2a02:c206:2093:4875::1]:36656,a7f1dcf7441761b0e0e1f8c6fdc79d3904c22c01@38.242.150.63:36656,99f6675049e22a0216af0e2447e7a4c5021874cd@142.132.132.200:28656,9392c27a9a561c31e7a920dc6f577d663c473ef8@154.12.225.88:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.okp4d/config/config.toml

#bpeers=""
#sed -i.bak -e "s/^bootstrap-peers *=.*/bootstrap-peers = \"$bpeers\"/" $HOME/.okp4d/config/config.toml

seeds=""
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.okp4d/config/config.toml

# при необходимости увеличиваем количество входящих и исходящих пиров для подключения, за исключением постоянных пиров в config.toml
# может помочь при падении ноды, но увеличивает нагрузку
sed -i 's/max_num_inbound_peers =.*/max_num_inbound_peers = 50/g' $HOME/.okp4d/config/config.toml
sed -i 's/max_num_outbound_peers =.*/max_num_outbound_peers = 25/g' $HOME/.okp4d/config/config.toml

# настраиваем фильтрацию "плохих" peers
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.okp4d/config/config.toml

# изменение timeout_commit
#sed -i -e "s/^timeout_commit *=.*/timeout_commit = \"2s\"/" $HOME/.okp4d/config/config.toml

# отключаем JSON RPC Configuration в app.toml
# nano /root/.okp4d/config/app.toml
# enable = false
(ОПЦИОНАЛЬНО) Настраиваем прунинг одной командой вapp.toml

pruning="custom"
pruning_keep_recent="1000"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.okp4d/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.okp4d/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.okp4d/config/app.toml
(ОПЦИОНАЛЬНО) Выкл индексацию вconfig.toml

indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.okp4d/config/config.toml
(ОПЦИОНАЛЬНО) Вкл/выкл снэпшоты вapp.toml

# По умолчанию снэпшоты выключены "snapshot-interval=0"
snapshot_interval=1000
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" ~/.okp4d/config/app.toml
(ОПЦИОНАЛЬНО) Смена портов #для 2 ноды

# config.toml
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:36658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:36657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:6061\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:36656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":36660\"%" $HOME/.okp4d/config/config.toml

# app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:9190\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:9191\"%; s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:1327\"%" $HOME/.okp4d/config/app.toml

# client.toml
sed -i.bak -e "s%^node = \"tcp://localhost:26657\"%node = \"tcp://localhost:36657\"%" $HOME/.okp4d/config/client.toml

external_address=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:36656\"/" $HOME/.okp4d/config/config.toml
Подробнее о смене портов здесь

(ОПЦИОНАЛЬНО) State Sync 

!!!Для загрузки со statesync обязательно Вкл снэпшоты вapp.toml!!!

# при необходимости скачиваем wasm
curl -L https://share.utsa.tech/okp4/wasm-okp4.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.okp4d --strip-components 2
# добавляем пир
peers="1f2fe5c95dec0529e23cdcb233723c2708f58d51@65.108.6.45:61356"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.okp4d/config/config.toml
SNAP_RPC=https://t-okp4.rpc.utsa.tech:443

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.okp4d/config/config.toml
systemctl restart okp4d && journalctl -u okp4d -f -o cat
Важно - для разных блокчейнов нужно разное количество RAM для успешного старта со State sync


systemctl stop okp4d
#rm $HOME/.okp4d/config/addrbook.json
okp4d tendermint unsafe-reset-all --home $HOME/.okp4d --keep-addr-book
Создаем сервисный файл

tee /etc/systemd/system/okp4d.service > /dev/null <<EOF
[Unit]
Description=okp4d
After=network-online.target

[Service]
User=$USER
ExecStart=$(which okp4d) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
systemctl daemon-reload
systemctl enable okp4d
systemctl restart okp4d && journalctl -u okp4d -f -o cat
Если после старта нода долго не может подцепиться к пирам, то ищем новые пиры или просим addrbook.json в дискорд

# стопаем ноду, удаляем адресную книгу и сбрасываем данные
systemctl stop okp4d
rm $HOME/.okp4d/config/addrbook.json
okp4d tendermint unsafe-reset-all --home $HOME/.okp4d

# перезагружаем ноду
systemctl restart okp4d && journalctl -u okp4d -f -o cat
Cоздаем или восстанавливаем кошелек и сохраняем вывод

# создать кошелек
okp4d keys add <name_wallet> --keyring-backend os

# восстановить кошелек (после команды вставить seed)
okp4d keys add <name_wallet> --recover --keyring-backend os

# восстановить кошелек для EVM сетей
okp4d keys add <name_wallet> --recover --coin-type 118 --algo secp256k1
Не забываем сохранить seed !!!

Создаем валидатора

okp4d tx staking create-validator \
--chain-id okp4-nemeton-1 \
--commission-rate 0.05 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.1 \
--min-self-delegation "1000000" \
--amount 1000000uknow \
--pubkey $(okp4d tendermint show-validator) \
--moniker "<name_moniker>" \
--from <name_wallet>
Не забываем сохранить priv_validator_key.json !!!

Подробнее о создании/редактировании валидатора можно почитать здесь


Полезные команды
Для добавления лого в mintscan (ТОЛЬКО ДЛЯ MINTSCAN):

форк https://github.com/cosmostation/cosmostation_token_resource
в папке Moniker находим название проекта
через add file/upload file добавляем свою аватарку. название файла обязательно должно быть валопер.png . и только png
PR
Информация

# проверить блоки
okp4d status 2>&1 | jq ."SyncInfo"."latest_block_height"

# проверить логи
journalctl -u okp4d -f -o cat

# проверить статус
curl localhost:26657/status

# проверить баланс
okp4d q bank balances <address>

# проверить pubkey валидатора
okp4d tendermint show-validator

# проверить валидатора
okp4d query staking validator <valoper_address>
okp4d query staking validators --limit 1000000 -o json | jq '.validators[] | select(.description.moniker=="<name_moniker>")' | jq

# проверка информации по TX_HASH
okp4d query tx <TX_HASH>

# параметры сети
okp4d q staking params
okp4d q slashing params

# проверить сколько блоков пропущено валидатором и с какого блока актив
okp4d q slashing signing-info $(okp4d tendermint show-validator)

# узнать транзакцию создания валидатора (заменить свой valoper_address)
okp4d query txs --events='create_validator.validator=<your_valoper_address>' -o=json | jq .txs[0].txhash -r

# просмотр активного сета
okp4d q staking validators -o json --limit=1000 \
| jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' \
| jq -r '.tokens + " - " + .description.moniker' \
| sort -gr | nl

# просмотр неактивного сета
okp4d q staking validators -o json --limit=1000 \
| jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' \
| jq -r '.tokens + " - " + .description.moniker' \
| sort -gr | nl
Транзакции

# собрать реварды со всех валидаторов, которым делегировали (без комиссии)
okp4d tx distribution withdraw-all-rewards --from <name_wallet> --fees 5000uknow -y

# собрать реварды c отдельного валидатора или реварды + комиссию со своего валидатора
okp4d tx distribution withdraw-rewards <valoper_address> --from <name_wallet> --fees 5000uknow --commission -y

# заделегировать себе в стейк еще (так отправляется 1 монетa)
okp4d tx staking delegate <valoper_address> 1000000uknow --from <name_wallet> --fees 5000uknow -y

# ределегирование на другого валидатора
okp4d tx staking redelegate <src-validator-addr> <dst-validator-addr> 1000000uknow --from <name_wallet> --fees 5000uknow -y

# unbond 
okp4d tx staking unbond <addr_valoper> 1000000uknow --from <name_wallet> --fees 5000uknow -y

# отправить монеты на другой адрес
okp4d tx bank send <name_wallet> <address> 1000000uknow --fees 5000uknow -y

# выбраться из тюрьмы
okp4d tx slashing unjail --from <name_wallet> --fees 5000uknow -y
! Если транзакции не отправляются с ошибкой account sequence mismatch, expected 18, got 17: incorrect account sequence, то добавьте в команду ключ -s 18 (номер замените на тот, который ждет sequence)

Работа с кошельками

# вывести список кошельков
okp4d keys list

# показать ключ аккаунта
okp4d keys show <name_wallet> --bech acc

# показать ключ валидатора
okp4d keys show <name_wallet> --bech val

# показать ключ консенсуса
okp4d keys show <name_wallet> --bech cons

# показать все поддерживаемые адреса
okp4d debug addr <wallet_addr>

# показать приватный ключ
okp4d keys export <name_wallet> --unarmored-hex --unsafe

# запрос учетной записи
okp4d q auth account $(okp4d keys show <name_wallet> -a) -o text

# удалить кошелек
okp4d keys delete <name_wallet>
Удалить ноду

systemctl stop okp4d && \
systemctl disable okp4d && \
rm /etc/systemd/system/okp4d.service && \
systemctl daemon-reload && \
cd $HOME && \
rm -rf .okp4d okp4d && \
rm -rf $(which okp4d)
ГОВЕРНАНС (подробнее здесь)

# список proposals
okp4d q gov proposals

# посмотреть результат голосования
okp4d q gov proposals --voter <ADDRESS>

# проголосовать за предложение 
okp4d tx gov vote 1 yes --from <name_wallet> --fees 555uknow

# внести депозит в предложение
okp4d tx gov deposit 1 1000000uknow --from <name_wallet> --fees 555uknow

# создать предложение
okp4d tx gov submit-proposal --title="Randomly reward" --description="Reward 10 testnet participants who completed more than 3 tasks" --type="Text" --deposit="11000000grain" --from=<name_wallet> --fees 500grain
Peers and RPC

FOLDER=.okp4d

# узнать свой peer
PORTR=$(grep -A 3 "\[p2p\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+") && \
echo $(okp4d tendermint show-node-id)@$(curl ifconfig.me)$PORTR

# узнать порт RPC
echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/$FOLDER/config/config.toml | egrep -o ":[0-9]+")\033[0m"

# проверка количества пиров
PORT=
curl -s http://localhost:$PORT/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr | split(":")[2])"' | wc -l

# cписок моникеров подключенных пиров
curl -s http://localhost:$PORT/net_info | jq '.result.peers[].node_info.moniker'

# Проверка prevotes/precommits. Пригодится при обновах
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].prevotes_bit_array' && \
curl -s localhost:$PORT/consensus_state | jq '.result.round_state.height_vote_set[0].precommits_bit_array'

# check prevote of your validator
curl -s localhost:$PORT/consensus_state -s | grep $(curl -s localhost:26657/status | jq -r .result.validator_info.address[:12])
Поиск всех исходящих транзакций по адресу

okp4d q txs --events transfer.sender=<ADDRESS> 2>&1 | jq | grep txhash
Поиск всех входящих транзакций по адресу

okp4d q txs --events transfer.recipient=<ADDRESS> 2>&1 | jq | grep txhash
