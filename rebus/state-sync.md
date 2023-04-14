# Insctructions
## Stop the service and reset the data
```
sudo systemctl stop rebusd
cp $HOME/.rebusd/data/priv_validator_state.json $HOME/.rebusd/priv_validator_state.json.backup
rebusd tendermint unsafe-reset-all --home $HOME/.rebusd --keep-addr-book
```
## Get and configure the state sync information
```
STATE_SYNC_RPC=http://23.88.5.169:19657
STATE_SYNC_PEER=eae6728de0c1ed97aa93c01197da6371877bc197@23.88.5.169:19656
LATEST_HEIGHT=$(curl -s $STATE_SYNC_RPC/block | jq -r .result.block.header.height)
SYNC_BLOCK_HEIGHT=$(($LATEST_HEIGHT - 500))
SYNC_BLOCK_HASH=$(curl -s "$STATE_SYNC_RPC/block?height=$SYNC_BLOCK_HEIGHT" | jq -r .result.block_id.hash)

echo $LATEST_HEIGHT $SYNC_BLOCK_HEIGHT $SYNC_BLOCK_HASH

sed -i \
  -e "s|^enable *=.*|enable = true|" \
  -e "s|^rpc_servers *=.*|rpc_servers = \"$STATE_SYNC_RPC,$STATE_SYNC_RPC\"|" \
  -e "s|^trust_height *=.*|trust_height = $SYNC_BLOCK_HEIGHT|" \
  -e "s|^trust_hash *=.*|trust_hash = \"$SYNC_BLOCK_HASH\"|" \
  -e "s|^persistent_peers *=.*|persistent_peers = \"$STATE_SYNC_PEER\"|" \
  $HOME/.rebusd/config/config.toml

mkdir -p $HOME/.rebusd/data && mv $HOME/.rebusd/priv_validator_state.json.backup $HOME/.rebusd/data/priv_validator_state.json
```
## Restart the service and check the log
```
sudo systemctl start rebusd && sudo journalctl -u rebusd -fn 100 -o cat
```
