## und query ibc-transfer denom-hash

Query the denom hash info from a given denom trace

### Synopsis

Query the denom hash info from a given denom trace

```
und query ibc-transfer denom-hash [trace] [flags]
```

### Examples

```
und query ibc-transfer denom-hash transfer/channel-0/uatom
```

### Options

```
      --height int      Use a specific height to query state at (this can error if the node is pruning state)
  -h, --help            help for denom-hash
      --node string     <host>:<port> to Tendermint RPC interface for this chain (default "tcp://localhost:26657")
  -o, --output string   Output format (text|json) (default "text")
```

### Options inherited from parent commands

```
      --chain-id string   The network chain ID
```

### SEE ALSO

* [und query ibc-transfer](und_query_ibc-transfer.md)	 - IBC fungible token transfer query subcommands

###### Auto generated by spf13/cobra on 8-Jul-2022