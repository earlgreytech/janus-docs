---
description: 'https://github.com/qtumproject/janus'
---

# Getting Started

## Requirements

Below is a list of the dependencies necessary to setup and run Janus:

* Go
* Docker
* Linux commands `make` and `curl` 

Janus can be run locally in MacOS, Linux, and Windows \(Through the use of WSL2\)

## Installation 🛠 

To install Janus locally, simply run the following commands:

```bash
$ go get github.com/qtumproject/janus/...
$ cd $GOPATH/src/github.com/qtumproject/janus
```

## Setting it everything up  ⚙ 

After following the installation steps above, you want to run the following commands at the project root level in order to start using Janus with Docker: 

```
$ make
$ make docker-dev
$ make quick-start
```

This will build the docker image for Janus, as well as spin up two containers:

* One named `janus` running on port 23889
* Another one named `qtum` running on port 3889

`make quick-start` will also fund the tests accounts with QTUM in order for you to start testing and developing locally. Janus maps the base58 Qtum addresses of the Qtum node \(`qtum` container\) to its unique hex address, here's an example from the accounts that come with Janus' local setup:

```text
- qUbxboqjBRp96j3La8D1RYkyqx5uQbJPoW (hex 0x7926223070547d2d15b2ef5e7383e541c338ffe9 )
- qLn9vqbr2Gx3TsVR9QyTVB5mrMoh4x43Uf (hex 0x2352be3db3177f0a07efbe6da5857615b8c9901d )
```

Additionally, if you need or want to make changes and or additions to Janus, but don't want to go through the hassle of rebuilding the container, you can run the following command at the project root level:

```text
$ make run-janus
```

Which will run the most current local version of Janus on port 23888, but without rebuilding the image or the local docker container.

Congratulations! you have successfully installed and set up Janus, now you are ready to start interacting with Qtum using the tools you are already familiar with. 



