[<img src='https://user-images.githubusercontent.com/83868103/210116122-8e1f35b7-1578-4856-b02c-734154fc40c9.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4)
[<img src='https://user-images.githubusercontent.com/83868103/210116164-088cf8f8-868e-477f-9659-3c184dc22868.png' alt='gentx'  width='16.2%'>](https://nemeton.okp4.network/leaderboard#leaderboard)
[<img src='https://user-images.githubusercontent.com/83868103/210116187-9d459a76-e956-481a-92b6-6c11e6e97db2.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4)
[<img src='https://user-images.githubusercontent.com/83868103/210116210-6997e3df-0e06-47c4-9ffd-c1c3b692431c.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/rewards.md)
[<img src='https://user-images.githubusercontent.com/83868103/210116228-414bb6b0-f66f-4207-bb6f-12774d803daa.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/FAQ.md)
[<img src='https://user-images.githubusercontent.com/83868103/210116267-bfff3f45-b653-4d5a-a58e-d9e046f79e96.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/Terms.md)
## Description:

### Observability is a key element for monitoring network behavior and usage and detect possible anomalies, and as a validator you have access to a lot of information and metrics. Create and expose a Dashboards with useful KPI and metrics about the Network.

**Rewards:**  
2 000 points.

**Judging Criteria:**  
OKP4 team will judge if any submission deserves points or not based on the following:
* Overall relevance
* Readability
* Usefulness

Non-relevant submissions or low-value ones will earn 0 points.

**How to Submit:**  
Share the link to your dashboard on this [form](https://okp4.typeform.com/NemetonDashboar). Only one submission per druid will be studied.
## Completing a task


#### Variable zone
```bash
TOKEN=uknow
PREFIX=okp4
RPC_PORT=26657
GRPC_PORT=9090
```
#### Install cosmos-exporter
```bash
wget https://github.com/solarlabsteam/cosmos-exporter/releases/download/v0.2.2/cosmos-exporter_0.2.2_Linux_x86_64.tar.gz
tar xvfz cosmos-exporter*
sudo cp ./cosmos-exporter /usr/bin
rm cosmos-exporter* -rf
```
#### Add user
```bash
sudo useradd -rs /bin/false cosmos_exporter
```

```bash
sudo tee <<EOF >/dev/null /etc/systemd/system/cosmos-exporter.service
[Unit]
Description=Cosmos Exporter
After=network-online.target

[Service]
User=cosmos_exporter
Group=cosmos_exporter
TimeoutStartSec=0
CPUWeight=95
IOWeight=95
ExecStart=cosmos-exporter --denom ${BOND_DENOM} --denom-coefficient 1000000 --bech-prefix ${BENCH_PREFIX} --tendermint-rpc http://localhost:${RPC_PORT} --node localhost:${GRPC_PORT}
Restart=always
RestartSec=2
LimitNOFILE=800000
KillSignal=SIGTERM

[Install]
WantedBy=multi-user.target
EOF
```
