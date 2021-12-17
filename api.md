# JSON-RPC API

Clients interact with Zenon Alphanet through `JSON-RPC` API calls. In the examples provided below, API calls are performed to a Zenon full node listening for websocket traffic on `127.0.0.1:35998` (IPv4) or `[::1]:35998` (IPv6). Follow the links below or use the navigation on the left to learn more about the available options.

## Configuration

In order to be able to enable the communication method with the Zenon full node, the `config.json` needs to be configured accordingly:

* For inter-process communication: `"EnableIPC":true`, default IPC port is `35996`
* For HTTP: `"EnableHTTP":true`, default HTTP port is `35997`
* For Websocket: `"EnableWS":true`, default Websocket port is `35998`

Learn more about configuring Nodes [Enable RPC communication for the Node daemon](quickstart.md#5)

## Endpoints & Permissions

In order to be able to perform calls to the respective `Endpoints`, the `config.json` needs to be configured with the following entries:

* [embedded](#Embedded): allows clients to interact with the NoM embedded smart contracts
* [ledger](#Ledger): allows clients to interact with the NoM dual-ledger
* [stats](#Stats): allows clients to examine stats and other information about the Node
* [wallet](#Wallet): allows clients to manage wallets through the Node

The bash command `znn-cli enableRPC` can be used; it will automatically populate the `config.json` with the necessary information

Learn more about configuring Nodes [Enable RPC communication for the Node daemon](quickstart.md#5)

## Embedded Smart Contracts

* [embedded.pillar](#embedded.pillar)
* [embedded.plasma](#embedded.plasma)
* [embedded.sentinel](#embedded.sentinel)
* [embedded.token](#embedded.token)
* [embedded.swap](#embedded.swap)
* [embedded.stake](#embedded.stake)

## embedded.pillar

* [embedded.pillar.getQsrRegistrationCost](#embedded.pillar.getQsrRegistrationCost)
* [embedded.pillar.checkNameAvailability](#embedded.pillar.checkNameAvailability)
* [embedded.pillar.getAll](#embedded.pillar.getAll)
* [embedded.pillar.getByOwner](#embedded.pillar.getByOwner)
* [embedded.pillar.getByName](#embedded.pillar.getByName)
* [embedded.pillar.getDelegatedPillar](#embedded.pillar.getDelegatedPillar)
* [embedded.pillar.getDepositedQsr](#embedded.pillar.getDepositedQsr)
* [embedded.pillar.getUncollectedReward](#embedded.pillar.getUncollectedReward)
* [embedded.pillar.getFrontierRewardByPage](#embedded.pillar.getFrontierRewardByPage)

### embedded.pillar.getQsrRegistrationCost

> This API call will return the current QSR cost for registering a new Pillar.

#### Request

No parameters

```json
{
    "jsonrpc": "2.0",
    "id": 2,
    "method": "embedded.pillar.getQsrRegistrationCost",
    "params": []
}
```

#### Response

`number` - QSR cost (without decimals)

```json
{
    "jsonrpc": "2.0",
    "id": 2,
    "result": 15000000000000
}
```

### embedded.pillar.checkNameAvailability

> This API call will return information about the availability of a name for a Pillar.

#### Request

One parameter of type `string` that represents the `name`.

```json
{
    "jsonrpc": "2.0",
    "id": 9,
    "method": "embedded.pillar.checkNameAvailability",
    "params": ["Pillar_123"]
}
```

#### Response

* a parameter of type `bool`, which is true if the name is available and false otherwise

```json
{
    "jsonrpc": "2.0",
    "id": 9,
    "result": true
}
```

### embedded.pillar.getAll

> This API call will return the list of Pillars in the network with additional information.

#### Request

* first parameter of type `number` that represents the page index
* second parameter of type `number` that represents the number of entries to be displayed on this page

```json
{
    "jsonrpc": "2.0",
    "id": 3,
    "method": "embedded.pillar.getAll",
    "params": [0, 5]
}
```

#### Response

An `array` of `entries` and each item in the list contains the following fields:

* `name` of type `string`: name of the registered Pillar

* `rank` of type `int`: index of pillar sorted by weight

* `type` of type `int`: type of the registered pillar

* `isRevocable` of type `bool`: `true` if the Pillar can be revoked, `false` otherwise

* `ownerAddress` of type `string`: owner address the registered Pillar

* `producerAddress` of type `string`: address used by the Pillar to produce momentums

* `withdrawAddress` of type `string`: address used to collect the rewards

* `giveMomentumRewardPercentage` of type `int` : percentage of momentum rewards distributed to delegators

* `giveDelegateRewardPercentage` of type `int` : percentage of delegation rewards distributed to delegators

* `isRevocable` of type `bool` : specifies if the pillar is revocable

* `revokeCooldown` of type `number`: seconds remaining until revoke window opens

* `revokeTimestamp` of type `number`: UNIX timestamp that indicates when the Pillar was revoked

* `currentStats` - of type `dictionary` : block producing information
  * `producedMomentums` of type `number`: number of momentums produced during current epoch
  * `expectedMomentums` of type `number`: expected number of momentums to be produced during current epoch

* `weight` of type `number`: total amount of ZNN delegated to this Pillar

```json
{
    "jsonrpc": "2.0",
    "id": 3,
    "result": [
        {
            "name": "testname1",
            "rank": 0,
            "type": 1,
            "ownerAddress": "z1qz5p95pa8c6wq9pvfkg642gjv4nnaayx6vhm2w",
            "producerAddress": "z1qz5p95pa8c6wq9pvfkg642gjv4nnaayx6vhm2w",
            "withdrawAddress": "z1qz5p95pa8c6wq9pvfkg642gjv4nnaayx6vhm2w",
            "giveMomentumRewardPercentage": 0,
            "giveDelegateRewardPercentage": 100,
            "isRevocable": false,
            "revokeCooldown": 135840,
            "revokeTimestamp": 0,
            "currentStats": {
                "producedMomentums": 0,
                "expectedMomentums": 62
            },
            "weight": 350364710594425
        },
        {
            "name": "testname2",
            "rank": 1,
            "type": 1,
            "ownerAddress": "z1qz3f6svf805tewktk5yf9tn8cdhe2236wdnugk",
            "producerAddress": "z1qqna5fwl9cfd4h7xyg54qdg3nlxgjhntekdlw4",
            "withdrawAddress": "z1qz3f6svf805tewktk5yf9tn8cdhe2236wdnugk",
            "giveMomentumRewardPercentage": 0,
            "giveDelegateRewardPercentage": 100,
            "isRevocable": false,
            "revokeCooldown": 94740,
            "revokeTimestamp": 0,
            "currentStats": {
                "producedMomentums": 0,
                "expectedMomentums": 73
            },
            "weight": 237213658559729
        }
    ]
}
```

### embedded.pillar.getByOwner

> This API call will return all the Pillars registered by an address.

#### Request

One parameter of type `string` that represents the `ownerAddress` of the Pillars.

```json
{
    "jsonrpc": "2.0",
    "id": 4,
    "method": "embedded.pillar.getByOwner",
    "params": ["z1qqna5fwl9cfd4h7xyg54qdg3nlxgjhntekdlw4"]
}
```

#### Response

An `array` of `entries`

Same information as [embedded.pillar.getAll](#embedded.pillar.getAll)

```json
{
    "jsonrpc": "2.0",
    "id": 4,
    "result": [
        {
            "name": "testName",
            "rank": 14,
            "type": 1,
            "ownerAddress": "z1qqna5fwl9cfd4h7xyg54qdg3nlxgjhntekdlw4",
            "producerAddress": "z1qz3f6svf805tewktk5yf9tn8cdhe2236wdnugk",
            "withdrawAddress": "z1qqna5fwl9cfd4h7xyg54qdg3nlxgjhntekdlw4",
            "giveMomentumRewardPercentage": 5,
            "giveDelegateRewardPercentage": 80,
            "isRevocable": false,
            "revokeCooldown": 85320,
            "revokeTimestamp": 0,
            "currentStats": {
                "producedMomentums": 0,
                "expectedMomentums": 57
            },
            "weight": 25585096075852
        }
    ]
}
```

### embedded.pillar.getByName

> This API call will return information about the Pillar with the specified name.

#### Request

One parameter of type `string` that represents the name of the Pillar.

```json
{
    "jsonrpc": "2.0",
    "id": 5,
    "method": "embedded.pillar.getByName",
    "params": ["VPS_1"]
}
```

#### Response

`JSON` object representing the Pillar:

Same information as [embedded.pillar.getAll](#embedded.pillar.getAll)

```json
{
    "jsonrpc": "2.0",
    "id": 5,
    "result": {
        "name": "VPS_1",
        "rank": 31,
        "type": 1,
        "ownerAddress": "z1qqdtl63rkhap72nlaymtkemlchwv0ns9ksfjyn",
        "producerAddress": "z1qqdtl63rkhap72nlaymtkemlchwv0ns9ksfjyn",
        "withdrawAddress": "z1qqdtl63rkhap72nlaymtkemlchwv0ns9ksfjyn",
        "giveMomentumRewardPercentage": 10,
        "giveDelegateRewardPercentage": 90,
        "isRevocable": true,
        "revokeCooldown": 8720,
        "revokeTimestamp": 0,
        "currentStats": {
            "producedMomentums": 15,
            "expectedMomentums": 67
        },
        "weight": 32125334226305
    }
}
```

### embedded.pillar.getDelegatedPillar

> This API call will return the total number of delegations for a particular Pillar.

#### Request

One parameter of type `string` that represents the `ownerAddress` of the Pillar.

```json
{
    "jsonrpc": "2.0",
    "id": 8,
    "method": "embedded.pillar.getDelegatedPillar",
    "params": ["z1qqdtl63rkhap72nlaymtkemlchwv0ns9ksfjyn"]
}
```

#### Response

`JSON` object representing the delegation amount:

* `name` of  type `string`: name of the Pillar
* `status` of type `number`: status of the epoch
* `weight` of type `number`: total amount of ZNN delegated to this Pillar

```json
{
    "jsonrpc": "2.0",
    "id": 8,
    "result": {
        "name": "VPS_1",
        "status": 1,
        "weight": 9914999999998
    }
}
```

### embedded.pillar.getDepositedQsr

> This API call will return the amount of QSR deposited that can be used to create a Pillar.

#### Request

`1` parameter of type `string` that represents the address for querying the deposit.

```json
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "embedded.pillar.getDepositedQsr",
    "params": ["z1qqdtl63rkhap72nlaymtkemlchwv0ns9ksfjyn"]
}
```

#### Response

`number` - QSR amount deposited

```json
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": 100000000000
}
```

### embedded.pillar.getUncollectedReward

> This API call will return the uncollected reward for the specified pillar.

#### Request

One parameter of type `string` that represents the `ownerAddress` of the Pillar

```json
{
    "jsonrpc": "2.0",
    "id": 6,
    "method": "embedded.pillar.getUncollectedReward",
    "params": ["z1qqdtl63rkhap72nlaymtkemlchwv0ns9ksfjyn"]
}
```

#### Response

`JSON` object representing the uncollected reward:

* `address` of type `string`: address of the Pillar
* `znnAmount` of type `number`: the ZNN amount that has to be collected
* `qsrAmount` of type `number`: the QSR amount that has to be collected

```json
{
    "jsonrpc": "2.0",
    "id": 6,
    "result": {
        "address": "z1qqdtl63rkhap72nlaymtkemlchwv0ns9ksfjyn",
        "znnAmount": 100000000,
        "qsrAmount": 100000000
    }
}
```

### embedded.pillar.getFrontierRewardByPage

> This API call will return reward information about the specified pillar for a specified range of pages.

#### Request

3 parameters:

* first parameter of type `string` that represents the pillar address
* second parameter of type `number` that represents the page index
* third parameter of type `number` that represents the number of entries to be displayed on this page

```json
{
    "jsonrpc": "2.0",
    "id": 7,
    "method": "embedded.pillar.getFrontierRewardByPage",
    "params": ["z1qqdtl63rkhap72nlaymtkemlchwv0ns9ksfjyn", 0, 5]
}
```

#### Response

`JSON` object representing the rewards:

* `count` of type `number`: the total number of the epochs
* an array of entries for the rewards with the following fields:
    - `epoch` of type `number`: the specified epoch
    - `znnAmount` of type `number`: the ZNN amount that has to be collected
    - `qsrAmount` of type `number`: the QSR amount that has to be collected

```json
{
    "jsonrpc": "2.0",
    "id": 7,
    "result": {
        "count": 117,
        "list": [
            {
                "epoch": 116,
                "znnAmount": 1000,
                "qsrAmount": 0
            },
            {
                "epoch": 115,
                "znnAmount": 2000,
                "qsrAmount": 0
            },
            {
                "epoch": 114,
                "znnAmount": 3000,
                "qsrAmount": 0
            },
            {
                "epoch": 113,
                "znnAmount": 4000,
                "qsrAmount": 0
            },
            {
                "epoch": 112,
                "znnAmount": 5000,
                "qsrAmount": 0
            }
        ]
    }
}
```

## embedded.plasma

* [embedded.plasma.get](#embedded.plasma.get)
* [embedded.plasma.getEntriesByAddress](#embedded.plasma.getEntriesByAddress)
* [embedded.plasma.getRequiredFusionAmount](#embedded.plasma.getRequiredFusionAmount)

### embedded.plasma.get

> This API call will return plasma information about an address.

#### Request

One parameter of type `string` that represents the address.

```json
{
    "jsonrpc": "2.0",
    "id": 11,
    "method": "embedded.plasma.get",
    "params": ["z1qz5fskcw8q6zndyu2w5eps9cyk3ekn9ecvcngd"]
}
```

#### Response

`JSON` object representing plasma information:

* `currentPlasma` of type `number`: current amount of plasma
* `maxPlasma` of type `number`: current maximum amount of plasma for this address
* `qsrAmount` of type `number`: amount of QSR being fused for plasma

```json
{
    "jsonrpc": "2.0",
    "id": 11,
    "result": {
        "currentPlasma": 10416000,
        "maxPlasma": 10416000,
        "qsrAmount": 1000000000000
    }
}
```

### embedded.plasma.getEntriesByAddress

> This API call will return the list of all plasma fusion entries.

#### Request

3 parameters:

* first parameter of type `string` that represents the address
* second parameter of type `number` that represents the page
* third parameter of type `number` that represents the number of entries to be displayed on this page

```json
{
    "jsonrpc": "1.0",
    "id": 12,
    "method": "embedded.plasma.getEntriesByAddress",
    "params": ["z1qz5fskcw8q6zndyu2w5eps9cyk3ekn9ecvcngd", 0, 10]
}
```

#### Response

`JSON` object representing the list of all plasma fusion entries

* `count` of type `int`: total number of entries
* `list` of type `array`: list containing all the fusion entries. Each item in the list contains the following fields:
    - `qsrAmount` of type `number`: fused QSR amount
    - `beneficiary` of type `string`: the address that will receive plasma
    - `expirationHeight` of type `number`: momentum height when the fusion entry will expire
    - `id` of type `string`: id used to revoke this entry
* `qsrAmount` of type `number`: total fused QSR amount from all entries

```json
{
    "jsonrpc": "2.0",
    "id": 12,
    "result": {
        "count": 1,
        "list": [
            {
                "qsrAmount": 1000000000000,
                "beneficiary": "z1qz5fskcw8q6zndyu2w5eps9cyk3ekn9ecvcngd",
                "expirationHeight": 1,
                "id": "1dbd7d0b561a41d23c2a469ad42fbd70d5438bae826f6fd607413190c37c363b"
            }
        ],
        "qsrAmount": 1000000000000
    }
}
```

### embedded.plasma.getRequiredPoWForAccountBlock

#### Request

* `address` of type `string`: the sender address
* `blockType` of type `int`: block type
* `toAddress` of type `string`: the received
* `data` of type `string`: encoded smart contract data

```json
{
    "jsonrpc": "2.0",
    "id": 13,
    "method": "embedded.plasma.getRequiredPoWForAccountBlock",
    "params": [
        {
            "address": "z1qz5fskcw8q6zndyu2w5eps9cyk3ekn9ecvcngd",
            "blockType": 1,
            "toAddress": "z1qqdtl63rkhap72nlaymtkemlchwv0ns9ksfjyn",
            "data": "" 
        }
    ]
}
```

#### Response

* `requiredPlasma` of type `int`: required plasma to send the account block
* `requiredDifficulty` of type `int`: required difficulty for the Proof of Work

```json
{
    "jsonrpc": "2.0",
    "id": 13,
    "result": {
        "availablePlasma": 0,
        "basePlasma": 21000,
        "requiredDifficulty": 31500000
    }
}
```

## embedded.sentinel

* [embedded.sentinel.getByOwner](#embedded.sentinel.getByOwner)
* [embedded.sentinel.getAllActive](#embedded.sentinel.getAllActive)
* [embedded.sentinel.getDepositedQsr](#embedded.sentinel.getDepositedQsr)
* [embedded.sentinel.getUncollectedReward](#embedded.sentinel.getUncollectedReward)
* [embedded.sentinel.getFrontierRewardByPage](#embedded.sentinel.getFrontierRewardByPage)

### embedded.sentinel.getByOwner

> This API call will return all the Sentinels registered by an address.

#### Request

One parameter of type `string` that represents the `ownerAddress` of the Sentinels.

```json
{
    "jsonrpc": "2.0",
    "id": 4,
    "method": "embedded.sentinel.getByOwner",
    "params": ["z1qz5fskcw8q6zndyu2w5eps9cyk3ekn9ecvcngd"]
}
```

#### Response

JSON object representing the registered Sentinel:

Same information as [embedded.sentinel.getAllActive](#embedded.sentinel.getAllActive)

```json
{
    "id": 4,
    "jsonrpc": "2.0",
    "result": {
        "owner": "z1qz5fskcw8q6zndyu2w5eps9cyk3ekn9ecvcngd",
        "registrationTimestamp": 1622025090,
        "isRevocable": true,
        "revokeCooldown": 12480,
        "active": true
    } 
}
```

### embedded.sentinel.getAllActive

> This API call will return a list of all registered Sentinels.

#### Request

2 parameters:

* first parameter of type `number` that represents the page
* second parameter of type `number` that represents the number of entries to be displayed on this page

```json
{
    "jsonrpc": "2.0",
    "id": 15,
    "method": "embedded.sentinel.getAllActive",
    "params": [0, 100]
}
```

#### Response

`JSON` object representing the registered Sentinels:

* `count` of type `number`: the total number of registered Sentinels
* an array of `entries` with the following fields:
  - `owner` of type `string`: the address that registered the Sentinel
  - `registrationTimestamp` of type `number`: Sentinel's registration UNIX timestamp
  - `isRevocable` of type `bool`: `true` if the Sentinel can be revoked, `false` otherwise
  - `revokeCooldown` of type `number`: seconds until the revoke window
  - `active` of type `bool`: `true` if Sentinel is active, `false` if revoked

```json
{
    "jsonrpc": "2.0",
    "id": 15,
    "result": {
        "count": 3,
        "list": [
            {   "owner": "z1qz5fskcw8q6zndyu2w5eps9cyk3ekn9ecvcngd",
                "registrationTimestamp": 1622627030,
                "isRevocable": false,
                "revokeCooldown": 63520,
                "active": true
            },
            {
                "owner": "z1qz5fskcw8q6zndyu2w5eps9cyk3ekn9ecvcngd",
                "registrationTimestamp": 1622025090,
                "isRevocable": false,
                "revokeCooldown": 66380,
                "active": true
            },
            {
                "owner": "z1qz5fskcw8q6zndyu2w5eps9cyk3ekn9ecvcngd",
                "registrationTimestamp": 1623135770,
                "isRevocable": false,
                "revokeCooldown": 53860,
                "active": true
            }
        ]
    }
}
```

### embedded.sentinel.getDepositedQsr

> This API call will return the amount of QSR the address has deposited in order to create a Sentinel.

#### Request

One parameter of type `string` that represents the Sentinel address.

```json
{
    "jsonrpc": "2.0",
    "id": 14,
    "method": "embedded.sentinel.getDepositedQsr",
    "params": ["z1qzpa55k8328ff0ys7jfakfvhw8k2cwm53f5d5u"]
}
```

#### Response

`number` that represents the amount of QSR deposited

```json
{
    "id": 14,
    "jsonrpc": "2.0",
    "result": 5000000000000
}
```

### embedded.sentinel.getUncollectedReward

> This API call will return the uncollected reward for the specified sentinel.

#### Request

One parameter of type string that represents the address of the Sentinel

```json
{
    "jsonrpc": "2.0",
    "id": 17,
    "method": "embedded.sentinel.getUncollectedReward",
    "params": ["z1qzpa55k8328ff0ys7jfakfvhw8k2cwm53f5d5u"]
}
```

#### Response

JSON object representing the uncollected reward:

* `address` of type `string`: address of the Sentinel
* `znnAmount` of type `number`: the ZNN amount that has to be collected
* `qsrAmount` of type `number`: the QSR amount that has to be collected

```json
{
    "id": 6,
    "jsonrpc": "2.0",
    "result": {
        "address": "z1qzpa55k8328ff0ys7jfakfvhw8k2cwm53f5d5u",
        "znnAmount": 100000000,
        "qsrAmount": 100000000
    }
}
```

### embedded.sentinel.getFrontierRewardByPage

> This API call will return reward information the specified sentinel for a specified range of pages.

#### Request

3 parameters:

* first parameter of type `string` that represents the sentinel address
* second parameter of type `number` that represents the page index
* third parameter of type `number` that represents the number of entries to be displayed on this page

```json
{
    "jsonrpc": "2.0",
    "id": 7,
    "method": "embedded.sentinel.getFrontierRewardByPage",
    "params": ["z1qzpa55k8328ff0ys7jfakfvhw8k2cwm53f5d5u", 0, 2]
}
```

#### Response

`JSON` object representing the rewards:

* `count` of type `number`: the total number of the epochs
* an array of entries for the rewards with the following fields:
    - `epoch` of type `number`: the specified epoch
    - `znnAmount` of type `number`: the ZNN amount that has to be collected
    - `qsrAmount` of type `number`: the QSR amount that has to be collected

```json
{
    "id": 7,
    "jsonrpc": "2.0",
    "result": {
      "count": 2,
      "list": [
        {
            "epoch": 0,
            "znnAmount": 100000000,
            "qsrAmount":  1000000000
        },
        {
            "epoch": 1,
            "znnAmount": 100000000,
            "qsrAmount":  1000000000
        }
      ]
    }
}
```

## embedded.stake

* [embedded.stake.getEntriesByAddress](#embedded.stake.getEntriesByAddress)
* [embedded.stake.getUncollectedReward](#embedded.stake.getUncollectedReward)
* [embedded.stake.getFrontierRewardByPage](#embedded.stake.getFrontierRewardByPage)

### embedded.stake.getEntriesByAddress

> This API call will return staking information for a particular address

#### Request

3 parameters:

* first parameter of type `string` that represents the address
* second parameter of type `number` that represents the page index
* third parameter of type `number` that represents the number of entries to be displayed on this page

```json
{
    "jsonrpc": "2.0",
    "id": 19,
    "method": "embedded.stake.getEntriesByAddress",
    "params": ["z1qzpa55k8328ff0ys7jfakfvhw8k2cwm53f5d5u", 0, 5]
}
```

#### Response

JSON object representing the staking entries:

* `totalAmount` of type `int`: total amount of ZNN staked
* `totalWeightedAmount` of type `int`: total weighted amount of ZNN staked
* `count` of type `number`: number of entries
* `list` of type `array`: list of entries. Each entry is defined by the following fields:
    - `amount` of type `number`: staking amount
    - `weightedAmount` of type `number`: ZNN amount that is actually staked calculated based on the number of months
    - `startTimestamp` of type `number`: UNIX timestamp of stake registration
    - `expirationTimestamp` of type `number`: UNIX timestamp of stake expiration
    - `address` of type `string`: staking address
    - `id` of type `string`: id of stake, used to cancel the stake

```json
{
    "jsonrpc": "2.0",
    "id": 19,
    "result": {
        "totalAmount": 65000000000,
        "totalWeightedAmount": 73000000000,
        "count": 3,
        "list": [
            {
                "amount": 15000000000,
                "weightedAmount": 15000000000,
                "startTimestamp": 1607956630,
                "expirationTimestamp": 1610548630,
                "address": "z1qzpa55k8328ff0ys7jfakfvhw8k2cwm53f5d5u",
                "id": "172fbda8a0b97fab5b79d4e7790c477e618dac416c9c0dc356267078f9afc549"
            },
            {
              "amount": 25000000000,
              "weightedAmount": 25000000000,
              "startTimestamp": 1607956635,
              "expirationTimestamp": 1610548645,
              "address": "z1qzpa55k8328ff0ys7jfakfvhw8k2cwm53f5d5u",
              "id": "272fbda8a0b97fab5b79d4e7790c477e618dac416c9c0dc356267078f9afc549"
            },
            {
              "amount": 35000000000,
              "weightedAmount": 35000000000,
              "startTimestamp": 1607956640,
              "expirationTimestamp": 1610548660,
              "address": "z1qzpa55k8328ff0ys7jfakfvhw8k2cwm53f5d5u",
              "id": "372fbda8a0b97fab5b79d4e7790c477e618dac416c9c0dc356267078f9afc549"
            }
        ]
    }
}
```

### embedded.stake.getUncollectedReward

> This API call will return the uncollected reward(s) for the specified stake.

#### Request

One parameter of type string that represents the address of the Stake

```json
{
    "jsonrpc": "2.0",
    "id": 17,
    "method": "embedded.stake.getUncollectedReward",
    "params": ["z1qzpa55k8328ff0ys7jfakfvhw8k2cwm53f5d5u"]
}
```

#### Response

JSON object representing the uncollected reward:

* `address` of type `string`: address of the Stake
* `znnAmount` of type `number`: the ZNN amount that has to be collected
* `qsrAmount` of type `number`: the QSR amount that has to be collected

```json
{
    "jsonrpc": "2.0",
    "id": 6,
    "result": {
        "address": "z1qzpa55k8328ff0ys7jfakfvhw8k2cwm53f5d5u",
        "znnAmount": 0,
        "qsrAmount": 100000000
    }
}
```

### embedded.stake.getFrontierRewardByPage

> This API call will return reward information the specified stake for a specified range of pages.

#### Request

3 parameters:

* first parameter of type `string` that represents the stake address
* second parameter of type `number` that represents the page index
* third parameter of type `number` that represents the number of entries to be displayed on this page

```json
{
    "jsonrpc": "2.0",
    "id": 7,
    "method": "embedded.stake.getFrontierRewardByPage",
    "params": ["z1qzpa55k8328ff0ys7jfakfvhw8k2cwm53f5d5u", 0, 2]
}
```

#### Response

`JSON` object representing the rewards:

* `count` of type `number`: the total number of the epochs
* an array of entries for the rewards with the following fields:
    - `epoch` of type `number`: the specified epoch
    - `znnAmount` of type `number`: the ZNN amount that has to be collected
    - `qsrAmount` of type `number`: the QSR amount that has to be collected

```json
{
    "jsonrpc": "2.0",
    "id": 7,
    "result": {
        "count": 2,
        "list": [
                {
                    "epoch": 0,
                    "znnAmount": 0,
                    "qsrAmount":  1000000000
                },
                {
                    
                    "epoch": 1,
                    "znnAmount": 0,
                    "qsrAmount":  1000000000
                }
        ]
    }
}
```

## embedded.swap

* [embedded.swap.getAssetsByKeyIdHash](#embedded.swap.getAssetsByKeyIdHash)
* [embedded.swap.getAssets](#embedded.swap.getAssets)
* [embedded.swap.getLegacyPillars](#embedded.swap.getLegacyPillars)

### embedded.swap.getAssetsByKeyIdHash

> This API call will return the amount of ZNN and QSR that have not been swapped yet

#### Request

One parameter of type `string` that represents the `sha256` sum of the legacy keyId which is `HASH160` sum of a legacy public key

```json
{
    "jsonrpc": "2.0",
    "id": 20,
    "method": "embedded.swap.getAssetsByKeyIdHash",
    "params": ["3835082b4afb76971d58d6ad510e7e91f3bb0d41912fac4ec4cfef7bd7bbea73"]
}
```

#### Response

An array of `entries`:

* `sha256` sum of the legacy keyId which is `HASH160` sum of a legacy public key
* `qsr` of type `number`: QSR amount left
* `znn` of type `number`: ZNN amount left

```json
{
    "jsonrpc": "2.0",
    "id": 20,
    "result": [
        {
            "keyIdHash": "3835082b4afb76971d58d6ad510e7e91f3bb0d41912fac4ec4cfef7bd7bbea73",
            "qsr": 25000000000000,
            "znn": 2500000000000
        }
    ]
}
```

### embedded.swap.getAssets

> This API call will return for every keyId hash the amount of znn or qsr that can be swapped

#### Request

No parameters

```json
{
    "jsonrpc": "2.0",
    "id": 20,
    "method": "embedded.swap.getAssets",
    "params": []
}
```

#### Response

An array of `entries`:

* `keyIdHash` of type string: `sha256` sum of the legacy keyId which is `HASH160` sum of a legacy public key
* `qsr` of type `number`: QSR amount left
* `znn` of type `number`: ZNN amount left

```json
{
    "jsonrpc": "2.0",
    "id": 2,
    "result": {
        "abdefg0123456789d7fdea4cc00ca15ba9f703e3f611b70c0bd022e94d6eabcd": {
            "znn": 14507500000000,
            "qsr": 0
        },
        "bbdefg0123456789d7fdea4cc00ca15ba9f703e3f611b70c0bd022e94d6eabcd": {
            "znn": 503780000000,
            "qsr": 0
        }
    }
}
```

### embedded.swap.getLegacyPillars

> This API call will return the number of legacy Pillars not swapped yet

#### Request

No parameters

```json
{
    "jsonrpc": "2.0",
    "id": 21,
    "method": "embedded.swap.getLegacyPillars",
    "params": []
}
```

#### Response

An array of `entries`:

* `KeyIdHash` of type `string`:  `sha256` sum of the legacy keyId which is `HASH160` sum of a legacy public key
* `numPillars` of type `number`: number of legacy Pillars left

```json
{
    "id": 21,
    "jsonrpc": "2.0",
    "result": [
        {
          "KeyIdHash": "abdefg0123456789d7fdea4cc00ca15ba9f703e3f611b70c0bd022e94d6eabcd",
          "numPillars": 1
        },
        {
          "KeyIdHash": "bbdefg0123456789d7fdea4cc00ca15ba9f703e3f611b70c0bd022e94d6eabcd",
          "numPillars": 1
        }
    ]
}
```

## embedded.token

* [embedded.token.getAll](#embedded.token.getAll)
* [embedded.token.getByOwner](#embedded.token.getByOwner)
* [embedded.token.getByZts](#embedded.token.getByZts)

### embedded.token.getAll

> This API call will return a list of all ZTS tokens

#### Request

2 parameters:

* first parameter of type `number` that represents the page
* second parameter of type `number` that represents the number of entries

```json
{
    "jsonrpc": "2.0",
    "id": 22,
    "method": "embedded.token.getAll",
    "params": [0, 2]
}
```

#### Response

An array of `entries`:

* `count` of type `number`: total number of ZTS tokens
* `list` of type `array`: array containing information about each token ordered by creation time. Each entry is defined by the following fields:
    - `name` of type `string`: name of the ZTS token
    - `symbol` of type `string`: symbol of the ZTS token
    - `domain` of type `string`: that represent a valid web domain
    - `totalSupply` of type `number`: circulating supply
    - `decimals` of type `number`: number of decimals
    - `owner` of type `string`: address that issued this token
    - `tokenStandard` of type `string`: unique identifier of this ZTS
    - `maxSupply` of type `number`: maximum supply
    - `isBurnable` of type `bool`: `true` if it can be burned by any holder, `false` if it can be burned only by its owner
    - `isMintable` of type `bool`: `true` if the owner can increase the supply by minting, `false` otherwise
    - `isUtility` of type `bool`: `true` if the token is utilty, `false` otherwise

```json
{
    "jsonrpc": "2.0",
    "id": 22,
    "result": {
        "count": 200,
        "list": [
            {
                "name": "testToken1",
                "symbol": "test1",
                "domain": "zenon.network",
                "totalSupply": 3000305500000000000,
                "decimals": 8,
                "owner": "z1qz5p95pa8c6wq9pvfkg642gjv4nnaayx6vhm2w",
                "tokenStandard": "zts10mxtv874tre00stfd6uelu",
                "maxSupply": 4611686018427387903,
                "isBurnable": true,
                "isMintable": true,
                "isUtility": true
            },
            {
                "name": "testToken2",
                "symbol": "test2",
                "domain": "zenon.network",
                "totalSupply": 3000038061600000000,
                "decimals": 8,
                "owner": "z1qz5p95pa8c6wq9pvfkg642gjv4nnaayx6vhm2w",
                "tokenStandard": "zts1nl8h22r2ynw9jyeg5q5ssq",
                "maxSupply": 4611686018427387903,
                "isBurnable": false,
                "isMintable": true,
                "isUtility": true
            }
        ]
    }
}
```

### embedded.token.getByOwner

> This API call will return the list of ZTS issued by an address

#### Request

3 parameters:

* first parameter of type `string` that represents the address
* second parameter of type `number` that represents the page index
* third parameter of type `number` that represents the number of entries to be displayed on this page

```json
{
    "jsonrpc": "2.0",
    "id": 24,
    "method": "embedded.token.getByOwner",
    "params": ["z1qz5p95pa8c6wq9pvfkg642gjv4nnaayx6vhm2w", 0, 5]
}
```

#### Response

Same information as [embedded.token.getAll](#embedded.token.getAll)

```json
{
    "jsonrpc": "2.0",
    "id": 24,
    "result": {
        "count": 1,
        "list": [
            {
                "name": "testToken2",
                "symbol": "test2",
                "domain": "zenon.network",
                "totalSupply": 3000038061600000000,
                "decimals": 8,
                "owner": "z1qz5p95pa8c6wq9pvfkg642gjv4nnaayx6vhm2w",
                "tokenStandard": "zts1nl8h22r2ynw9jyeg5q5ssq",
                "maxSupply": 4611686018427387903,
                "isBurnable": true,
                "isMintable": true,
                "isUtility": true
            }
        ]
    }
}
```

### embedded.token.getByZts

> This API call will return the ZTS with the specified unique indentifier

#### Request

One parameter of type `string` that represents the ZTS

```json
{
    "jsonrpc": "2.0",
    "id": 23,
    "method": "embedded.token.getByZts",
    "params": ["zts1nl8h22r2ynw9jyeg5q5ssq"]
}
```

#### Response

Same information as [embedded.token.getAll](#embedded.token.getAll)

```json
{
    "id": 23,
    "jsonrpc": "2.0",
    "result": {
        "name": "testToken1",
        "symbol": "test1",
        "domain": "zenon.network",
        "totalSupply": 500,
        "decimals": 8,
        "owner": "z1qz3f6svf805tewktk5yf9tn8cdhe2236wdnugk",
        "tokenStandard": "zts1nl8h22r2ynw9jyeg5q5ssq",
        "maxSupply": 1000,
        "isBurnable": true,
        "isMintable": true,
        "isUtility": true
    }
}
```

## Dual-ledger

* [ledger.getFrontierAccountBlock](#ledger.getFrontierAccountBlock)
* [ledger.getUnconfirmedBlocksByAddress](#ledger.getUnconfirmedBlocksByAddress)
* [ledger.getUnreceivedBlocksByAddress](#ledger.getUnreceivedBlocksByAddress)
* [ledger.getAccountBlockByHash](#ledger.getAccountBlockByHash)
* [ledger.getAccountBlocksByHeight](#ledger.getAccountBlocksByHeight)
* [ledger.getAccountBlocksByPage](#ledger.getAccountBlocksByPage)
* [ledger.getFrontierMomentum](#ledger.getFrontierMomentum)
* [ledger.getMomentumBeforeTime](#ledger.getMomentumBeforeTime)
* [ledger.getMomentumByHash](#ledger.getMomentumByHash)
* [ledger.getMomentumsByHeight](#ledger.getMomentumsByHeight)
* [ledger.getMomentumsByPage](#ledger.getMomentumsByPage)
* [ledger.getDetailedMomentumsByHeight](#ledger.getDetailedMomentumsByHeight)
* [ledger.getAccountInfoByAddress](#ledger.getAccountInfoByAddress)

### ledger.getFrontierAccountBlock

> This API call will return the last account block of the specified address

#### Request

One parameter of type `string` that represents the address

```json
{
    "jsonrpc": "2.0",
    "id": 25,
    "method": "ledger.getFrontierAccountBlock",
    "params": ["z1qzpa55k8328ff0ys7jfakfvhw8k2cwm53f5d5u"]
}
```

#### Response

`JSON` object representing the Frontier Account Block with the following fields:

* `version` of type `number`: current account block version
* `chainIdentifier` of type `number`: network chain identifier
* `blockType` of type `number`: type of the block
* `hash` of type `string`: current account block hash
* `previousHash` of type `string`: hash of the block `height - 1` on this account-chain, `000 ... 00` if `height == 0`
* `height` of type `number`: height of this account-chain
* `momentumAcknowledged` of type `dictionary`: momentum that must exists in order for this transaction to be valid 
  - `hash` of type `string`: hash of the momentum
  - `height` of type `number`: height of the momentum
* `address` of type `string`: address that published this account block
* `toAddress` of type `string`: receiving address
* `amount` of type `number`: ZTS amount
* `tokenStandard` of type `string`: token standard of the coin / token send in the corresponding send block
* `fromBlockHash` of type `string`:
* `descendantBlocks` of type `array` of `dictionaries`: all the account blocks that this block generated for embedded smart contracts, `null` in this case
* `data` of type `string`: `null` in case of default regular transactions, not `null` otherwise
* `fusedPlasma` of type `number`: plasma used for this account block from fusion
* `difficulty` of type `number`: PoW difficulty used to generate PoWPlasma
* `nonce` of type `string`: nonce that satisfies difficulty
* `basePlasma` of type `number`: min plasma for current account block
* `usedPlasma` of type `number`: sum of fusedPlasma and PoWPlasma
* `changesHash` of type `string`: hash of the changes that were applied to the NoM after inserting this account-block
* `publicKey` of type `string`: public key of the account block owner
* `signature` of type `string`: signature of this account block
* `token` of type `dictionary`: contains all the information about the ZTS, as defined in [embedded.token](#embedded.token)
* `confirmationDetail` of type `dictionary`: contains details about the momentum which contains this account-block
  - `numConfirmations` of type `number`: height difference between frontier momentum and confirmation-momentum
  - `momentumHeight` of type `number`: confirmation-momentum height
  - `momentumHash` of type `string`: confirmation-momentum hash
  - `momentumTimestamp` of type `number`: confirmation-momentum timestamp
* `pairedAccountBlock` of type `dictionary`: prefetched account-block. For Receive blocks, points to the send block. For send blocks, points to the receive block if it exists. otherwise is null.

```json
{
    "jsonrpc": "2.0",
    "id": 25,
    "result": {
        "version": 1,
        "chainIdentifier": 3,
        "blockType": 2,
        "hash": "578ddd15e6ee8c08d31575392c6d9901823ac13f9d6a50ef2eeb89b101b2db1d",
        "previousHash": "f3036bb04c91ea67a2950dde2b097e35267ab616417a1e48095b298f45a661e3",
        "height": 150,
        "momentumAcknowledged": {
            "hash": "24f1339df42935a57356e00aa8bfa183ed5b86a6aa7da8ee646ebd03e7a72c40",
            "height": 33410
        },
        "address": "z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt",
        "toAddress": "z1qxemdeddedxlyquydytyxxxxxxxxxxxxflaaae",
        "amount": 0,
        "tokenStandard": "zts1qqqqqqqqqqqqqqqqtq587y",
        "fromBlockHash": "0000000000000000000000000000000000000000000000000000000000000000",
        "descendantBlocks": [],
        "data": "IAk+pg==",
        "fusedPlasma": 52500,
        "difficulty": 0,
        "nonce": "0000000000000000",
        "basePlasma": 52500,
        "usedPlasma": 52500,
        "changesHash": "b12a4749287b75a264e8e42be84c53d792c83fbf6d9a3f921d650ae78cd3785d",
        "publicKey": "6zoCw5uZaS4N2eJlqGh2SmgSJbh5mJNY8aOgKLWl+D4=",
        "signature": "To0tLosaVbRcXhjzImPusxn1YKuK6nmpzbDNZDvonyu5bErAFVq1/Mhy47xbq+QRzTmWNb5BanXPxIsrLjdUDg==",
        "token": null,
        "confirmationDetail": {
            "numConfirmations": 996,
            "momentumHeight": 33411,
            "momentumHash": "0de0a593358ab4860f4349c54f8e4f975edecd90e85b87c67f3f26dba7ff1ced",
            "momentumTimestamp": 1637588850
        },
        "pairedAccountBlock": {
            "version": 1,
            "chainIdentifier": 3,
            "blockType": 5,
            "hash": "a02d89c57b7571fb76159dcb3fb30c31aa9ae11f57d01da3fafc7808b0f1eec1",
            "previousHash": "81507dab31bb0bbe8e14aa886bd445037464e7e57f6feab531b8fcb91ad13d0f",
            "height": 123,
            "momentumAcknowledged": {
                "hash": "0de0a593358ab4860f4349c54f8e4f975edecd90e85b87c67f3f26dba7ff1ced",
                "height": 33411
            },
            "address": "z1qxemdeddedxlyquydytyxxxxxxxxxxxxflaaae",
            "toAddress": "z1qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqsggv2f",
            "amount": 0,
            "tokenStandard": "zts1qqqqqqqqqqqqqqqqtq587y",
            "fromBlockHash": "578ddd15e6ee8c08d31575392c6d9901823ac13f9d6a50ef2eeb89b101b2db1d",
            "descendantBlocks": [],
            "data": "AAAAAAAAAAE=",
            "fusedPlasma": 0,
            "difficulty": 0,
            "nonce": "0000000000000000",
            "basePlasma": 0,
            "usedPlasma": 0,
            "changesHash": "f24bb794df99f5a727ced81f2f78259ab1c8a0909e06af0eb69aa142afea605e",
            "publicKey": null,
            "signature": null,
            "token": null,
            "confirmationDetail": {
                "numConfirmations": 995,
                "momentumHeight": 33412,
                "momentumHash": "2e3c8e22911a19854cc727c598e9576b29aaf1da0441db3f22d1abab391adc26",
                "momentumTimestamp": 1637588860
            },
            "pairedAccountBlock": null
        }
    }
}
```

### ledger.getUnconfirmedBlocksByAddress

> This API call will return a list of all account blocks sent to this address that have not been included into a momentum so far

#### Request

3 parameters:

* first parameter of type `string` that represents the address
* second parameter of type `number` that represents the page
* third parameter of type `number` that represents the number of entries to be displayed on this page

```json
{
    "jsonrpc": "2.0",
    "id": 26,
    "method": "ledger.getUnconfirmedBlocksByAddress",
    "params": ["z1qz3f6svf805tewktk5yf9tn8cdhe2236wdnugk", 0, 1]
}
```

#### Response

An array of `entries`:

* `list` of type `array`: array containing same information as [embedded.ledger.getFrontierAccountBlock](#embedded.ledger.getFrontierAccountBlock)
* `count` of type `int`: number of unconfirmed account blocks in total
* `more` of type `bool`: whether there are more unconfirmed account blocks

```json
{
    "jsonrpc": "2.0",
    "id": 26,
    "result": {
        "list": [
            {
                "version": 1,
                "chainIdentifier": 3,
                "blockType": 4,
                "hash": "12514c8f80b6740455b6486aea6f1da2c05edb819ec780d39fd7827e3b904557",
                "previousHash": "ddd81c83b687f6d6c66897cf32c5f04e7428dec9bc296450887fdbf924aa92f0",
                "height": 25,
                "momentumAcknowledged": {
                    "hash": "3077eff5d46281f277d7a421446f74868f7bffaac36a718e24d971c0b43c3655",
                    "height": 32540
                },
                "address": "z1qxemdeddedxt0kenxxxxxxxxxxxxxxxxh9amk0",
                "toAddress": "z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt",
                "amount": 1234567890,
                "tokenStandard": "zts17ejv0drss06n5qz7ccad24",
                "fromBlockHash": "0000000000000000000000000000000000000000000000000000000000000000",
                "descendantBlocks": [],
                "data": "",
                "fusedPlasma": 0,
                "difficulty": 0,
                "nonce": "0000000000000000",
                "basePlasma": 0,
                "usedPlasma": 0,
                "changesHash": "0000000000000000000000000000000000000000000000000000000000000000",
                "publicKey": null,
                "signature": null,
                "token": {
                    "name": "ZenonWikiToken",
                    "symbol": "ZWT",
                    "domain": "zenon.wiki",
                    "totalSupply": 1234567890,
                    "decimals": 8,
                    "owner": "z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt",
                    "tokenStandard": "zts17ejv0drss06n5qz7ccad24",
                    "maxSupply": 1234567890,
                    "isBurnable": true,
                    "isMintable": false,
                    "isUtility": true
                },
                "confirmationDetail": {
                    "numConfirmations": 2044,
                    "momentumHeight": 32541,
                    "momentumHash": "ea104a85df9e6effc91ff7633ffc2950a4382554ed475cae362b0160b61f43ca",
                    "momentumTimestamp": 1637580150
                },
                "pairedAccountBlock": null
            }
        ],
        "count": 2,
        "more": false
    }
}
```

### ledger.getUnreceivedBlocksByAddress

> This API call will return a list of all account blocks sent to this address that currently don't have a corresponding receive account block

#### Request

3 parameters:

* first parameter of type `string` that represents the address
* second parameter of type `number` that represents the page
* third parameter of type `number` that represents the number of entries to be displayed on this page

```json
{
    "jsonrpc": "2.0",
    "id": 27,
    "method": "ledger.getUnreceivedBlocksByAddress",
    "params": ["z1qqna5fwl9cfd4h7xyg54qdg3nlxgjhntekdlw4", 0, 5]
}
```

#### Response

Same information as [embedded.ledger.getUnconfirmedBlocksByAddress](#embedded.ledger.getUnconfirmedBlocksByAddress)

```json
{
    "jsonrpc": "2.0",
    "id": 27,
    "result": {
        "list": [
            {
                "version": 1,
                "chainIdentifier": 3,
                "blockType": 4,
                "hash": "12514c8f80b6740455b6486aea6f1da2c05edb819ec780d39fd7827e3b904557",
                "previousHash": "ddd81c83b687f6d6c66897cf32c5f04e7428dec9bc296450887fdbf924aa92f0",
                "height": 25,
                "momentumAcknowledged": {
                    "hash": "3077eff5d46281f277d7a421446f74868f7bffaac36a718e24d971c0b43c3655",
                    "height": 32540
                },
                "address": "z1qxemdeddedxt0kenxxxxxxxxxxxxxxxxh9amk0",
                "toAddress": "z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt",
                "amount": 1234567890,
                "tokenStandard": "zts17ejv0drss06n5qz7ccad24",
                "fromBlockHash": "0000000000000000000000000000000000000000000000000000000000000000",
                "descendantBlocks": [],
                "data": "",
                "fusedPlasma": 0,
                "difficulty": 0,
                "nonce": "0000000000000000",
                "basePlasma": 0,
                "usedPlasma": 0,
                "changesHash": "0000000000000000000000000000000000000000000000000000000000000000",
                "publicKey": null,
                "signature": null,
                "token": {
                    "name": "ZenonWikiToken",
                    "symbol": "ZWT",
                    "domain": "zenon.wiki",
                    "totalSupply": 1234567890,
                    "decimals": 8,
                    "owner": "z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt",
                    "tokenStandard": "zts17ejv0drss06n5qz7ccad24",
                    "maxSupply": 1234567890,
                    "isBurnable": true,
                    "isMintable": false,
                    "isUtility": true
                },
                "confirmationDetail": {
                    "numConfirmations": 2092,
                    "momentumHeight": 32541,
                    "momentumHash": "ea104a85df9e6effc91ff7633ffc2950a4382554ed475cae362b0160b61f43ca",
                    "momentumTimestamp": 1637580150
                },
                "pairedAccountBlock": null
            }
        ],
        "count": 2,
        "more": false
    }
}
```

### ledger.getAccountBlockByHash

> This API call will return information about the account block with the specified hash

#### Request

One parameter of type `string` that represents the hash

```json
{
    "jsonrpc": "2.0",
    "id": 28,
    "method": "ledger.getAccountBlockByHash",
    "params": ["e543ecc94dc51f68eda9de0ec8ee94ad00ca4608df7b4d3c34b31511811be965"]
}
```

#### Response

Same information as [ledger.getFrontierAccountBlock](#ledger.getFrontierAccountBlock)

```json
{
    "jsonrpc": "2.0",
    "id": 28,
    "result": {
        "version": 1,
        "chainIdentifier": 3,
        "blockType": 4,
        "hash": "12514c8f80b6740455b6486aea6f1da2c05edb819ec780d39fd7827e3b904557",
        "previousHash": "ddd81c83b687f6d6c66897cf32c5f04e7428dec9bc296450887fdbf924aa92f0",
        "height": 25,
        "momentumAcknowledged": {
            "hash": "3077eff5d46281f277d7a421446f74868f7bffaac36a718e24d971c0b43c3655",
            "height": 32540
        },
        "address": "z1qxemdeddedxt0kenxxxxxxxxxxxxxxxxh9amk0",
        "toAddress": "z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt",
        "amount": 1234567890,
        "tokenStandard": "zts17ejv0drss06n5qz7ccad24",
        "fromBlockHash": "0000000000000000000000000000000000000000000000000000000000000000",
        "descendantBlocks": [],
        "data": "",
        "fusedPlasma": 0,
        "difficulty": 0,
        "nonce": "0000000000000000",
        "basePlasma": 0,
        "usedPlasma": 0,
        "changesHash": "0000000000000000000000000000000000000000000000000000000000000000",
        "publicKey": null,
        "signature": null,
        "token": {
            "name": "ZenonWikiToken",
            "symbol": "ZWT",
            "domain": "zenon.wiki",
            "totalSupply": 1234567890,
            "decimals": 8,
            "owner": "z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt",
            "tokenStandard": "zts17ejv0drss06n5qz7ccad24",
            "maxSupply": 1234567890,
            "isBurnable": true,
            "isMintable": false,
            "isUtility": true
        },
        "confirmationDetail": {
            "numConfirmations": 2102,
            "momentumHeight": 32541,
            "momentumHash": "ea104a85df9e6effc91ff7633ffc2950a4382554ed475cae362b0160b61f43ca",
            "momentumTimestamp": 1637580150
        },
        "pairedAccountBlock": null
    }
}
```

### ledger.getAccountBlocksByHeight

> This API call will return a list of account blocks for the account-chain with the specified address

#### Request

3 parameters:

* first parameter of type `string` that represents the address
* second parameter of type `number` that represents the starting height
* third parameter of type `number` that represents the count

```json
{
    "jsonrpc": "2.0",
    "id": 29,
    "method": "ledger.getAccountBlocksByHeight",
    "params": ["z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt", 25, 1]
}
```

#### Response

Same information as [embedded.ledger.getUnconfirmedBlocksByAddress](#embedded.ledger.getUnconfirmedBlocksByAddress)

```json
{
    "jsonrpc": "2.0",
    "id": 29,
    "result": {
        "list": [
            {
                "version": 1,
                "chainIdentifier": 3,
                "blockType": 2,
                "hash": "e86390533149b969d3beb3887241caf4aa690175603cae13271befdcd368a7aa",
                "previousHash": "bf96b8fe86229e9866a660d9340b5088e8bf61818831c54785bbf6eb1beebe2e",
                "height": 25,
                "momentumAcknowledged": {
                    "hash": "7b97a15bf49f48cf8b13831117cfe3786044c3e5f7b5220d9698c587bc97cbd5",
                    "height": 5417
                },
                "address": "z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt",
                "toAddress": "z1qxemdeddedxstakexxxxxxxxxxxxxxxxjv8v62",
                "amount": 0,
                "tokenStandard": "zts1qqqqqqqqqqqqqqqqtq587y",
                "fromBlockHash": "0000000000000000000000000000000000000000000000000000000000000000",
                "descendantBlocks": [],
                "data": "IAk+pg==",
                "fusedPlasma": 52500,
                "difficulty": 0,
                "nonce": "0000000000000000",
                "basePlasma": 52500,
                "usedPlasma": 52500,
                "changesHash": "0aa24240f88d6081daaba6c579ac31b2f6af55602cd3faf71db250f462b7a02f",
                "publicKey": "6zoCw5uZaS4N2eJlqGh2SmgSJbh5mJNY8aOgKLWl+D4=",
                "signature": "nF2oI/UA+ukvgUUAj//bn5FiJRRkiwRXr94pRHOzvI6WQgEfd/7AcMIUKc5BgJ8GajaO9B2O1i5lIOeDIO/7AQ==",
                "token": null,
                "confirmationDetail": {
                    "numConfirmations": 29264,
                    "momentumHeight": 5418,
                    "momentumHash": "8443608ae19195a4e92924fde8d67c3371bc045ccfc23950e86c7b224c9cfb06",
                    "momentumTimestamp": 1637308410
                },
                "pairedAccountBlock": {
                    "version": 1,
                    "chainIdentifier": 3,
                    "blockType": 5,
                    "hash": "341d7f182b230e55762b1d018b2fe8492ce3de10a9e38fea2b15afd239ccc591",
                    "previousHash": "d98500d9bdf4c0f6bb93920aa9eae50ff688566bb0463d6dbf18bfc858882b94",
                    "height": 21,
                    "momentumAcknowledged": {
                        "hash": "8443608ae19195a4e92924fde8d67c3371bc045ccfc23950e86c7b224c9cfb06",
                        "height": 5418
                    },
                    "address": "z1qxemdeddedxstakexxxxxxxxxxxxxxxxjv8v62",
                    "toAddress": "z1qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqsggv2f",
                    "amount": 0,
                    "tokenStandard": "zts1qqqqqqqqqqqqqqqqtq587y",
                    "fromBlockHash": "e86390533149b969d3beb3887241caf4aa690175603cae13271befdcd368a7aa",
                    "descendantBlocks": [],
                    "data": "AAAAAAAAAAE=",
                    "fusedPlasma": 0,
                    "difficulty": 0,
                    "nonce": "0000000000000000",
                    "basePlasma": 0,
                    "usedPlasma": 0,
                    "changesHash": "eec1a5c050310df429a6f26b56b3dfd6d9d26d1192fcf17100c477302be9190b",
                    "publicKey": null,
                    "signature": null,
                    "token": null,
                    "confirmationDetail": {
                        "numConfirmations": 29263,
                        "momentumHeight": 5419,
                        "momentumHash": "43c8e4edd0e4ef0ebccea8186495e2ec6a76ad58e5490938930327788c3add38",
                        "momentumTimestamp": 1637308420
                    },
                    "pairedAccountBlock": null
                }
            }
        ],
        "count": 150,
        "more": false
    }
}
```

### ledger.getAccountBlocksByPage

> This API call will return a list of account blocks for the account-chain with the specified address for a specified range of pages.

#### Request

3 parameters:

* first parameter of type `string` that represents the address
* second parameter of type `number` that represents the page index
* third parameter of type `number` that represents the number of entries to be displayed on this page

```json
{
    "jsonrpc": "2.0",
    "id": 30,
    "method": "ledger.getAccountBlocksByPage",
    "params": ["z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt", 0, 1]
}
```

#### Response

Same information as [embedded.ledger.getUnconfirmedBlocksByAddress](#embedded.ledger.getUnconfirmedBlocksByAddress)

```json
{
    "jsonrpc": "2.0",
    "id": 30,
    "result": {
        "list": [
            {
                "version": 1,
                "chainIdentifier": 3,
                "blockType": 2,
                "hash": "578ddd15e6ee8c08d31575392c6d9901823ac13f9d6a50ef2eeb89b101b2db1d",
                "previousHash": "f3036bb04c91ea67a2950dde2b097e35267ab616417a1e48095b298f45a661e3",
                "height": 150,
                "momentumAcknowledged": {
                    "hash": "24f1339df42935a57356e00aa8bfa183ed5b86a6aa7da8ee646ebd03e7a72c40",
                    "height": 33410
                },
                "address": "z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt",
                "toAddress": "z1qxemdeddedxlyquydytyxxxxxxxxxxxxflaaae",
                "amount": 0,
                "tokenStandard": "zts1qqqqqqqqqqqqqqqqtq587y",
                "fromBlockHash": "0000000000000000000000000000000000000000000000000000000000000000",
                "descendantBlocks": [],
                "data": "IAk+pg==",
                "fusedPlasma": 52500,
                "difficulty": 0,
                "nonce": "0000000000000000",
                "basePlasma": 52500,
                "usedPlasma": 52500,
                "changesHash": "b12a4749287b75a264e8e42be84c53d792c83fbf6d9a3f921d650ae78cd3785d",
                "publicKey": "6zoCw5uZaS4N2eJlqGh2SmgSJbh5mJNY8aOgKLWl+D4=",
                "signature": "To0tLosaVbRcXhjzImPusxn1YKuK6nmpzbDNZDvonyu5bErAFVq1/Mhy47xbq+QRzTmWNb5BanXPxIsrLjdUDg==",
                "token": null,
                "confirmationDetail": {
                    "numConfirmations": 1289,
                    "momentumHeight": 33411,
                    "momentumHash": "0de0a593358ab4860f4349c54f8e4f975edecd90e85b87c67f3f26dba7ff1ced",
                    "momentumTimestamp": 1637588850
                },
                "pairedAccountBlock": {
                    "version": 1,
                    "chainIdentifier": 3,
                    "blockType": 5,
                    "hash": "a02d89c57b7571fb76159dcb3fb30c31aa9ae11f57d01da3fafc7808b0f1eec1",
                    "previousHash": "81507dab31bb0bbe8e14aa886bd445037464e7e57f6feab531b8fcb91ad13d0f",
                    "height": 123,
                    "momentumAcknowledged": {
                        "hash": "0de0a593358ab4860f4349c54f8e4f975edecd90e85b87c67f3f26dba7ff1ced",
                        "height": 33411
                    },
                    "address": "z1qxemdeddedxlyquydytyxxxxxxxxxxxxflaaae",
                    "toAddress": "z1qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqsggv2f",
                    "amount": 0,
                    "tokenStandard": "zts1qqqqqqqqqqqqqqqqtq587y",
                    "fromBlockHash": "578ddd15e6ee8c08d31575392c6d9901823ac13f9d6a50ef2eeb89b101b2db1d",
                    "descendantBlocks": [],
                    "data": "AAAAAAAAAAE=",
                    "fusedPlasma": 0,
                    "difficulty": 0,
                    "nonce": "0000000000000000",
                    "basePlasma": 0,
                    "usedPlasma": 0,
                    "changesHash": "f24bb794df99f5a727ced81f2f78259ab1c8a0909e06af0eb69aa142afea605e",
                    "publicKey": null,
                    "signature": null,
                    "token": null,
                    "confirmationDetail": {
                        "numConfirmations": 1288,
                        "momentumHeight": 33412,
                        "momentumHash": "2e3c8e22911a19854cc727c598e9576b29aaf1da0441db3f22d1abab391adc26",
                        "momentumTimestamp": 1637588860
                    },
                    "pairedAccountBlock": null
                }
            }
        ],
        "count": 150,
        "more": false
    }
}
```

### ledger.getFrontierMomentum

> This API call will return the latest momentum

#### Request

No parameters

```json
{
    "jsonrpc": "2.0",
    "id": 31,
    "method": "ledger.getFrontierMomentum",
    "params": []
}
```

#### Response

`JSON` object representing the frontier momentum:

* `version` of type `number`: momentum version
* `chainIdentifier` of type `number`: network chain identifier
* `hash` of type `string`: hash of the momentum
* `previousHash` of type `string`: hash of the `height - 1` momentum
* `height` of type `number`: height of the momentum
* `timestamp` of type `number`: UNIX timestamp of accepted momentum
* `data` of type `string`: encoded smart contract data
* `content` of type `dictionary`: contains a list of account blocks that are included in this momentum
* `changesHash` of type `string`: hash of the changes that were applied to the NoM after inserting this momentum
* `publicKey` of type `string`: public key of the producer
* `signature` of type `string`: signature of the momentum
* `producer` of type `string`: `producerAddress` of the Pillar that produced this momentum

```json
{
    "jsonrpc": "2.0",
    "id": 31,
    "result": {
        "version": 1,
        "chainIdentifier": 3,
        "hash": "3e890162362a03dca5df079fc2d5991c128d8980d6d7d68e019565023c55ec1a",
        "previousHash": "001f3fab94417e5d4f1159428395785f4f0622b4d6f96d0cf7bcaa22d634e76c",
        "height": 34715,
        "timestamp": 1637601890,
        "data": "",
        "content": [],
        "changesHash": "a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a",
        "publicKey": "aBOWZfktDI3QxgMh9df0HpX2wns4mZtKrDKUIYvgK+I=",
        "signature": "nfnOscPt+kfCJjYJVTGxDWPSwUrT9micsiecyOQ2eqoT5jUB9MnK1eUrJmk/Qq9yGNZD1pUZhijsctcDcymXDQ==",
        "producer": "z1qqmqp40duzvtxvg7dwxph7724mq63t3mru297p"
    }
}
```

### ledger.getMomentumBeforeTime

> This API call will return the momentum for the period before the specified time

#### Request

One parameter of type `number` that represents the time

```json
{
    "jsonrpc": "2.0",
    "id": 32,
    "method": "ledger.getMomentumBeforeTime",
    "params": [1833524084]
}
```

#### Response

Same information as [ledger.getFrontierMomentum](#ledger.getFrontierMomentum)

```json
{
    "jsonrpc": "2.0",
    "id": 32,
    "result": {
        "version": 1,
        "chainIdentifier": 3,
        "hash": "b11bc2190922f0c9bcc89a338f2127b53a7adb1d3f7d4b2a53d6a25881acc146",
        "previousHash": "f21fc76bb57acdbbc4041eeee901b4b62b2819c4dd7db35c5a0052f0429977ba",
        "height": 34745,
        "timestamp": 1637602190,
        "data": "",
        "content": [],
        "changesHash": "a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a",
        "publicKey": "aBOWZfktDI3QxgMh9df0HpX2wns4mZtKrDKUIYvgK+I=",
        "signature": "64rGI8PqSlxd5x3fyCRRZx0PY233S6pvlA3vNG40IGwaP8qK5P++pV1DqNu6CmksMkS+NWFMFuiT4bwOmMwJAQ==",
        "producer": "z1qqmqp40duzvtxvg7dwxph7724mq63t3mru297p"
    }
}
```

### ledger.getMomentumByPage

> This API call will return momentums by page

#### Request

One parameter of type `number` that represents the time

```json
{
  "jsonrpc": "2.0",
  "id": 33,
  "method": "ledger.getMomentumsByPage",
  "params": [0, 2]
}
```

#### Response

An array of `entries`:

* `list` of type `array`: array containing same information as [ledger.getFrontierMomentum](#ledger.getFrontierMomentum)
* `count` of type `int`: number of momentums in total

```json
{
    "jsonrpc": "2.0",
    "id": 33,
    "result": {
        "list": [
            {
                "version": 1,
                "chainIdentifier": 3,
                "hash": "cd91bd8c8cfbfb7e2b71eee2995d24a2cdec692d04f432951fe1245101d90e53",
                "previousHash": "250201abea9599da729a913491222d78604bf79f0e45f39a99b8fb9f5ada0865",
                "height": 34757,
                "timestamp": 1637602310,
                "data": "",
                "content": [],
                "changesHash": "a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a",
                "publicKey": "aBOWZfktDI3QxgMh9df0HpX2wns4mZtKrDKUIYvgK+I=",
                "signature": "EgXaAhFEmBrEgjrxG8Bvl1SvlK/sD6kAINfR8hxsq4uqfQb5Kv3OdOao4S5v2u9J5L8lH0S+rMxWyCqPgilZCA==",
                "producer": "z1qqmqp40duzvtxvg7dwxph7724mq63t3mru297p"
            },
            {
                "version": 1,
                "chainIdentifier": 3,
                "hash": "250201abea9599da729a913491222d78604bf79f0e45f39a99b8fb9f5ada0865",
                "previousHash": "5e814b9d78978e27878f0b1605739bb95ea5a1b43711d1216ee1fe29004d8e2e",
                "height": 34756,
                "timestamp": 1637602300,
                "data": "",
                "content": [],
                "changesHash": "a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a",
                "publicKey": "6zoCw5uZaS4N2eJlqGh2SmgSJbh5mJNY8aOgKLWl+D4=",
                "signature": "HJd+ZMr370tZv/hJJdCzSCKuRf6Bf7ijYPimZ6buBucBFfA4w4mQkDSZV5N1lqjFND+2KJ2+IuOdTBoiYRV1AQ==",
                "producer": "z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt"
            }
        ],
        "count": 34757
    }
}
```

### ledger.getMomentumByHash

> This API call will return the momentum with the specified hash

#### Request

One parameter of type `string` that represents the hash of the momentum

```json
{
    "jsonrpc": "2.0",
    "id": 34,
    "method": "ledger.getMomentumByHash",
    "params": ["cd91bd8c8cfbfb7e2b71eee2995d24a2cdec692d04f432951fe1245101d90e53"]
}
```

#### Response

Same information as [ledger.getFrontierMomentum](#ledger.getFrontierMomentum)

```json
{
    "jsonrpc": "2.0",
    "id": 34,
    "result": {
        "version": 1,
        "chainIdentifier": 3,
        "hash": "cd91bd8c8cfbfb7e2b71eee2995d24a2cdec692d04f432951fe1245101d90e53",
        "previousHash": "250201abea9599da729a913491222d78604bf79f0e45f39a99b8fb9f5ada0865",
        "height": 34757,
        "timestamp": 1637602310,
        "data": "",
        "content": [],
        "changesHash": "a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a",
        "publicKey": "aBOWZfktDI3QxgMh9df0HpX2wns4mZtKrDKUIYvgK+I=",
        "signature": "EgXaAhFEmBrEgjrxG8Bvl1SvlK/sD6kAINfR8hxsq4uqfQb5Kv3OdOao4S5v2u9J5L8lH0S+rMxWyCqPgilZCA==",
        "producer": "z1qqmqp40duzvtxvg7dwxph7724mq63t3mru297p"
    }
}
```

### ledger.getMomentumsByHeight

> This API call will return a list of momentums from `height` to `height + count`

#### Request

2 parameters:

* first parameter of type `number` that represents the height
* second parameter of type `number` that represents the count

```json
{
    "jsonrpc": "2.0",
    "id": 35,
    "method": "ledger.getMomentumsByHeight",
    "params": [34757, 1]
}
```

#### Response

Same information as [ledger.getMomentumsByPage](#ledger.getMomentumsByPage)

```json
{
    "jsonrpc": "2.0",
    "id": 35,
    "result": {
        "list": [
            {
                "version": 1,
                "chainIdentifier": 3,
                "hash": "cd91bd8c8cfbfb7e2b71eee2995d24a2cdec692d04f432951fe1245101d90e53",
                "previousHash": "250201abea9599da729a913491222d78604bf79f0e45f39a99b8fb9f5ada0865",
                "height": 34757,
                "timestamp": 1637602310,
                "data": "",
                "content": [],
                "changesHash": "a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a",
                "publicKey": "aBOWZfktDI3QxgMh9df0HpX2wns4mZtKrDKUIYvgK+I=",
                "signature": "EgXaAhFEmBrEgjrxG8Bvl1SvlK/sD6kAINfR8hxsq4uqfQb5Kv3OdOao4S5v2u9J5L8lH0S+rMxWyCqPgilZCA==",
                "producer": "z1qqmqp40duzvtxvg7dwxph7724mq63t3mru297p"
            }
        ],
        "count": 34803
    }
}
```

### ledger.getDetailedMomentumsByHeight

> This API call will return a list of momentums from `height` to `height + count` with information about the account blocks they contain

#### Request

2 parameters:

* first parameter of type `number` that represents the height
* second parameter of type `number` that represents the count

```json
{
    "jsonrpc": "2.0",
    "id": 36,
    "method": "ledger.getDetailedMomentumsByHeight",
    "params": [2, 1]
}
```

#### Response
`JSON` object representing the detailed momentums:

* `count` of type `number`: the total number of momentums
* an array of entries for the rewards with the following fields:
  - `blocks` of type `array` of dictionaries: contains all the blocks inserted in the momentum
  - `momentum` of type `dictionary`: same information as [ledger.getFrontierMomentum](#ledger.getFrontierMomentum)

```json
{
    "jsonrpc": "2.0",
    "id": 36,
    "result": {
        "list": [
            {
                "blocks": [],
                "momentum": {
                    "version": 1,
                    "chainIdentifier": 3,
                    "hash": "5efd0e49736f2a1ff7eeef3e3e73fbbb087471ff1097d2e41041942adccdec93",
                    "previousHash": "761f482683e6d0ed1f92af1140418b989b89c474d3491a2f4651bce99954bed6",
                    "height": 2,
                    "timestamp": 1637252770,
                    "data": "",
                    "content": [],
                    "changesHash": "a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a",
                    "publicKey": "aBOWZfktDI3QxgMh9df0HpX2wns4mZtKrDKUIYvgK+I=",
                    "signature": "B+NBitwhrRlJ6rYVMwqcjfHkJ/sz0hjwcDiLDpPUP93DtEzRZ9UvL9JxFJr5ZGophGd/jgTZvf0W8CrIGORFBw==",
                    "producer": "z1qqmqp40duzvtxvg7dwxph7724mq63t3mru297p"
                }
            }
        ],
        "count": 34818
    }
}
```

### ledger.getAccountInfoByAddress

> This API call will return information about the account-chain of the specified address

#### Request

One parameter of type `string` that represents the address

```json
{
    "jsonrpc": "2.0",
    "id": 37,
    "method": "ledger.getAccountInfoByAddress",
    "params": ["z1qz5p95pa8c6wq9pvfkg642gjv4nnaayx6vhm2w"]
}
```

#### Response

* `address` of type `string`: account-chain address
* `accountHeight` of type `number`: height of the account-chain
* `balanceInfoMap` of type `dictionary`: information about ZTS

```json
{
    "jsonrpc": "2.0",
    "id": 37,
    "result": {
        "address": "z1qz5p95pa8c6wq9pvfkg642gjv4nnaayx6vhm2w",
        "accountHeight": 0,
        "balanceInfoMap": {}
    }
}
```

## ledger.subscribe

* [subscribe.toMomentums](#subscribe.toMomentums)
* [subscribe.toAllAccountBlocks](#subscribe.toAllAccountBlocks)
* [subscribe.toAccountBlocksByAddress](#subscribe.toAccountBlocksByAddress)
* [subscribe.toUnreceivedAccountBlocksByAddress](#subscribe.toUnreceivedAccountBlocksByAddress)

### subscribe.toMomentums

#### Request

* `event` of type `string`: event to subscribe to

```json
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "ledger.subscribe",
    "params": ["momentums"]
}
```

#### Response

* `id` of type `string` that represents the subscription id

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x819156fe49dec82e0fff4ac98252f513"
}
```

### subscribe.toAllAccountBlocks

#### Request

* `event` of type `string`: event to subscribe to

```json
{
    "jsonrpc": "2.0",
    "id": 2,
    "method": "ledger.subscribe",
    "params": ["allAccountBlocks"]
}
```

#### Response

* `id` of type `string` that represents the subscription id

```json
{
  "jsonrpc": "2.0",
  "id": 2,
  "result": "0x919156fe49dec82e0fff4ac98252f513"
}
```

### subscribe.toAccountBlocksByAddress

#### Request

* `event` of type `string`: event to subscribe to
* `address` of type `string`: address of the account block

```json
{
    "jsonrpc": "2.0",
    "id": 3,
    "method": "ledger.subscribe",
    "params": ["accountBlocksByAddress", "z1qz5p95pa8c6wq9pvfkg642gjv4nnaayx6vhm2w"]
}
```

#### Response

* `id` of type `string` that represents the subscription id

```json
{
  "jsonrpc": "2.0",
  "id": 3,
  "result": "0xa19156fe49dec82e0fff4ac98252f513"
}
```

### subscribe.toUnreceivedAccountBlocksByAddress

#### Request

* `event` of type `string`: event to subscribe to
* `address` of type `string`: address of the account block

```json
{
    "jsonrpc": "2.0",
    "id": 4,
    "method": "ledger.subscribe",
    "params": ["unreceivedAccountBlocksByAddress", "z1qz5p95pa8c6wq9pvfkg642gjv4nnaayx6vhm2w"]
}
```

#### Response

* `id` of type `string` that represents the subscription id

```json
{
  "jsonrpc": "2.0",
  "id": 4,
  "result": "0xb19156fe49dec82e0fff4ac98252f513"
}
```

## Stats

* [stats.osInfo](#stats.osInfo)
* [stats.runtimeInfo](#stats.runtimeInfo)
* [stats.processInfo](#stats.processInfo)
* [stats.syncInfo](#stats.syncInfo)
* [stats.networkInfo](#stats.networkInfo)

### stats.osInfo

> This API call will return information about the os.

#### Request

No parameters

```json
{
    "jsonrpc": "2.0",
    "id": 40,
    "method": "stats.osInfo",
    "params": []
}
```

#### Response

```json
{
    "os": "linux", 
    "platform": "ubuntu", 
    "platformFamily": "debian", 
    "platformVersion": "20.10", 
    "kernelVersion": "5.8.0-41-generic", 
    "memoryTotal": "33123618816", 
    "memoryFree": "22736371712", 
    "numCPU": "16", 
    "numGoroutine": "71"   
}
```

### stats.processInfo

> This API call will return information about the process.

#### Request

No parameters

```json
{
    "jsonrpc": "2.0",
    "id": 40,
    "method": "stats.processInfo",
    "params": []
}
```

#### Response

```json
{
    "version": "v2.0.2",
    "commit": "53305d2977364c04f334c0e2838061326e119c0a"
}
```

### stats.networkInfo

> This API call will return information about the network.

#### Request

No parameters

```json
{
    "jsonrpc": "2.0",
    "id": 40,
    "method": "stats.networkInfo",
    "params": []
}
```

#### Response

```json
{
  "jsonrpc": "2.0",
  "id": 40,
  "result": {
    "numPeers": 3,
    "peers": [
      {
        "publicKey": "af636fff6e73fb56b7bbd9462e2f80ddd39d1ffa5ae08e186b1227730945aa5fec9a41120d1ab5476b41b8b8cb6d47d1c096060b2f75bc2472796279cb077025",
        "ip": "192.168.0.52"
      },
      {
        "id": "bf636fff6dafb56b7bbd9462e2f80dabc9d91ffa5ae08e186b1227730945aa5fec9a41120d1ab5476b41b8b8cb6d47d1c096060b2f75bc2472796279cb077025",
        "ip": "192.168.0.51"
      },
      {
        "id": "cf636fff6dafb56b7bbd9462e2f80ddd39d91ffa5ae08e186b1227730945cdefec9a41120d1ab5476b41b8b8cb6d47d1c096060b2f75bc2472796279cb077025",
        "ip": "192.168.0.50"
      }
    ],
    "self": {
      "publicKey": "f079ae86096ded5ee9d9a9e5c5ca26683182ba4ab34560ce44fee4c3369e88a209b2aea4ccc0b0073519ff88c08286c312f072cb0e8ec25a6ba1cd6cbfd72084",
      "ip": ""
    }
  }
}
```
