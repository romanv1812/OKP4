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

## Phase 1 **"SIDH"**
![image](https://user-images.githubusercontent.com/83868103/210104134-8de51fd2-3a57-47da-b1e9-52c7af551183.png)




[![Typing SVG](https://readme-typing-svg.herokuapp.com?font=Denver-Serial&size=25&pause=1000&color=50827B&center=true&vCenter=true&width=1000&lines=+Duration%3A+4+weeks++;From+Dec.+1st+to+Jan.+1st;It's+time+to+choose+a+task+%E2%87%A9;Good+luck+%F0%9F%A7%99%F0%9F%8F%BC)](https://git.io/typing-svg)

![1 000 pts](https://user-images.githubusercontent.com/83868103/210101706-d02c9c8d-b6d0-4d19-9638-74ca3f898651.png)
![2 000 pts](https://user-images.githubusercontent.com/83868103/210101795-14194d32-fd2f-43a9-bf8f-336dba6c38d4.png)
![ 500 pts](https://user-images.githubusercontent.com/83868103/210102101-2623a3b3-0b44-4166-91e5-9a114c223ad2.png)
![2 500 pts](https://user-images.githubusercontent.com/83868103/210102239-43d1dbeb-8062-48e3-96c1-0028f567fe92.png)
![10 000 pts](https://user-images.githubusercontent.com/83868103/210102341-65813fbe-789c-4ec7-8fa0-10e937371088.png)
[![Typing SVG](https://readme-typing-svg.herokuapp.com?font=Fira+Code&pause=1000&width=435&lines=+Duration%3A+4+weeks++;From+Dec.+1st+to+Jan.+1st)](https://git.io/typing-svg)





|                       Task                       |   Reward   |
| ------------------------------------------------ | ---------- |
| Submit your gentx on time                        | 1 000 Pts  |
| Setup your node                                  | 2 000 Pts  |
| Tweet about the OKP4 testnet                     | 500 Pts    | 
| Uptime challenge                                 | 2 500 Pts  |
| Submit an original content related to validation | 10 000 Pts |

**Duration:** 4 weeks

**From Dec. 1st to Jan. 1st**

## Phase 2 **"IMBOLC"**
<img width="1303" alt="image" src="https://user-images.githubusercontent.com/83868103/210098870-c2833e0b-c71d-4343-972c-9fc77b4564c9.png">

## Phase 3 **"BELTAINE"**
<img width="1302" alt="image" src="https://user-images.githubusercontent.com/83868103/210098770-22b9e918-7ef9-4377-aeec-bbb7a5a16ce0.png">

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
