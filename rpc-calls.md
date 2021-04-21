---
description: >-
  A full list of all the ETH-RPC calls that are supported by Janus, as well as
  calls unique to Janus
---

# RPC Calls

## JSON-RPC Endpoint

Default endpoints for Janus:

* http://localhost:23889 - Docker endpoint
* http://localhost:23888 - Local endpoint for `make run-janus`

## JSON-RPC Support

|  | Supported |
| :--- | :--- |
| JSON-RPC 2.0 | ✅  |
| Batch requests | ❌  |
| HTTP | ✅  |
| IPC | ❌  |
| WS | ❌  |

## The default block parameter

Similar to the ETH RPC calls, the following 

methods have an extra default block parameter:

* eth\_getBalance
* eth\_getCode
* eth\_getTransactionCount
* eth\_getStorageAt
* eth\_call

When requests are made that act on the state of qtum, the last default block parameter determines the height of the block.

The following options are possible for the defaultBlock parameter:

* `HEX String` - an integer block number
* `String "earliest"` for the earliest/genesis block
* `String "latest"` - for the latest mined block
* `String "pending"` - for the pending state/transactions

## Methods not yet supported

* eth\_protocolVersion
* eth\_syncing
* eth\_coinbase

## Supported JSON-RPC methods

### ETH methods

* web3\_clientVersion
* web3\_sha3
* net\_version
* net\_listening
* net\_peerCount
* eth\_mining
* eth\_hashrate
* eth\_accounts
* eth\_blockNumber
* eth\_call
* eth\_chainId
* eth\_estimateGas
* eth\_gasPrice
* eth\_getBalance
* eth\_getBlockByHash
* eth\_getBlockByNumber
* eth\_getCode
* eth\_getCompilers
* eth\_getFilterChanges
* eth\_getFilterLogs
* eth\_getLogs
* eth\_getStorageAt
* eth\_getTransactionByBlockHashAndIndex
* eth\_getTransactionByBlockNumberAndIndex
* eth\_getTransactionByHash
* eth\_getTransactionCount
* eth\_getTransactionReceipt
* eth\_getUncleByBlockHashAndIndex
* eth\_newBlockFilter
* eth\_newFilter
* eth\_personal\_unlockAccount
* eth\_sendRawTransaction
* eth\_sendTransaction
* eth\_sign
* eth\_signTransaction
* eth\_uninstallFilter

### Janus \(Qtum\) methods

* qtum\_getUTXOs

## JSON RPC API Reference

### ETH Methods

### web3\_clientVersion

Returns the current client version

**Parameters `None`**

**Returns** 

* **`String`**- The current client version

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"web3_clientVersion","params":[],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "QTUM ETHTestRPC/ethereum-js"
}
```

### web3\_sha3

Returns Keccak-256 \(not the standardized SHA3-256\) of the given data

**Parameters**

*  **`DATA` -** the data to convert into a SHA3 hash

```text
params: [
  "0x68656c6c6f20776f726c64"
]
```

**Returns** 

* **`DATA`** - The SHA3 result of the given string

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"web3_sha3","params":["0x68656c6c6f20776f726c64"],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
}
```

### net\_version

Returns the current network id.

**Parameters**

*  **`NONE`**

**Returns** 

* `String` - current network id.
  * `81` Mainnet 
  * `113` Regtest

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"net_version","params":[],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "813"
}
```

### net\_listening

Returns `true` if client is actively listening for network connections

**Parameters**

*  **`NONE`**

**Returns** 

* `Boolean` - `true` when listening, `false` otherwise

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"net_listening","params":[],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": true
}
```

### net\_**peerCount**

Returns number of peers currently  connected to the client.

**Parameters**

*  **`NONE`**

**Returns** 

* `Quantity` - integer of the number of connected peers.

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0x2" //2
}
```

### eth\_mining

Returns `true` if the wallet is **staking qtum** or not

**Parameters**

*  **`NONE`**

**Returns** 

* `Boolean` - returns `true` if the wallet is staking, `false` otherwise.

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_mining","params":[],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": true //2
}
```

### eth\_hashrate

Returns the wallet's current staking difficulty

**Parameters**

*  **`NONE`**

**Returns** 

* `Difficulty` - hex representing the float64 value for the wallet's staking difficulty

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_mining","params":[],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": true //2
}
```



