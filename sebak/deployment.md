To deploy the sebak network, the basic steps are mentioned below.

1. Compose network
1. Genesis block generation
1. Node deployment

If you deployed network already, you should skip generating genesis block .

You can run SEBAK in standalone mode, please check [Running Standalone Mode](deployment_standalone.md).

The `network-id` is `this-is-test-sebak-network`.

# Genesis Block generation

> Shortly, blockchain is the chains of block and genesis block is the first block of entire blocks.

Before generating genesis block, you must make the keypair for genesis block.

> Keypair of sebak has 2 important concepts. *secret seed* and *public address*. As name implies, *public address* will be used to represent your account. So you can send and receive BOScoin using the *public address*. The *secret seed* is such like private key. When you opened your account, you can use it. Secret seed never be shared with any other people, please keep it safe.

sebak can make keypair, *secret seed* and *public address*.
```sh
$ sebak key generate
       Secret Seed: SCN4NSV5SVHIZWUDJFT4Z5FFVHO3TFRTOIBQLHMNPAZJ37K5A2YFSCBM
    Public Address: GALQG5SCKCPXUG4ODPMFZJGZ6XBVJTLAJFR7OJKJOJVARA7M4H5SGSOG
```

For the Genesis block generation, 2 sets of key pairs(required), network id (required) and storage location(optional) are necessary.
First key pair which secret seed and public address meant to be genesis block, 2nd key pair meant to be common budget account.
Network id is the unique key phrase to distinguish its network.
Storage mention specific location for save data.
Details are looking below.

```sh
$ sebak genesis \
    --network-id "this-is-test-sebak-network" \
    --storage "file:///tmp/db-n0" \
     GCXOTSI6IXZNIEYWVJBPZV4VFK7IEQVHRPIC7TFTIN4FFXYK7BVUBCOU \
     GBLZVWCICHM4ZFCK2M5IRFOQLANTGT53GX2BEKOQ75FJ2TDK6OEQAF4U \

INFO[10-29|14:57:28] genesis block created ...
successfully created genesis block
```

> You can also check the usage of `sebak key generate` in [SEBAK Commands](command.md#sebak-genesis).

You should make genesis block in every nodes

# Node deployment

> You don"t need high performance machine for deploying sebak, but your nodes should be on the very fast network environment.

## Nodes and Keys

* Nodes

| node | endpoint | validators |
| -- | -- | -- |
| *n0* | https://localhost:12345 | *n1 n2* |
| *n1* | https://localhost:12346 | *n0 n2* |
| *n2* | https://localhost:12347 | *n0 n1* |

* Keys

You have to generate new keypairs for deploy nodes through `sebak key generate`.


## Running

To run sebak, you need SSL certificates for HTTP2 protocol.
```sh
$ sebak tls

INFO[10-31|16:55:03] Generate tls certificate and key files
```

> If you want to create self-signed SSL certificates, see [Generating a self-signed certificate using OpenSSL](https://www.ibm.com/support/knowledgecenter/en/SSWHYP_4.0.0/com.ibm.apimgmt.cmc.doc/task_apionprem_gernerate_self_signed_openSSL.html).

After successfully creating genesis block and SSL certificates, run *n0*:

```sh
$ sebak node \
    --network-id "this-is-test-sebak-network" \
    --bind "https://localhost:12345" \
    --tls-key "sebak.key" \
    --tls-cert "sebak.crt" \
    --storage "file:///tmp/db-n0" \
    --secret-seed SCN4NSV5SVHIZWUDJFT4Z5FFVHO3TFRTOIBQLHMNPAZJ37K5A2YFSCBM \
    --validator "GDPQ2LBYP3RL3O675H2N5IEYM6PRJNUA5QFMKXIHGTKEB5KS5T3KHFA2 GCZG7MBKRSS6MJVZOALYBJB5C223FSZ43MDTPX2O4UGQTCXTHWBDNUB6" \
    --discovery http://localhost:12346 \
    --log-level debug \

INFO[10-31|19:18:35] Starting Sebak                           module=main caller=run.go:401
DBUG[10-31|19:18:35] parsed flags:                            module=main
	network-id=this-is-test-sebak-network
	bind=https://localhost:12345
	publish=https://localhost:12345
	storage=file:///tmp/db-n0
	tls-cert=sebak.crt
	tls-key=sebak.key
	log-level=debug
	log-format=terminal
	log=
....
```

and *n1*
```sh
$ sebak node \
    --network-id "this-is-test-sebak-network" \
    --bind "https://localhost:12346" \
    --tls-key "sebak.key" \
    --tls-cert "sebak.crt" \
    --storage "file:///tmp/db-n1" \
    --secret-seed SBGJDQ2J4PIYU7JVGKIBLNF6X3DOEVW3I4W2T77M2B47X2MPSUNXZ7T7 \
    --validators "GBNUTWSM4FRSEULVMHZF7NFQWIBGEDF5X5OHXFOZJB6SH5MIEDEJEJ2F GCZG7MBKRSS6MJVZOALYBJB5C223FSZ43MDTPX2O4UGQTCXTHWBDNUB6" \
    --discovery http://localhost:12345 \
    --log-level debug \

INFO[10-31|19:18:35] Starting Sebak                           module=main caller=run.go:401
DBUG[10-31|19:18:35] parsed flags:                            module=main
	network-id=this-is-test-sebak-network
	bind=https://localhost:12346
	publish=https://localhost:12346
	storage=file:///tmp/db-n1
	tls-cert=sebak.crt
	tls-key=sebak.key
	log-level=debug
	log-format=terminal
	log=
....
```

and *n2*
```sh
$ sebak node \
    --network-id "this-is-test-sebak-network" \
    --bind "https://localhost:12347" \
    --tls-key "sebak.key" \
    --tls-cert "sebak.crt" \
    --storage "file:///tmp/db-n2" \
    --secret-seed SDQKKG2MBSAXVLUE5JFNM7MXQ7MV7WPRIEOS7U7KLWFDKYDKXTLSSRTC \
    --validators "GBNUTWSM4FRSEULVMHZF7NFQWIBGEDF5X5OHXFOZJB6SH5MIEDEJEJ2F GDPQ2LBYP3RL3O675H2N5IEYM6PRJNUA5QFMKXIHGTKEB5KS5T3KHFA2" \
    --discovery http://localhost:12345 \
    --log-level debug \

INFO[10-31|19:18:35] Starting Sebak                           module=main caller=run.go:401
DBUG[10-31|19:18:35] parsed flags:                            module=main
	network-id=this-is-test-sebak-network
	bind=https://localhost:12347
	publish=https://localhost:12347
	storage=file:///tmp/db-n2
	tls-cert=sebak.crt
	tls-key=sebak.key
	log-level=debug
	log-format=terminal
	log=
....
```

If nodes launched sucessful, each node will connect other nodes and will be ready to make consensus.

# Spinning Network using Docker

To spawn a simple network, we prepared to deploy using docker.

First build the docker image:
```sh
$ docker build . -t sebak
# The build process creates a rather large image, which is regenarated every time
# To clean up all orphaned images, you can run the following command
$ docker rmi -f $(docker images -f "dangling=true" -q)
```
Then you can spawn 3 nodes using the following commands:
```sh
$ docker run --net host --rm -it --env-file=docker/node1.env sebak
$ docker run --net host --rm -it --env-file=docker/node2.env sebak
$ docker run --net host --rm -it --env-file=docker/node3.env sebak
```
