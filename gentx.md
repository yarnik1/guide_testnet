# Generate Cosmos gentx

## Setting up vars
Here you have to put name of your moniker (validator) that will be visible in explorer
```
NODENAME=<YOUR_MONIKER_NAME_GOES_HERE>
```

Save and import variables into system
```
echo "export NODENAME=$NODENAME" >> $HOME/.bash_profile
echo "export WALLET=wallet" >> $HOME/.bash_profile
echo "export CHAIN_ID=jagrat" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

## Update packages
```
sudo apt update && sudo apt upgrade -y
```

## Install dependencies
```
sudo apt-get install make build-essential gcc git jq chrony -y
```

## Install go
```
ver="1.18.2"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> ~/.bash_profile
source ~/.bash_profile
```

## Download and install binaries
```
cd $HOME
git clone https://github.com/hypersign-protocol/hid-node.git
cd hid-node
make install
```

## Init node
```
hid-noded init $NODENAME --chain-id $CHAIN_ID
```

## Config app
```
hid-noded config chain-id $CHAIN_ID
hid-noded config keyring-backend test
```

## Recover or create new wallet for testnet
Option 1 - generate new wallet
```
hid-noded keys add $WALLET
```

Option 2 - recover existing wallet
```
hid-noded keys add $WALLET --recover
```

## Add genesis account
```
WALLET_ADDRESS=$(hid-noded keys show $WALLET -a)
hid-noded add-genesis-account $WALLET_ADDRESS 100000000000uhid
```

## Generate gentx
```
hid-noded gentx $WALLET 100000000000uhid \
--chain-id $CHAIN_ID \
--moniker=$NODENAME \
--commission-max-change-rate=0.01 \
--commission-max-rate=1.0 \
--commission-rate=0.05 \
--min-self-delegation=100000000000
```

## Things you have to backup
- `24 word mnemonic` of your generated wallet
- contents of `$HOME/.hid-node/config/*`

## Submit PR with Gentx
1. Copy the contents of ${HOME}/.hid-node/config/gentx/gentx-XXXXXXXX.json.
2. Fork https://github.com/hypersign-protocol/networks
3. Create a file `gentx-<VALIDATOR_NAME>.json` under the `testnet/jagrat/gentxs/` folder in the forked repo, paste the copied text into the file.
4. Create a Pull Request to the main branch of the repository

### Await further instructions!
