---
title: Ubuntu 上目前在运行的service及自动化脚本
tag: linux
date: 2019-07-07 16:42:22
---

由于比较尴尬的发现Azure vm的root password不知道，所以现在处于一直只能ssh上去，但是用不了root权限，之前的service还好，但是最近想添加新的service是办不到的。所以估计还要重新安装一遍，下面就记录一下上面在跑的service。

## Prepare
zsh and so on.
```
sudo apt update && sudo apt install git -y
chsh -s /bin/zsh
```
## MTProxy
```
nohup python3 mtprotoproxy.py &
```
## ehforwarderbot
```
nohup /home/zhishan/.local/bin/ehforwarderbot &
vi ~/.ehforwarderbot/profiles/default/zhangzhishan.filter/config.yaml
```

```
cat ~/.ehforwarderbot/profiles/default/config.yaml
master_channel: blueset.telegram
slave_channels:
       - blueset.wechat
middlewares:
        - zhangzhishan.filter
```

```
➜  ~ cat ~/.ehforwarderbot/profiles/default/blueset.telegram/config.yaml
token: "489086321:AAHDxV4fFDCmwmmucZi4KFhgDTndfDvpJuo"

admins: 80049719
flags:
        chats_per_page: 20
        network_error_prompt_interval: 250
```

```
version: 18.6
match_mode: fuzz
work_mode:
        - black_persons
        - white_groups
        - white_publics
white_persons:
        - Georgia
        - You
white_groups:
        - 经典
        - Georgia
        - 石总
        - 脚
        - V2EX
white_publics:
        - 银行
        - 微信
        - 汇丰
        - OK
black_persons:
        - nopo
```
## RSSHub
```
git clone https://github.com/zhangzhishan/RSSHub.git
```
## rssbot
```
wget https://github.com/iovxw/rssbot/releases/download/v1.4.4/rssbot-v1.4.4-linux.zip
unzip rssbot-v1.4.4-linux.zip
./rssbot rssbot 857369827:AAGTsmN8KwnYcfuV7sQmIUcBFBBVXmOZ0Lo
nohup ./rssbot DATAFILE TELEGRAM-BOT-TOKEN > /dev/null 2>&1 &
nohup ./rssbot rssbotjson 857369827:AAGTsmN8KwnYcfuV7sQmIUcBFBBVXmOZ0Lo > /dev/null 2>&1 &

```
## shadowsocksr

## syncthing

  599  mkdir programs
  600  cd programs
  601  mkdir proxy
  602  cd proxy
  603  git clone --branch akkariiin/master https://github.com/shadowsocksrr/shadowsocksr.git
  604  cd shadowsocksr
  605  bash initcfg.sh
  606  vim user-config.json
  607  mkdir -p ~/.config/systemd/user
  608  cd ~/.config/systemd/user
  609  vim shadowsocksr.service
  610  systemctl --user enable shadowsocksr
  611  systemctl --user start shadowsocksr
  612  ..
  613  ...
  614  cd programs/proxy/shadowsocksr
  615  vim shadowsocksr.service
  616  vim user-config.json
  617  systemctl --user status shadowsocksr
  618  which python
  619  cd ~/.config/systemd/user
  620  vim shadowsocksr.service
  621  systemctl --user start shadowsocksr
  622  systemctl --user daemon-reload
  623  systemctl --user status shadowsocksr
  624  vim shadowsocksr.service
  625  \n\n
  626  vim shadowsocksr.service
  627  pwd
  628  vim shadowsocksr.service
  629  cd ~/programs/proxy/shadowsocksr
  630  ls
  631  cd shadowsocks
  632  python server.py -c ../user-config.json
  633  vim ../user-config.json
  634  python server.py -c ../user-config.json
  635  vim ../user-config.json
  636  python server.py -c ../user-config.json
  637  systemctl -h
  638  systemctl --user start shadowsocksr
  639  systemctl --user daemon-reload
  640  systemctl --user start shadowsocksr
  641  systemctl --user status shadowsocksr
  642  vim ../user-config.json
  643  systemctl --user daemon-reload
  644  systemctl --user start shadowsocksr
  645  systemctl --user status shadowsocksr
  646  systemctl --user daemon-reload
  647  systemctl --user start shadowsocksr
  648  systemctl --user status shadowsocksr
  649  systemctl --user daemon-reload
  650  systemctl --user status shadowsocksr
  651  vim user-config.json
  652  cd shadowsocks
  653  python server.py -c ../user-config.json
  654  systemctl --user stop shadowsocksr
  655  systemctl --user status shadowsocksr
  656  python server.py -c ../user-config.json
  657  vim ../user-config.json
  658  python server.py -c ../user-config.json
  659  systemctl --user status shadowsocksr
  660  cd programs/proxy/shadowsocksr/shadowsocks
  661  python server.py -c ../user-config.json
  662  systemctl --user start shadowsocksr
  663  systemctl --user status shadowsocksr
  664  vim shadowsocksr.service
  665  cd ~/.config/systemd/user
  666  ls
  667  cat shadowsocksr.service
  668  cd ~/programs/proxy/shadowsocksr
  669  cat user-config.json
  670  cd ~/programs/proxy/shadowsocksr
  671  cd ~/.config/systemd/user
  758  touch rssbot.json
  759  ./rssbot rssbot.json 857369827:AAGTsmN8KwnYcfuV7sQmIUcBFBBVXmOZ0Lo
  760  ./rssbot rssbotjson 857369827:AAGTsmN8KwnYcfuV7sQmIUcBFBBVXmOZ0Lo
  761  ls
  762  ls rssbot
  763  git clone git@github.com:zhangzhishan/RSSHub.git
  765  cd RSSHub
  767  apt install nodejs
  768  sudo apt install nodejs
  769  sudo su
  770  sudo password root
  771  grep root /etc/shadow
  780  nohup /home/zhishan/.local/bin/ehforwarderbot &
