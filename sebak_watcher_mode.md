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
