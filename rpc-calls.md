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
* eth\_chainId
* eth\_mining
* eth\_hashrate
* eth\_gasPrice
* eth\_accounts
* eth\_blockNumber
* eth\_getBalance
* eth\_getStorageAt
* eth\_getTransactionCount
* eth\_getCode
* eth\_sign
* eth\_signTransaction
* eth\_sendTransaction
* eth\_sendRawTransaction
* eth\_call
* eth\_estimateGas
* eth\_getBlockByHash
* eth\_getBlockByNumber
* eth\_getTransactionByHash
* eth\_getTransactionByBlockHashAndIndex
* eth\_getTransactionByBlockNumberAndIndex
* eth\_getTransactionReceipt
* eth\_getUncleByBlockHashAndIndex
* eth\_getCompilers
* eth\_newFilter
* eth\_newBlockFilter
* eth\_uninstallFilter
* eth\_getFilterChanges
* eth\_getFilterLogs
* eth\_getLogs

### Janus methods

* qtum\_getUTXOs

### RPC Errors

The JSON response for an RPC error have the following structure:

* `Code` an int for the code number
* `Message` a string with a message for the reason behind the error

For example:

```text
curl --header 'Content-Type: application/json' --data \
'{"jsonrpc":"3.0","method":"qtum_getUTXOs","params":,"id":10}' \
'http://localhost:23889'

// Calling with params not being set to an empty array casues an error
{
    "code": 100,
    "message": "json decoder issue: invalid character ',' looking for beginning of value"
}
```

## JSON RPC API Reference

## ETH Methods

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

Returns the wallet's current staking difficulty, it **does not** return a hash rate.

**Parameters**

*  **`NONE`**

**Returns** 

* `Difficulty` - hex representing the float64 value for the wallet's staking difficulty

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_hashrate","params":[],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0x3dffffe03fffc081" //1.99806
}
```

### eth\_gasPrice

Returns the QTUM amount per gas unit.

**Parameters**

*  **`NONE`**

**Returns** 

* `Quantity` - hex representing the QTUM amount per gas unit

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0x28" //Minimum gas price
}
```

### eth\_accounts

Returns a list of the hex addresses corresponding to the base58 QTUM addresses owned by the wallet

**Parameters**

*  **`NONE`**

**Returns** 

* `Array of DATA` - 20 Bytes - hex address representation of base58 address owned by wallet

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": [
    "0x7926223070547d2d15b2ef5e7383e541c338ffe9",
    "0x2352be3db3177f0a07efbe6da5857615b8c9901d",
    "0x69b004ac2b3993bf2fdf56b02746a1f57997420d",
    "0x8c647515f03daeefd09872d7530fa8d8450f069a",
    "0x2191744eb5ebeac90e523a817b77a83a0058003b",
    "0x88b0bf4b301c21f8a47be2188bad6467ad556dcf"
  ] 
}
```

### eth\_blockNumber

Returns the number of the most recent block

**Parameters**

*  **`NONE`**

**Returns** 

* `Quantity` - integer of the current block number the client is on

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0x4b7" // 1207
}
```

### eth\_getBalance

Returns the balance of the account of a given address

**NOTE: Balances are represented in satoshis, in other words:**

* `1 QTUM = 10 ^ 8 Satoshi`

**Parameters**

* **`Data`** 20 Bytes- address to check for balance
* `QUANTITY|TAG` - integer block number, or the string `"latest"` , `"earliest"` or `"pending"` , see the [default block parameter](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#the-default-block-parameter) for more info.\|

**NOTE: The QTUM RPC calls used in Janus for translation do not make use of the block parameter, so including it in this call won't affect the return value as it will always return the most current indexed balance.**

```text
params: [
   '0x7926223070547d2d15b2ef5e',
   'latest'
]
```

**Returns** 

* `QUANTITY` - uint64 encoded hex of the current balance of the address in satoshis

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_getBalance","params": ["0x7926223070547d2d15b2ef5e"],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0x2387adc5a6ae00" // 10000804600000000 satoshis or 10,000,804.6 QTUM
}
```

### eth\_getStorageAt

Returns the value from a storage position at a given address.

**Parameters**

* **`Data`** 20 bytes - address of the storage
* `QUANTITY` - integer of the position in the storage
* `QUANTITY|TAG` - integer block number, or the string `"latest"` , `"earliest"` or `"pending"` , see the [default block parameter](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#the-default-block-parameter) for more info.

**Returns** 

* `DATA` - the value at this storage position.

**Example**

Example taken from ETH RPC page \([https://eth.wiki/json-rpc/API](https://eth.wiki/json-rpc/API)\):

Calculating the correct position depends on the storage to retrieve. Consider the following contract deployed at `0x295a70b2de5e3953354a6a8344e616ed314d7251` by address `0x391694e7e0b0cce554cb130d723a9d27458f9298`.

```text
contract Storage {
    uint pos0;
    mapping(address => uint) pos1;

    function Storage() {
        pos0 = 1234;
        pos1[msg.sender] = 5678;
    }
}
```

Retrieving the value of pos0 is straight forward:

```text
$ curl --header 'Content-Type: application/json' --data \
  '{"jsonrpc":"2.0", "method": "eth_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x0", "latest"], "id": 1}'
  'http://localhost:23889'
  
{"jsonrpc":"2.0","id":1,"result":"0x00000000000000000000000000000000000000000000000000000000000004d2"}
```

Retrieving an element of the map is harder. The position of an element in the map is calculated with:

```text
keccack(LeftPad32(key, 0), LeftPad32(map position, 0))
```

This means to retrieve the storage on pos1\[“0x391694e7e0b0cce554cb130d723a9d27458f9298”\] we need to calculate the position with:

```text
keccak(decodeHex("000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"))
```

The web3 library can be used to make the calculation:

```text
> var key = "000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"
undefined
> web3.sha3(key, {"encoding": "hex"})
"0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9"
```

Now to fetch the storage:

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0", "method": "eth_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9", "latest"], "id": 1}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0x000000000000000000000000000000000000000000000000000000000000162e"
}
```

### eth\_getTransactionCount

Returns a default value of `0x1` 

**NOTE:**  This call is set to a default value because QTUM does not use a nonce to determine the transaction count.

**Parameters**

* **`Data`** 20 Bytes- address.
* `QUANTITY|TAG` - integer block number, or the string `"latest"` , `"earliest"` or `"pending"` , see the [default block parameter](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#the-default-block-parameter) for more info.

```text
params: [
   '0x7926223070547d2d15b2ef5e',
   'latest'
]
```

**Returns** 

* `QUANTITY` - will always return 0x1

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_getTransactionCount","params": ["0x7926223070547d2d15b2ef5e"],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0x1" //Default value
}
```

### eth\_getCode

Returns a code at a given address

**Parameters**

* **`Data`** 20 Bytes- address.
* `QUANTITY|TAG` - integer block number, or the string `"latest"` , `"earliest"` or `"pending"` , see the [default block parameter](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#the-default-block-parameter) for more info.

```text
params: [
   '0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b',
   '0x2'
]
```

**Returns** 

* `DATA` - the code from the given address

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_getCode","params": ["0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x2"],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056" 
}
```

### eth\_sign

The sign method calculates an Ethereum specific signature with: `sign(keccak256("\x19Ethereum Signed Message:\n" + len(message) + message)))`.

By adding a prefix to the message makes the calculated signature recognizable as an Ethereum specific signature. This prevents misuse where a malicious DApp can sign arbitrary data \(e.g. transaction\) and use the signature to impersonate the victim.

**Note** the address to sign with must be unlocked.

**Parameters**

* **`Data`** 20 Bytes- address.
* **`Data`** N Bytes - message to sign

```text
params: [
    "0x9b2055d370f73ec7d8a03e965129118dc8f5bf83", 
    "0xdeadbeaf"
]
```

**Returns** 

* `DATA` - Signature

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_sign","params": ["0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x2"],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b" 
}
```

### eth\_signTransaction

Signs a transaction that can be submitted to the network at a later time using eth\_sendRawTransaction

**Parameters**

1. `Object` - The transaction object
   * `from:` `DATA` 20 Bytes - The address the transaction is sent from.
   * `to:` `DATA` 20 Bytes - \(optional when creating a new contract\) The address the transaction is directed to
   * `gas:` `QUANTITY` \(optional, default: TBD\) Integer of the gas provided for the transaction execution. It will return unused gas.
   * `gasPrice:` `QUANTITY` \(optional, default: TBD\) Integer of the gasPrice used for each paid gas in QTUM
   * `value:` `QUANTITY` - \(optional\) Integer of the value sent with this transaction, in Wei.
   * `data:` `DATA` - The compiled code of a contract OR the hash of the invoked method signature and encoded parameters. For more details see [Ethereum ABI Specification](https://docs.soliditylang.org/en/v0.8.4/abi-spec.html).

**NOTE:** This call does not have the nonce option, given that nonces are not used in QTUM.

```text
"params": [{
        "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675",
        "from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
        "gas": "0x76c0",
        "gasPrice": "0x9184e72a000",
        "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
        "value": "0x9184e72a"
}]
```

**Returns** 

* `DATA` - Signature

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_signTransaction",
 "params": [{
        "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675",
        "from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
        "gas": "0x76c0",
        "gasPrice": "0x9184e72a000",
        "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
        "value": "0x9184e72a"
 }],
 "id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b" 
}
```

### eth\_sendTransaction

Creates a new message call transaction or contract creation, if the data field contains code.

**Parameters**

1. `Object` - The transaction object
   * `from:` `DATA` 20 Bytes - The address the transaction is sent from.
   * `to:` `DATA` 20 Bytes - \(optional when creating a new contract\) The address the transaction is directed to
   * `gas:` `QUANTITY` \(optional, default: TBD\) Integer of the gas provided for the transaction execution. It will return unused gas.
   * `gasPrice:` `QUANTITY` \(optional, default: TBD\) Integer of the gasPrice used for each paid gas in QTUM
   * `value:` `QUANTITY` - \(optional\) Integer of the value sent with this transaction, in Wei.
   * `data:` `DATA` - The compiled code of a contract OR the hash of the invoked method signature and encoded parameters. For more details see [Ethereum ABI Specification](https://docs.soliditylang.org/en/v0.8.4/abi-spec.html).

**NOTE:** This call does not have the nonce option, given that nonces are not used in QTUM.

```text
params: [{
  "from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
  "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
  "gas": "0x76c0", // 30400
  "gasPrice": "0x9184e72a000", // 10000000000000
  "value": "0x9184e72a", // 2441406250
  "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
}]
```

**Returns** 

* `DATA` - Signature

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_sendTransaction",
 "params": [{
  "from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
  "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
  "gas": "0x76c0", // 30400
  "gasPrice": "0x9184e72a000", // 10000000000000
  "value": "0x9184e72a", // 2441406250
  "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
}],
 "id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331" 
}
```

### eth\_sendRawTransaction

Creates a new message call transaction or a contract creation for signed transactions.

**Parameters**

1. `DATA` The signed transaction data

**NOTE:** This call does not have the nonce option, given that nonces are not used in QTUM.

```text
params: ["0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"]
```

**Returns** 

* `DATA` - 32 Bytes - the transaction hash, or the zero hash if the transaction is not yet available.

**NOTE:**  Use [eth\_getTransactionReceipt](https://eth.wiki/json-rpc/API#eth_gettransactionreceipt) to get the contract address, after the transaction was mined, when you created a contract.

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_sendRawTransaction",
 "params": ["0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"], "id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331" 
}
```

### eth\_call

Executes a new message call immediately without creating a transaction on the block chain.

**Parameters**

1. `Object` - The transaction object
   * `from:` `DATA` 20 Bytes - The address the transaction is sent from.
   * `to:` `DATA` 20 Bytes - \(optional when creating a new contract\) The address the transaction is directed to
   * `gas:` `QUANTITY` \(optional, default: TBD\) Integer of the gas provided for the transaction execution. It will return unused gas.
   * `gasPrice:` `QUANTITY` \(optional, default: TBD\) Integer of the gasPrice used for each paid gas in QTUM
   * `value:` `QUANTITY` - \(optional\) Integer of the value sent with this transaction, in Wei.
   * `data:` `DATA` - The compiled code of a contract OR the hash of the invoked method signature and encoded parameters. For more details see [Ethereum ABI Specification](https://docs.soliditylang.org/en/v0.8.4/abi-spec.html).
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](https://eth.wiki/json-rpc/API#the-default-block-parameter)

**NOTE:** This call does not have the nonce option, given that nonces are not used in QTUM.

**Returns** 

* `DATA` - the return value of executed contract

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_call","params": [{see above}],"id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "{ReturnValueInHex}" 
}
```

### eth\_estimateGas

Generates and returns an estimate of how much gas is necessary to allow the transaction to complete. The transaction will not be added to the blockchain. Note that the estimate may be significantly more than the amount of gas actually used by the transaction, for a variety of reasons including EVM mechanics and node performance.

**Parameters**

1. See [eth\_call parameters](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_call)

**Returns** 

* `QUANTITY` the amount of gas used 

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_estimateGas","params": [{seeabove}], "id":10}' \
  'http://localhost:23889'

// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": "0x5208" // 21000
}
```

### eth\_getBlockByHash

Returns information about a block by hash.

**Parameters**

1. `DATA` 32 Bytes - Hash of a block
2. `Boolean` If true it returns the full transaction objects, if `false` only the hashes of the transactions.

```text
params: [
   '0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae',
   false
]
```

**Returns** 

`Object` - A block object, or `null` when no block was found:

* `number`: `QUANTITY` - the block number. `null` when its pending block.
* `hash`: `DATA`, 32 Bytes - hash of the block. `null` when its pending block.
* `parentHash`: `DATA`, 32 Bytes - hash of the parent block.
* `nonce`: `DATA`, 8 Bytes - hash of the generated proof-of-work. `null` when its pending block.
* `sha3Uncles`: `DATA`, 32 Bytes - SHA3 of the uncles data in the block.
* `logsBloom`: `DATA`, 256 Bytes - the bloom filter for the logs of the block. `null` when its pending block.
* `transactionsRoot`: `DATA`, 32 Bytes - the root of the transaction trie of the block.
* `stateRoot`: `DATA`, 32 Bytes - the root of the final state trie of the block.
* `receiptsRoot`: `DATA`, 32 Bytes - the root of the receipts trie of the block.
* `miner`: `DATA`, 20 Bytes - the address of the beneficiary to whom the mining rewards were given.
* `difficulty`: `QUANTITY` - integer of the difficulty for this block.
* `totalDifficulty`: `QUANTITY` - integer of the total difficulty of the chain until this block.
* `extraData`: `DATA` - the “extra data” field of this block.
* `size`: `QUANTITY` - integer the size of this block in bytes.
* `gasLimit`: `QUANTITY` - the maximum gas allowed in this block.
* `gasUsed`: `QUANTITY` - the total used gas by all transactions in this block.
* `timestamp`: `QUANTITY` - the unix timestamp for when the block was collated.
* `transactions`: `Array` - Array of transaction objects, or 32 Bytes transaction hashes depending on the last given parameter.
* `uncles`: `Array` - Array of uncle hashes.

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_getBlockByHash","params": ["0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae", false], "id":10}' \
  'http://localhost:23889'

// Result
{
  "jsonrpc": "2.0",
  "id": 10,
  "result": {
    "difficulty": "0x4ea3f27bc",
    "extraData": "0x476574682f4c5649562f76312e302e302f6c696e75782f676f312e342e32",
    "gasLimit": "0x1388",
    "gasUsed": "0x0",
    "hash": "0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae",
    "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    "miner": "0xbb7b8287f3f0a933474a79eae42cbca977791171",
    "mixHash": "0x4fffe9ae21f1c9e15207b1f472d5bbdd68c9595d461666602f2be20daf5e7843",
    "nonce": "0x689056015818adbe",
    "number": "0x1b4",
    "parentHash": "0xe99e022112df268087ea7eafaf4790497fd21dbeeb6bd7a1721df161a6657a54",
    "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
    "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
    "size": "0x220",
    "stateRoot": "0xddc8b0234c2e0cad087c8b389aa7ef01f7d79b2570bccb77ce48648aa61c904d",
    "timestamp": "0x55ba467c",
    "totalDifficulty": "0x78ed983323d",
    "transactions": [
    ],
    "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
    "uncles": [
    ]
  }
```

### eth\_getTransactionByHash

Returns the information about a transaction requested by transaction hash.

**Parameters**

1. `DATA` 32 Bytes - hash of a transaction.

```text
params: [
   "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
]
```

**Returns** 

`Object` - A transaction object, or `null` when no transaction was found:

* `blockHash`: `DATA`, 32 Bytes - hash of the block where this transaction was in. `null` when its pending.
* `blockNumber`: `QUANTITY` - block number where this transaction was in. `null` when its pending.
* `from`: `DATA`, 20 Bytes - address of the sender.
* `gas`: `QUANTITY` - currently defaults to `0x`
* `gasPrice`: `QUANTITY` - gas price provided by the sender in Wei.
* `hash`: `DATA`, 32 Bytes - hash of the transaction.
* `input`: `DATA` - the data send along with the transaction.
* `nonce`: `QUANTITY` - the number of transactions made by the sender prior to this one.
* `to`: `DATA`, 20 Bytes - address of the receiver. `null` when its a contract creation transaction.
* `transactionIndex`: `QUANTITY` - integer of the transactions index position in the block. `null` when its pending.
* `value`: `QUANTITY` - value transferred in QTUM

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_getTransactionByHash","params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"], "id":10}' \
  'http://localhost:23889'
  
  // Result
{
  "jsonrpc":"2.0",
  "id": 10,
  "result":{
    "blockHash":"0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2",
    "blockNumber":"0x5daf3b", // 6139707
    "from":"0xa7d9ddbe1f17865597fbd27ec712455208b6b76d",
    "gas":"0xc350", // 50000
    "gasPrice":"0x4a817c800", // 20000000000
    "hash":"0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
    "input":"0x68656c6c6f21",
    "nonce":"0x15", // 21
    "to":"0xf02c1c8e6114b1dbe8937a39260b5b0a374432bb",
    "transactionIndex":"0x41", // 65
    "value":"0xf3dbb76162000", // Amount in QTUM
  }
}
```

### eth\_getTransactionByBlockHashAndIndex

Returns information about a transaction by block hash and transaction index position.

**Parameters**

1. `DATA` 32 Bytes - hash of a block.
2. `QUANTITY` integer of the transaction index position.

```text
params: [
   '0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331',
   '0x0' // 0
]
```

**Returns** 

See [eth\_getTransactionByHash](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_gettransactionbyhash)

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockHashAndIndex","params": ["0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331", "0x0"], "id":10}' \
 'http://localhost:23889'
```

Result see [eth\_getTransactionByHash](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_gettransactionbyhash)

### eth\_getTransactionBy**BlockNumberAndIndex**

Returns information about a transaction by block number and transaction index position.

**Parameters**

1. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`, see the [default block parameter](https://eth.wiki/json-rpc/API#the-default-block-parameter)
2. `QUANTITY` integer of the transaction index position.

```text
params: [
   '0x29c', // 668
   '0x0' // 0
]
```

**Returns** 

See [eth\_getTransactionByHash](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_gettransactionbyhash)

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockNumberAndIndex","params": ["0x29c", "0x0"], "id":10}' \
 'http://localhost:23889'
```

Result see [eth\_getTransactionByHash](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_gettransactionbyhash)

### eth\_getTransactionReceipt

Returns the receipt of a transaction by transaction hash.

**Parameters**

1. `DATA` 32 Bytes - hash of a transaction.

```text
params: [
   "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"
]
```

**Returns** 

`Object` - A transaction receipt object, or `null` when no transaction was found:

* `transactionHash`: `DATA`, 32 Bytes - hash of the transaction.
* `transactionIndex`: `QUANTITY`, integer of the transactions index position in the block
* `blockHash`: `DATA`, 32 Bytes - hash of the block where this transaction was in. `null` when its pending.
* `blockNumber`: `QUANTITY` - block number where this transaction was in. `null` when its pending.
* `from`: `DATA`, 20 Bytes - address of the sender.
* `to`: `DATA`, 20 Bytes - address of the receiver. `null` when its a contract creation transaction.
* `cumulativeGasUsed`: `QUANTITY` - The total amount of gas used when this transaction was executed in the block.
* `gasUsed`: `QUANTITY` - The amount of gas used by this specific transaction alone
* `contractAddress:` `DATA` - 20 Bytes - The contract address created, if the transaction was a contract creation, otherwise `null`
* `logs:` `Array` - Array of log objects, which this transaction generated.
* `logsBloom:` `DATA` 256 Bytes - Bloom filter for light clients to quickly retrieve related logs. **NOTE:** Currently set to  always return `0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000`

It also returns either:

* `status`: `QUANTITY` either `1` \(success\) or `0` \(failure\)

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_getTransactionReceipt","params": ["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"], "id":10}' \
  'http://localhost:23889'
  
  // Result
{
  "jsonrpc":"2.0",
  "id": 10,
  "result": {
     transactionHash: '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238',
     transactionIndex:  '0x1', // 1
     blockNumber: '0xb', // 11
     blockHash: '0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b',
     cumulativeGasUsed: '0x33bc', // 13244
     gasUsed: '0x4dc', // 1244
     contractAddress: '0xb60e8dd61c5d32be8058bb8eb970870f07233155', // or null, if none was created
     logs: [{
         // logs as returned by getFilterLogs, etc.
     }, ...],
     logsBloom: "0x00...0", // 256 byte bloom filter
     status: '0x1'
  }
}
```

### eth\_getUnlceByBlockHashAndIndex

Returns information about an uncle of a block by hash and uncle index position

**Parameters**

1. `DATA` 32 Bytes - Hash of a block
2. `QUANTITY` The uncle's index position

```text
params: [
   '0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b',
   '0x0' // 0
]
```

**Returns** 

See [eth\_getBlockByHash](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_getblockbyhash)

**Note:** An uncle doesn't contain individual transactions.

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_getUncleByBlockHashAndIndex","params": ["0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b", "0x0"], "id":10}' \
  'http://localhost:23889'
```

Result see [eth\_getBlockByHash](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_getblockbyhash)

### eth\_getCompilers

Returns a list of available compilers in the client

**Parameters `None`**

**Returns** 

`Array` Array of available compilers. \(Hardcoded to an empty array\)

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_getCompilers","params": [], "id":10}' \
  'http://localhost:23889'
  
// Result
{
  "id":10,
  "jsonrpc": "2.0",
  "result": []
}
```

### eth\_newFilter

 Creates a filter object, based on filter options, to notify when the state changes \(logs\).  
To check if the state has changed, call [eth\_getFilterChanges.](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_getfilterchanges)

**A note on specifying topic filters:**

Topics are order-dependent. A transaction with a log with topics \[A, B\] will be matched by the following topic filters:

* `[]` “anything”
* `[A]` “A in first position \(and anything after\)”
* `[null, B]` “anything in first position AND B in second position \(and anything after\)”
* `[A, B]` “A in first position AND B in second position \(and anything after\)”
* `[[A, B], [A, B]]` “\(A OR B\) in first position AND \(A OR B\) in second position \(and anything after\)”

**Parameters** 

1. `Object` - The filter options:

* `fromBlock`: `QUANTITY|TAG` - \(optional, default: `"latest"`\) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
* `toBlock`: `QUANTITY|TAG` - \(optional, default: `"latest"`\) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
* `address`: `DATA|Array`, 20 Bytes - \(optional\) Contract address or a list of addresses from which logs should originate.
* `topics`: `Array of DATA`, - \(optional\) Array of 32 Bytes `DATA` topics. Topics are order-dependent. Each topic can also be an array of DATA with “or” options.

```text
params: [{
  "fromBlock": "0x1",
  "toBlock": "0x2",
  "address": "0x8888f1f195afa192cfee860698584c030f4c9db1",
  "topics": ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b", null, ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x0000000000000000000000000aff3454fce5edbc8cca8697c15331677e6ebccc"]]
}]
```

**Returns** 

`QUANTITY` A filter id

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_newFilter","params":[{"topics":["0x12341234"]}], "id":10}' \
  'http://localhost:23889'
  
// Result
{
  "id": 10,
  "jsonrpc": "2.0",
  "result": "0x1" // filter id for "topics":["0x12341234"] is 1
}
```

### eth\_newBlockFilter

Creates a filter in the node, to notify when a new block arrives. To check if the state has changed, call [eth\_getFilterChanges.](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_getfilterchanges)

**Parameters `None`**

**Returns** 

`QUANTITY` A filter id

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_newBockFilter","params":[], "id":10}' \
  'http://localhost:23889'
  
// Result
{
  "id": 10,
  "jsonrpc": "2.0",
  "result": "0x1" // block_filter id is 1
}
```

### eth\_uninstallFilter

Uninstalls a filter with given id. Should always be called when watch is no longer needed. Additonally Filters timeout when they aren’t requested with [eth\_getFilterChanges](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_getfilterchanges) for a period of time

**Parameters** 

* `QUANTITY` The filter id

```text
params: [
  "0xb" // 11
]
```

**Returns** 

`Boolean` `true` if the filter was successfully uninstalled, otherwise `false`

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_uninstallFilter","params":["0xb"], "id":10}' \
  'http://localhost:23889'
  
// Result
{
  "id": 10,
  "jsonrpc": "2.0",
  "result": true
}
```

### eth\_getFilterChanges

Polling method for a filter, which returns an array of logs which occurred since last poll.

**Parameters** 

* `QUANTITY` The filter id

```text
params: [
  "0x16" // 22
]
```

**Returns** 

`Array` - Array of log objects, or an empty array if nothing has changed since last poll.

* For filters created with `eth_newBlockFilter` the return are block hashes \(`DATA`, 32 Bytes\), e.g. `["0x3454645634534..."]`.
* For filters created with `eth_newFilter` logs are objects with following params:
  * `removed`: `TAG` - `true` when the log was removed, due to a chain reorganization. `false` if its a valid log.
  * `logIndex`: `QUANTITY` - integer of the log index position in the block. `null` when its pending log.
  * `transactionIndex`: `QUANTITY` - integer of the transactions index position log was created from. `null` when its pending log.
  * `transactionHash`: `DATA`, 32 Bytes - hash of the transactions this log was created from. `null` when its pending log.
  * `blockHash`: `DATA`, 32 Bytes - hash of the block where this log was in. `null` when its pending. `null` when its pending log.
  * `blockNumber`: `QUANTITY` - the block number where this log was in. `null` when its pending. `null` when its pending log.
  * `address`: `DATA`, 20 Bytes - address from which this log originated.
  * `data`: `DATA` - contains one or more 32 Bytes non-indexed arguments of the log.
  * `topics`: `Array of DATA` - Array of 0 to 4 32 Bytes `DATA` of indexed log arguments. \(In _solidity_: The first topic is the _hash_ of the signature of the event \(e.g. `Deposit(address,bytes32,uint256)`\), except you declared the event with the `anonymous` specifier.\)

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_uninstallFilter","params":["0x16"], "id":10}' \
  'http://localhost:23889'
  
// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": [{
    "logIndex": "0x1", // 1
    "blockNumber":"0x1b4", // 436
    "blockHash": "0x8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "transactionHash":  "0xdf829c5a142f1fccd7d8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcf",
    "transactionIndex": "0x0", // 0
    "address": "0x16c5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "data":"0x0000000000000000000000000000000000000000000000000000000000000000",
    "topics": ["0x59ebeb90bc63057b6515673c3ecf9438e5058bca0f92585014eced636878c9a5"]
    },{
      ...
    }]
}
```

### eth\_getFilterLogs

Returns an array of all logs matching filter with given id.

**Parameters** 

* `QUANTITY` The filter id

```text
params: [
  "0x16" // 22
]
```

**Returns** 

See [eth\_getFilterChanges](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_getfilterchanges)

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_getFilterLogs","params":["0x16"], "id":10}' \
  'http://localhost:23889'
```

Result see [eth\_getFilterChanges](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_getfilterchanges)

### eth\_getLogs

Returns an array of all logs matching a given filter object.

**Parameters** 

1. `Object` - The filter options:

* `fromBlock`: `QUANTITY|TAG` - \(optional, default: `"latest"`\) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
* `toBlock`: `QUANTITY|TAG` - \(optional, default: `"latest"`\) Integer block number, or `"latest"` for the last mined block or `"pending"`, `"earliest"` for not yet mined transactions.
* `address`: `DATA|Array`, 20 Bytes - \(optional\) Contract address or a list of addresses from which logs should originate.
* `topics`: `Array of DATA`, - \(optional\) Array of 32 Bytes `DATA` topics. Topics are order-dependent. Each topic can also be an array of DATA with “or” options.
* `blockhash`: `DATA`, 32 Bytes - \(optional\) Using `blockHash` is equivalent to `fromBlock` = `toBlock` = the block number with hash `blockHash`. If `blockHash` is present in in the filter criteria, then neither `fromBlock` nor `toBlock` are allowed.

```text
params: [{
  "topics": ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b"]
}]
```

**Returns** 

See [eth\_getFilterChanges](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_getfilterchanges)

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_uninstallFilter","params":[{"topics":["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b"]}], "id":10}' \
  'http://localhost:23889'
```

Return see See [eth\_getFilterChanges](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_getfilterchanges)

## Janus methods

### qtum\_getUTXOs

Returns an array containing a set of UTXOs given an address \(must be an unlocked addres\) for which the the total amount \(the sum of the amount of each individual UTXO\) is greater than a specified **minimum amount**

**NOTE:** This call will only work if the address used is part of the wallet used to make the call

**Parameters** 

1. `DATA` - 20 Bytes - hex address representation of base58 address owned by wallet
2. `QUANTITY` float value for **minimum amount** in QTUM, if the minimum amount is greater than the sum of all available UTXOs the call will return with an error message **"required minimum amount is greater than total amount of UTXOs"**

```text
params: [
 "0x7926223070547d2d15b2ef5e7383e541c338ffe9",
 10 
]
```

**Returns** 

`Array` Array of JSON objects representing the gathered UTXOs

```text
[                
  {
    "txid" : "txid",          (string) the transaction id 
    "vout" : n,               (numeric) the vout value
    "address" : "address",    (string) the qtum address
    "label" : "label",        (string) The associated label, or "" for the default label
    "scriptPubKey" : "key",   (string) the script key
    "amount" : x.xxx,         (numeric) the transaction output amount in QTUM
    "confirmations" : n,      (numeric) The number of confirmations
    "redeemScript" : n        (string) The redeemScript if scriptPubKey is P2SH
    "spendable" : xxx,        (bool) Whether we have the private keys to spend this output
    "solvable" : xxx,         (bool) Whether we know how to spend this output, ignoring the lack of keys
    "safe" : xxx              (bool) Whether this output is considered safe to spend. Unconfirmed transactions
                              from outside keys and unconfirmed replacement transactions are considered unsafe
                              and are not eligible for spending by fundrawtransaction and sendtoaddress.
  }
  ,...
]
```

**Example**

```http
$ curl --header 'Content-Type: application/json' --data \
 '{"jsonrpc":"2.0","method":"eth_uninstallFilter","params":["0x7926223070547d2d15b2ef5e7383e541c338ffe9", 1], "id":10}' \
  'http://localhost:23889'
  
// Result
{
  "id":10,
  "jsonrpc":"2.0",
  "result": [{
    "address":"qUbxboqjBRp96j3La8D1RYkyqx5uQbJPoW",
    "txid":"a54963eadaf88f0e30340c3649f426181f5c9aff3580b6a44923a1e796e3d7ff",
    "vout":0,
    "amount":"20000",
    "safe":true,
    "spendable":true,
    "solvable":true,
    "label":"",
    "confirmations":50696,
    "scriptPubKey":"76a9147926223070547d2d15b2ef5e7383e541c338ffe988ac",
    "redeemScript":""},
    {
      ...
    },
    ...
    ]
}
```

Return see See [eth\_getFilterChanges](https://app.gitbook.com/@earl-grey-tech/s/janus-docs/~/drafts/-MYu8ax0AMrsdcYZw6US/rpc-calls#eth_getfilterchanges)



