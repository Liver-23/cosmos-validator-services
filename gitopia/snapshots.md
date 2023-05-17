# Insctructions
## Stop the service and reset the data
```
sudo systemctl stop gitopiad
cp $HOME/.gitopia/data/priv_validator_state.json $HOME/.gitopia/priv_validator_state.json.backup
rm -rf $HOME/.gitopia/data
```
## Download latest snapshot
```
curl -L http://snapshots.liveraven.net/snapshots/gitopia/gitopia_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.gitopia
mv $HOME/.gitopia/priv_validator_state.json.backup $HOME/.gitopia/data/priv_validator_state.json
```
## Restart the service and check the log
```
sudo systemctl start gitopiad && sudo journalctl -u gitopiad -fn 100 -o cat
```
