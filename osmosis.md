Show wasm parameters
```
osmosisd query wasm params --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443"
```

List all code blobs
```
osmosisd query wasm list-code --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443"
```

Show code info of code blob with identifier 100
```
osmosisd query wasm code-info 100 --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443"
```

Show pinned code blobs
```
osmosisd query wasm pinned --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443"
```

Download code blob for contract with code id = 7 and save to file code12.wasm
```
osmosisd query wasm code 7 code12.wasm --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443"
```

List instances of contracts instantiated from code with identifier 100
```
osmosisd query wasm list-contract-by-code 100 --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443"
```

Query contract state
```
//wrong
osmosisd query wasm contract-state smart osmo1xwahguax578tvequeg70xn0ej78gn2ahugq92m7dx8hkklsyupmsqqh66x '{"all_tokens":{}}' --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443"

//correct
osmosisd query wasm contract-state smart osmo1xwahguax578tvequeg70xn0ej78gn2ahugq92m7dx8hkklsyupmsqqh66x '{"list_proposals":{}}' --chain-id=osmosis-1 --node="https://rpc.osmosis.zone:443" 
```