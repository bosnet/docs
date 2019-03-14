---
layout: post
permalink: /watcher-node-mode/
---
---
### Watcher node mode

There are several node types in Sebak network. One of these is Watcher, this will sync the entire data from validator nodes or other watcher nodes, so you can monitor all the operations in the network.

- Watcher nodes are configured to watch the activity from the network
- You can submit the transaction to the network using watcher


## Configuration

Watcher mode has almost the same settings as Sebak nodes,but to run a watcher, you need to add the following settings

```sh
$ sebak node
      ...
      --watch-interval string               watch interval (default "5s")
      --watcher-mode                        watcher mode
      --discovery list                      initial endpoint for discovery
      ...
```

- `--watcher-mode` :  Enable watcher mode, It means that this node doesn't join consensus network,just watches watcher's validators.
- `--watch-interval` (default: "5s") : It's a setting for how often the watcher check the status of watcher's validators.
- `--discovery` : Set the endpoint of running node. If you are trying to connect to TESTNET, you can set `--discovery https://testnet-sebak.blockchainos.org`. Without `--discovery` option, your watcher will not do anything :)


### Notes (v0.1.2)

To avoid validator's rate limit, the following settings are recommended in outside networks:

```
--sync-pool-size=3
--sync-check-interval=10s
```

### Basic Example

This is basic example of watcher node.

#### Environment Setting(for MainNet)
```sh
# -*- sh -*-
export SEBAK_SECRET_SEED=SBMMSJXY5ZFYCJU25QXW3WHAUFN5QZLF7LS34FDWIT4S6YEN5B6DNCCH
export SEBAK_PUBLIC_KEY=GBVGD37OGCFNJS3TEL5PANSQ6FMGAVOZBTWDIGUX2RS7TY2IMAIONRKX
export SEBAK_BIND=https://0.0.0.0:12345
export SEBAK_VALIDATORS=GAYRF72WHUB7IQYCV7C7A3CARH4OP2GCR4SKBFKQLALAMCSR67RRBTFD
export SEBAK_GENESIS_BLOCK=GCRQV5NLUZRCU4G5QFWF2GBY4RBTENAUEGVPEECTK6QLVRBGFHGV76CV
export SEBAK_COMMON=GCPQQIX2LRX2J63C7AHWDXEMNGMZR2UI2PRN5TCSOVMEMF7BAUADMKH5
export SEBAK_STORAGE=file:///tmp/sebak/watcher
export SEBAK_NETWORK_ID="sebak-network; 2018-11-26"
export SEBAK_RATE_LIMIT_NODE=0-s
export SEBAK_RATE_LIMIT_API=0-s
export SEBAK_LOG_LEVEL=info
export SEBAK_SYNC_POOL_SIZE=3
export SEBAK_SYNC_CHECK_INTERVAL=10s
export SEBAK_CONGRESS_ADDR=GDK4CSKXCXKJB5LOQLJO55WIUGMIRGYGRKJ5RV7FLLR2D3GVSPIE3S7B
export SEBAK_BALANCE=5000000000000000
export SEBAK_WATCHER_MODE=1
export SEBAK_DISCOVERY=https://mainnet-node-0.blockchainos.org:443
```

#### Environment Setting(for TestNet)
```sh
# -*- sh -*-
export SEBAK_SECRET_SEED=SBMMSJXY5ZFYCJU25QXW3WHAUFN5QZLF7LS34FDWIT4S6YEN5B6DNCCH
export SEBAK_PUBLIC_KEY=GBVGD37OGCFNJS3TEL5PANSQ6FMGAVOZBTWDIGUX2RS7TY2IMAIONRKX
export SEBAK_BIND=https://0.0.0.0:12345
export SEBAK_VALIDATORS=GDIRF4UWPACXPPI4GW7CMTACTCNDIKJEHZK44RITZB4TD3YUM6CCVNGJ
export SEBAK_GENESIS_BLOCK=GDIRF4UWPACXPPI4GW7CMTACTCNDIKJEHZK44RITZB4TD3YUM6CCVNGJ
export SEBAK_COMMON=GCTVCU764UPXKRJW5DS5AWF5ETCCIYPZTWXN5U5CLUNAAJH6D5NGEIIH
export SEBAK_STORAGE=file:///tmp/sebak/watcher
export SEBAK_NETWORK_ID="sebak-test-network"
export SEBAK_RATE_LIMIT_NODE=0-s
export SEBAK_RATE_LIMIT_API=0-s
export SEBAK_LOG_LEVEL=info
export SEBAK_SYNC_POOL_SIZE=3
export SEBAK_SYNC_CHECK_INTERVAL=10s
export SEBAK_CONGRESS_ADDR=GCT67AX6PSARM47MJYAEMAQTS4LQRJDJFSMBAEEKHVCDIP5VRC3P2RPC
export SEBAK_BALANCE=10000000000000000000
export SEBAK_WATCHER_MODE=1
export SEBAK_DISCOVERY=http://testnet-sebak-4n-0.blockchainos.org/
```

```
$ sebak node --genesis="${SEBAK_GENESIS_BLOCK},${SEBAK_COMMON},${SEBAK_BALANCE}"
```

This watcher node can be accessed through https://localhost:12345.
To change location of storage, you can modify the setting of `SEBAK_STORAGE`.
