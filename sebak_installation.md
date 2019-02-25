---
layout: post
permalink: /SEBAK-installation/
---
---
# SEAK installation

## Prerequisite
Before installing, you must install Go 1.11 or above.

To start sebak:

```sh
$ # cd to any folder you see fit, and you might want to set GOPATH
$ git clone https://github.com/bosnet/sebak.git
$ cd sebak
$ go install ./...
$ sebak <command>
```

## Test

You can test sebak. see below.

```sh
$ go test ./...
```

You can see the detailed logs;
```sh
$ go test ./... -v
```

