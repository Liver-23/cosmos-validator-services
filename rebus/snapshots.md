# Insctructions
## Stop the service and reset the data
```
sudo systemctl stop rebusd
cp $HOME/.rebusd/data/priv_validator_state.json $HOME/.rebusd/priv_validator_state.json.backup
rm -rf $HOME/.rebusd/data
```
## Download latest snapshot
```
curl -L http://142.44.143.93/snapshots/rebus/reb_1111-1_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.rebusd
mv $HOME/.rebusd/priv_validator_state.json.backup $HOME/.rebusd/data/priv_validator_state.json
```
## Restart the service and check the log
```
sudo systemctl start rebusd && sudo journalctl -u rebusd -fn 100 -o cat
```
