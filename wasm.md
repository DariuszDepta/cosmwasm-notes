# Creating single-node blockchain locally

Original instructions are in file wasmd/contrib/local/README.md.

Below are the ones used on Linux, based on the general instructions.

Go to `wasmd` folder of cloned repository.
```
$ make install
$ sudo ln -s /home/confio/go/bin/wasmd /usr/local/bin/wasmd
$ cd contrib/local
$ rm -rf /tmp/trash
$ HOME=/tmp/trash bash setup_wasmd.sh
$ HOME=/tmp/trash bash start_node.sh
```

WORKS!

```
$ wasmd query wasm list-code

code_infos: []
pagination:
  next_key: null
  total: "0"
```

Error messages reported by `wasmd query wasm code-info`:

**code id: invalid**

```
$ wasmd query wasm code-info 0

Error: rpc error: code = Unknown desc = 
github.com/cosmos/cosmos-sdk/baseapp.gRPCErrorToSDKError
	github.com/cosmos/cosmos-sdk@v0.47.5/baseapp/abci.go:720
[...]
net/http.(*conn).serve
	net/http/server.go:2009
code id: invalid: unknown request
```

**code id 100: no such code**
```
$ wasmd query wasm code-info 100

Error: rpc error: code = Unknown desc = 
github.com/cosmos/cosmos-sdk/baseapp.gRPCErrorToSDKError
	github.com/cosmos/cosmos-sdk@v0.47.5/baseapp/abci.go:720
[...]
net/http.(*conn).serve
	net/http/server.go:2009
code id 100: no such code: unknown request
```

```
$ wasmd query wasm code-info 18446744073709551615

Error: rpc error: code = Unknown desc = 
github.com/cosmos/cosmos-sdk/baseapp.gRPCErrorToSDKError
	github.com/cosmos/cosmos-sdk@v0.47.5/baseapp/abci.go:720
[...]
net/http.(*conn).serve
	net/http/server.go:2009
code id 18446744073709551615: no such code: unknown request
```

**input parsing errors**
```
$ wasmd query wasm code-info alfa
Error: strconv.ParseUint: parsing "alfa": invalid syntax

wasmd query wasm code-info 1.234
Error: strconv.ParseUint: parsing "1.234": invalid syntax

$ wasmd query wasm code-info 18446744073709551616
Error: strconv.ParseUint: parsing "18446744073709551616": value out of range 
```