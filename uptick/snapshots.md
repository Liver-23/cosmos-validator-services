# Insctructions
## Stop the service and reset the data
```
sudo systemctl stop uptickd
cp $HOME/.uptickd/data/priv_validator_state.json $HOME/.uptickd/priv_validator_state.json.backup
rm -rf $HOME/.uptickd/data
```
## Download latest snapshot
```
curl -L http://142.44.143.93/snapshots/uptick/uptick_117-1_latest.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.uptickd
mv $HOME/.uptickd/priv_validator_state.json.backup $HOME/.uptickd/data/priv_validator_state.json
```
## Restart the service and check the log
```
sudo systemctl start uptickd && sudo journalctl -u uptickd -fn 100 -o cat
```
