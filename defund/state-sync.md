# Insctructions
## Stop the service and reset the data
```
sudo systemctl stop defundd
cp $HOME/.defund/data/priv_validator_state.json $HOME/.defund/priv_validator_state.json.backup
rm -rf $HOME/.defund/data
```
## Get and configure the state sync information
```
STATE_SYNC_RPC=http://142.44.143.93:24657
STATE_SYNC_PEER=b8f0bee92d7b87ec4b9abf15888fefb6d2e07092@142.44.143.93:24656
LATEST_HEIGHT=$(curl -s $STATE_SYNC_RPC/block | jq -r .result.block.header.height)
SYNC_BLOCK_HEIGHT=$(($LATEST_HEIGHT - 1000))
SYNC_BLOCK_HASH=$(curl -s "$STATE_SYNC_RPC/block?height=$SYNC_BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i \
  -e "s|^enable *=.*|enable = true|" \
  -e "s|^rpc_servers *=.*|rpc_servers = \"$STATE_SYNC_RPC,$STATE_SYNC_RPC\"|" \
  -e "s|^trust_height *=.*|trust_height = $SYNC_BLOCK_HEIGHT|" \
  -e "s|^trust_hash *=.*|trust_hash = \"$SYNC_BLOCK_HASH\"|" \
  -e "s|^persistent_peers *=.*|persistent_peers = \"$STATE_SYNC_PEER\"|" \
  $HOME/.defund/config/config.toml

mv $HOME/.defund/priv_validator_state.json.backup $HOME/.defund/data/priv_validator_state.json
```
## Restart the service and check the log
```
sudo systemctl start defundd && sudo journalctl -u defundd -fn 100 -o cat
```