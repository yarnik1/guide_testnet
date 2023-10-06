# Local snapshot solution

**Stop your node**

## Saving the snapshot files
```
CHAIN_ID=public-testnet-14.5d79b6958580
mkdir $HOME/snapshot
mv $HOME/.local/share/namada/$CHAIN_ID/db $HOME/snapshot
mv $HOME/.local/share/namada/$CHAIN_ID/cometbft/data $HOME/snapshot
```
  
## Re-join the network
```
cd $HOME/.local/share/namada
rm -rf $CHAIN_ID/ && rm $CHAIN_ID.toml && rm global-config.toml && cd
namadac utils join-network --chain-id $CHAIN_ID --genesis-validator <your-validator-alias>
```
Note: you don't need to use the *--genesis-validator* flag if you are not a pre-genesis validator.
  
**Start your node, wait 30-60s, and stop it**
  
## Applying the snapshot
```
rm -rf $HOME/.local/share/namada/$CHAIN_ID/db
rm -rf $HOME/.local/share/namada/$CHAIN_ID/cometbft/data
cd $HOME/snapshot
mv db/ $HOME/.local/share/namada/$CHAIN_ID
mv data/ $HOME/.local/share/namada/$CHAIN_ID/cometbft
```
  
**Start your node**
