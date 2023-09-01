```
junod q wasm params --chain-id=juno-1 --node="tcp://88.99.164.158:1067"
osmosisd q wasm params --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443"
```

```
junod q wasm list-code  --chain-id=juno-1 --node="tcp://88.99.164.158:1067"
osmosisd q wasm list-code  --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443"
```

```
junod q wasm  pinned --chain-id=juno-1 --node="tcp://88.99.164.158:1067"
osmosisd q wasm  pinned --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443"
```

```
osmosisd q wasm  code 7 code12  --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443"
```

```
osmosisd q wasm list-contract-by-code 100  --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443"
```

```
//wrong
osmosisd q wasm  contract-state smart osmo1xwahguax578tvequeg70xn0ej78gn2ahugq92m7dx8hkklsyupmsqqh66x '{"all_tokens":{}}' --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443"

//correct
osmosisd q wasm  contract-state smart osmo1xwahguax578tvequeg70xn0ej78gn2ahugq92m7dx8hkklsyupmsqqh66x '{"list_proposals":{}}' --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443" 
```