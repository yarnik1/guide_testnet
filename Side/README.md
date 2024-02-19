# Side Testnet guide

Ubuntu 22.04.3 LTS

~~~
TIKER=sided
~~~

### Install GO
~~~
ver="1.20.5"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
~~~

### Build 18.02.24
~~~
cd $HOME && mkdir -p go/bin/
git clone -b dev https://github.com/sideprotocol/sidechain.git
cd sidechain
git checkout 0.0.1-75-gbd63479
make install
~~~
~~~
sided version
~~~

### init
```sided init WellNode --chain-id side-testnet-1
sided config chain-id side-testnet-1
$TIKER config keyring-backend test
```

### Create wallet
~~~
sided keys add wallet
~~~

### Download Genesis && addrbook
~~~
wget -O $HOME/.sidechain/config/genesis.json https://raw.githubusercontent.com/sideprotocol/testnet/main/shambhala/genesis.json 
wget -O $HOME/.sidechain/config/addrbook.json https://raw.githubusercontent.com/obajay/nodes-Guides/main/Projects/Side_Protocol/addrbook.json
~~~

### Create a service file
~~~
sudo tee /etc/systemd/system/sided.service > /dev/null <<EOF
[Unit]
Description=sided
After=network-online.target

[Service]
User=$USER
ExecStart=$(which sided) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
~~~
~~~
sudo systemctl daemon-reload
sudo systemctl enable sided
sudo systemctl restart sided && sudo journalctl -u sided -f -o cat
~~~

### Create validator
~~~
$TIKER tx staking create-validator --moniker "WellNode 🦁" --identity "1B7C4191E48B1F4C" --website="https://wellnode.net/" --commission-max-change-rate "1" --commission-max-rate "1" --commission-rate "0.05" --min-self-delegation "1" --pubkey  $($TIKER tendermint show-validator) --amount 1000000$TOKEN --from walletYarNik-1 --gas auto -y --fees=1750uside --chain-id side-testnet-1 ~~~


