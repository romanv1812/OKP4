[<img src='https://user-images.githubusercontent.com/83868103/210116122-8e1f35b7-1578-4856-b02c-734154fc40c9.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4)
[<img src='https://user-images.githubusercontent.com/83868103/210116164-088cf8f8-868e-477f-9659-3c184dc22868.png' alt='gentx'  width='16.2%'>](https://nemeton.okp4.network/leaderboard#leaderboard)
[<img src='https://user-images.githubusercontent.com/83868103/210116187-9d459a76-e956-481a-92b6-6c11e6e97db2.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4)
[<img src='https://user-images.githubusercontent.com/83868103/210116210-6997e3df-0e06-47c4-9ffd-c1c3b692431c.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/rewards.md)
[<img src='https://user-images.githubusercontent.com/83868103/210116228-414bb6b0-f66f-4207-bb6f-12774d803daa.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/FAQ.md)
[<img src='https://user-images.githubusercontent.com/83868103/210116267-bfff3f45-b653-4d5a-a58e-d9e046f79e96.png' alt='gentx'  width='16.2%'>](https://github.com/romanv1812/OKP4/blob/main/Terms.md)
## Description:
### Provide network snapshots.

**Rewards:**  
2 000 points.

**Judging Criteria:**  
You will receive the points once the OKP4 team has checked your snapshots availability.

**How to Submit:**  
Share the link to your snapshots on this [form](https://okp4.typeform.com/NemetonSnapshot). Only one submission per druid will be studied.

## Completing a task
To complete the task, you need to install an additional node, you cannot use the validator node to create snapshots (you need to stop the node, which will lead to loss of uptime). You can install with the installation [guide](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md) from the first phase. After successful installation and full synchronization of the node, you can start creating a snapshot.


### Snapshot creation:
#### 
Creating a snapshot directory
```bash
mkdir $HOME/snapshot-share 
```
#### Before creating a snapshot, you need to stop the node:
```bash
sudo systemctl stop okp4d
```

#### Archiving the current state of the $HOME/.okp4d/data folder:
```bash
tar -cf - $HOME/.okp4d/data/ | lz4 - $HOME/snapshot-share/okp4-snap.tar -f
```

#### Run a node after creating an archive:
```bash
sudo systemctl start okp4d
```

### Snapshot distribution:

#### Tmux setup:
```bash
sudo apt install tmux 
```
#### Creating a new session in tmux
```bash
tmux new-session -s okp4-snapshot
```
#### Starting the server to distribute the snapshot
```bash
cd $HOME/snapshot-share && \
sudo python3 -m http.server 1000
```
#### Use ctrl+b d to disconnect from tmux session
#### To get the snapshot download address, use the command 
```bash
echo -e "\033[0;31m http://$(wget -qO- eth0.me):1000/snapshot-share/okp4-snap.tar.lz4 \033[0m"
```
