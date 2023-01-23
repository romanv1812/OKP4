
[<img src='https://user-images.githubusercontent.com/83868103/210116122-8e1f35b7-1578-4856-b02c-734154fc40c9.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4)
[<img src='https://user-images.githubusercontent.com/83868103/210116164-088cf8f8-868e-477f-9659-3c184dc22868.png' alt='gentx'  width='16.2%'>](https://nemeton.okp4.network/leaderboard#leaderboard)
[<img src='https://user-images.githubusercontent.com/83868103/210116187-9d459a76-e956-481a-92b6-6c11e6e97db2.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4)
[<img src='https://user-images.githubusercontent.com/83868103/210116210-6997e3df-0e06-47c4-9ffd-c1c3b692431c.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/rewards.md)
[<img src='https://user-images.githubusercontent.com/83868103/210116228-414bb6b0-f66f-4207-bb6f-12774d803daa.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/FAQ.md)
[<img src='https://user-images.githubusercontent.com/83868103/210116267-bfff3f45-b653-4d5a-a58e-d9e046f79e96.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/Terms.md)
## Description:
### Provide a public RPC endpoint.

**Rewards:**  
1 500 points.

**Judging Criteria:**  
You will receive the points once the OKP4 team has checked your RPC endpoint availability.

**How to Submit:**  
Share the RPC endpoint on this [form](https://okp4.typeform.com/Nemeton-RPC). Only one submission per druid will be studied.

## Completing a task

To complete the task, you need to install an additional node, it is not recommended to use a validator node with open RPC. You can install with the installation [guide](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md) from the first phase. After successful installation and full synchronization of the node, you need to make changes to config.toml and app.toml .

### RPC opening 


Open access to RPC nano $HOME/.okp4d/config/config.toml
```
laddr = "tcp://0.0.0.0:26657" # If you have used port change, the port may be different
```

Open app.toml and set up snapshots nano $HOME/.okp4d/config/app.toml
```
snapshot-interval = 100 # 0 - disable
snapshot-keep-recent = 2
```

Node restart
```bash
sudo systemctl restart okp4d && \
sudo journalctl -u okp4d -f -o cat
```
Print RPC address
```bash
echo http://$(wget -qO- eth0.me)$(echo -e "\033[0;32m$(grep -A 3 "\[rpc\]" ~/.okp4d/config/config.toml | egrep -o ":[0-9]+")\033[0m")
```

