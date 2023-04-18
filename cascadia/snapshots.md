# Insctructions
## Stop the service and reset the data
```
sudo systemctl stop cascadiad
cp $HOME/.cascadiad/data/priv_validator_state.json $HOME/.cascadiad/priv_validator_state.json.backup
rm -rf $HOME/.cascadiad/data
```
## Download latest snapshot
```
curl -L http://142.44.143.93/snapshots/cascadia/cascadia_6102-1_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.cascadiad
mv $HOME/.cascadiad/priv_validator_state.json.backup $HOME/.cascadiad/data/priv_validator_state.json
```
## Restart the service and check the log
```
sudo systemctl start cascadiad && sudo journalctl -u cascadiad -fn 100 -o cat
```
