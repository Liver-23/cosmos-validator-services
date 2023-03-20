# Insctructions
## Stop the service and reset the data
```
sudo systemctl stop defundd
cp $HOME/.defund/data/priv_validator_state.json $HOME/.defund/priv_validator_state.json.backup
rm -rf $HOME/.defund/data
```
## Download latest snapshot
```
curl -L http://142.44.143.93/snapshots/defund/orbit-alpha-1_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.defund
mv $HOME/.defund/priv_validator_state.json.backup $HOME/.defund/data/priv_validator_state.json
```
## Restart the service and check the log
```
sudo systemctl start defundd && sudo journalctl -u defundd -fn 100 -o cat
```
