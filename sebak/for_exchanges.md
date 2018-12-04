# Pre-required Instruction

Before access to Sebak public Testnet, BOScoin devteam strongly recommends to test in your local environment with [standalone mode](Running-Standalone-Mode) & [sebak-angelbot](https://github.com/spikeekips/sebak-angelbot).

In case of you have seen unexpected errors or have questions,

* Please submit error report or questions to [github issue](https://github.com/bosnet/sebak/issues/new) or [dev-support@blockchainos.org](dev-support@blockchainos.org).
* When submit to github issue, please set the label, '[Exchange](https://github.com/bosnet/sebak/labels/Exchange)'.
* In the error report, please include these things,

    * Your error results or questions
    * Where did you find errors, in [Testnet](https://testnet-sebak.blockchainos.org) or your own environment.
    * SEBAK version and commit id.
    * If you request something to SEBAK, the payload you sent.

## Sebak Installation

Please check [SEBAK installation](SEBAK-Installation-Guide).

## Running Sebak on localhost (a.k.a 'standalone mode')

Please check contents in [standalone mode](Running-Standalone-Mode).

## Installation sebak-angelbot, the account creation tool

You can check specific information in the project page, [sebak-angelbot](https://github.com/spikeekips/sebak-angelbot).

## Test account creation through 'sebak-angelbot' in localhost enviroment

### How to create account using 'sebak-angelbot'?

- Running sebak node as [standalone](Running-Standalone-Mode) mode.
- Execute sebak command `$ sebak key generate` as much as you want. this key pair will be used to create new account in sebak node. We recommend almost 100 keypair.
- Open text editor and put secret-keys line by line and save in `sources.txt` file.
- Running 'sebak-angelbot' in your localhost. In 'sebak-anglebot' command line options, we recommend `--secret-seed` to be secret seed of `genesis account`.
- From now you can create new accounts.
- You can check account existed or not, when you used `http(or https)://localhost:12345/api/v1/accounts/<one of public address you set in sources.txt>`

## Test API in localhost environment

You can check [Sebak API document contents](https://bosnet.github.io/sebak/api/).

### How to make transaction??
**required below data format & data**

* All data must be sent through `POST` method
* `Content-type` must be `application/json`
* The structure of transaction, you can check in [API doc for Post Transaction](https://bosnet.github.io/sebak/api/#trasactions-transactions-post).
* BOScoin devteam is still developing the official SDK for various language and environments. At this time, you can use these,

    - [sebakjs-util](https://github.com/bosnet/sebakjs-util): Javascript library
    - [sebakpy-util](https://github.com/spikeekips/sebakpy-util): Python library

# Instruction test on Public Testnet

**NOTE: SEBAK only supports HTTP/2.**

## Sebak public Testnet
You can access https://testnet-sebak.blockchainos.org . You can test sebak API, [API docs](https://bosnet.github.io/sebak/api/).

## 'sebak-angelbot' for testing.
You can access https://testnet-angelbot.blockchainos.org .

