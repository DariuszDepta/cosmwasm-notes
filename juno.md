Show wasm parameters
```
junod query wasm params --chain-id=juno-1 --node="tcp://88.99.164.158:1067"
```

List all code blobs
```
junod query wasm list-code --chain-id=juno-1 --node="tcp://88.99.164.158:1067"
```

Show code info of code blob with identifier 100
```
junod query wasm code-info 100 --chain-id=juno-1 --node="tcp://88.99.164.158:1067"
```

Show pinned code blobs
```
junod query wasm pinned --chain-id=juno-1 --node="tcp://88.99.164.158:1067"
```

List instances of contracts instantiated from code with identifier 1
```
junod query wasm list-contract-by-code 1 --chain-id=juno-1 --node="tcp://88.99.164.158:1067"
```