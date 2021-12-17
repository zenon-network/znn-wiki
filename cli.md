# Zenon CLI tool

## Basic usage

```bash
./znn-cli --help
#will print all the available commands.
```

```bash
USAGE:
  znn-cli [OPTIONS] [FLAGS]

FLAGS:
-u, --url           Provide a websocket znnd connection URL with a port
                    (defaults to "ws://127.0.0.1:35998")
-p, --passphrase    use this passphrase for the keyStore or enter it manually in a secure way
-k, --keyStore      Select the local keyStore
                    (defaults to "available keyStore if only one is present")
-i, --index         Address index
                    (defaults to "0")
-v, --verbose       Prints detailed information about the action that it performs
-h, --help          Displays help information

OPTIONS:
  General
    send toAddress amount [ZNN/QSR/ZTS]
    receive blockHash
    receiveAll
    unreceived
    unconfirmed
    balance
    frontierMomentum
    version
  Plasma
    plasma.list [pageIndex pageCount]
    plasma.get
    plasma.fuse toAddress amount (in QSR)
    plasma.cancel id
  Sentinel
    sentinel.list
    sentinel.register
    sentinel.revoke
    sentinel.collect
    sentinel.withdrawQsr
  Staking
    stake.list [pageIndex pageCount]
    stake.register amount duration (in months)
    stake.revoke id
    stake.collect
  Pillar
    pillar.list
    pillar.register name producerAddress rewardAddress giveBlockRewardPercentage giveDelegateRewardPercentage
    pillar.revoke name
    pillar.delegate name
    pillar.undelegate
    pillar.collect
    pillar.withdrawQsr
  ZTS Tokens
    token.list [pageIndex pageCount]
    token.getByStandard tokenStandard
    token.getByOwner ownerAddress
    token.issue name symbol domain totalSupply maxSupply decimals isMintable isBurnable isUtility
    token.mint tokenStandard amount receiveAddress
    token.burn tokenStandard amount
    token.transferOwnership tokenStandard newOwnerAddress
    token.disableMint tokenStandard
  Wallet
    wallet.list
    wallet.createNew passphrase [keyStoreName]
    wallet.createFromMnemonic "mnemonic" passphrase [keyStoreName]
    wallet.dumpMnemonic
    wallet.deriveAddresses start end
    wallet.export filePath
```

### Check the version

```bash
./znn-cli version

znn-cli v0.0.1 using Zenon SDK v0.0.1
znnd version v0.0.1
```

### Send and receive funds

The NoM architecture features a block-lattice comprised of individual account-chains. When you issue a transaction, a *send block* will be created and signed by your private key and will also be broadcasted to the network. The transaction can be considered final after it is accepted by the network and embedded into several momentums. For the recipient to use the funds, he will need to create a corresponding *receive block* and properly sign it to complete the transaction.

- Use the following command to send `amount` of `ZNN` to an address `toAddress`. Similarly, you can use the command above to send `QSR`:

```bash
./znn-cli send z1qqjr86g6220kmhn2jrelwhtk7u0mp4r5amjwf2 10 znn --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Sending 10.00000000 znn to z1qqjr86g6220kmhn2jrelwhtk7u0mp4r5amjwf2
Done
```

- Use this command to receive all pending transactions:

```bash
./znn-cli receiveAll --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase 
```

- You will need to wait for Plasma generation via PoW:

```bash
You have 3 transaction(s) to receive
Please wait ...
Done
```

- Use this command to receive a specific transaction by the corresponding blockHash:

```bash
./znn-cli receive a994b94ae93b683833ddd3ff1fbf53fe918d4f7b93326e7875ab13eead2c5ee2 --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Please wait ...
Done
```

### Check your balance

- Use the following command to print the balance of your wallet from a particular `keyStore`:

```bash
./znn-cli balance --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Balance for account-chain z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x having height 2
  150000.00000000 QSR zenon.network zts1qsrxxxxxxxxxxxxxmrhjll
  15000.00000000 ZNN zenon.network zts1znnxxxxxxxxxxxxx9z4ulx
```

### Get info about unreceived transactions

- Use the following command to print info about the unreceived transactions from a particular `keyStore`:

```bash
./znn-cli  unreceived --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Nothing to receive
```

### Get info about unconfirmed transactions

- Use the following command to print info about the unconfirmed transactions from a particular `keyStore`:

```bash
./znn-cli  unconfirmed --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

No unconfirmed transactions
```

### Get info about the frontier momentum

- Use the following command to print info about the frontier momentum (latest momentum):

```bash
./znn-cli frontierMomentum

Momentum height: 32202
Momentum hash: 528ac51f9368930b4b07096f0aa45ccaf42a3fd7ae7e657e0233fe1942c0f707
Momentum previousHash: f5dada224e42dbc17094ac1a3debf748d17523ce23e3ba41558896682046a70d
Momentum timestamp: 1616284800
```

### Plasma commands

Plasma is required to perform actions on the network. You can either obtain it via a PoW computation or fuse `QSR` to generate it. Sending and receiving funds, calling embedded smart contracts require Plasma.

- If you don't want to wait for Plasma generation via PoW and you have a `QSR` balance, use this command to fuse `qsrAmount` of `QSR` for `toAddress` address:

```bash
./znn-cli plasma.fuse z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x 5000 --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Fusing 5000.00000000 QSR to z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x
Done
```

- Use this command to print all the fusion entries for your wallet:

```bash
./znn-cli plasma.list --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase
```

- If there are Plasma entries, it will print them with their corresponding IDs, so you can cancel the Plasma generation:

```bash
Fusing 10010.00000000 QSR for Plasma in 2 entries
  10000.00000000 QSR for z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt
Can be canceled at momentum height: 1. Use id 1dbd7d0b561a41d23c2a469ad42fbd70d5438bae826f6fd607413190c37c363b to cancel
  10.00000000 QSR for z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt
Can be canceled at momentum height: 35810. Use id eb02eb1e0f51a70b3d622d73d4b23f04710c33024593114ea12285d88c05dc61 to cancel
```

- Use this command to cancel the Plasma fusion based on the `id` provided by `plasma.list` command:

```bash
./znn-cli plasma.cancel eb02eb1e0f51a70b3d622d73d4b23f04710c33024593114ea12285d88c05dc61 --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Canceling Plasma fusion with id eb02eb1e0f51a70b3d622d73d4b23f04710c33024593114ea12285d88c05dc61
Done
```

- Use this command to get the current Plasma for the specified address in cli:

```bash
./znn-cli plasma.get --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x has 10 / 10 plasma with 10.00000000 QSR fused.
```

## Staking commands

Staking is the process that uses `ZNN` to produce `QSR`.

- Use this to create a new stake of `amount` for `duration` months

```bash
./znn-cli stake.register 100 2 --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Staking 100.00000000 ZNN for 2 month(s)
Done
```

- Use this command to print all the stake entries for an address:

```bash
./znn-cli stake.list --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Showing 1 out of a total of 1 staking entries
Stake id 7d43942c01cb79bc5db804c80b76310b379b41164bb71c21b1706d0f10257346 with amount 100.00000000 ZNN
    Can be revoked in 1439:59:05
```

- Use this command to revoke a stake, if it's due. Do not worry, you'll still receive some rewards based on when you revoked your stake entry.

```bash
./znn-cli stake.revoke efdb1e4d9bf93352aceff151465cd12245e03c51bef472292e1ddec9da73b61c --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Can`t revoke! Try again in 47:52:48
```

- After the staking period ended you can revoke the stake:

```bash
./znn-cli stake.revoke efdb1e4d9bf93352aceff151465cd12245e03c51bef472292e1ddec9da73b61c --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Done
Use receiveAll to collect your stake amount and uncollected reward(s) after 2 momentums
```

- If you want to collect the `QSR` staking rewards from all your stakes:

```bash
./znn-cli stake.collect --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Done
Use receiveAll to collect your stake reward(s) after 1 momentum
```

## Sentinel commands

A Sentinel Nodes requires a fixed deposit of `50000 QSR` for the Sentinel slot and `5000 ZNN` and can be deployed using the `znn-controller`. When you use `sentinel.register`, it will deposit the `QSR` and then call the smart contract while also sending `5000 ZNN`. Use `znn-controller` to deploy the Sentinel.

- Use this to register a Sentinel. It will deposit `QSR` and call the smart contract. Will fail if you don't have the required amount of `ZNN` or `QSR`

```bash
./znn-cli sentinel.register --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

You have 0 QSR deposited for the Sentinel
Done
Check after 2 momentums if the Sentinel was successfully registered using sentinel.list command
```

- Now you will receive the rewards here: `z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x`

- Use the `sentinel.list` command to check your Sentinel(s):

```bash
./znn-cli sentinel.list --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Revocation window will open in 647:59:20
```

- Use this to revoke a Sentinel:

```bash
./znn-cli sentinel.revoke --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase
```

- You need to wait for the revocation window to open in order to revoke the Sentinel

```bash
Cannot revoke Sentinel. Revocation window will open in 647:57:40
```

- When the revocation window is open you can revoke the Sentinel and collect back the locked `QSR` from the slot and `ZNN`:

```bash
Done
Use receiveAll to collect back the locked amount of ZNN and QSR
```

- If you want to collect the `QSR` staking rewards from your sentinel:

```bash
./znn-cli sentinel.collect --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Done
Use receiveAll to collect your sentinel reward(s) after 1 momentum
```

## Pillar commands

A Pillar Node requires a dynamic deposit of `150000 QSR` (base amount + `10000 QSR` extra per additional Pillar slot) that will be burned to create the Pillar Slot and a locked amount of `15000 ZNN`.

- Use this command to retrieve all the Pillars from the network:

```bash
./znn-cli pillar.list --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

#1 Pillar VPS_3 has a delegated weight of 100050.00000000 ZNN
    Producer address z1qprpnyv6xmc4mu5d405jgxjqfd79ggf8fjdewr
    Momentums 280 / expected 290
#2 Pillar VPS_2 has a delegated weight of 100000.00000000 ZNN
    Producer address z1qqmqp40duzvtxvg7dwxph7724mq63t3mru297p
    Momentums 282 / expected 290
#3 Pillar VPS_1 has a delegated weight of 98443.00000000 ZNN
    Producer address z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt
    Momentums 279 / expected 290
```

**Important notice:** It is recommended that you input a separate address from a different `keyStore` for the `produceAddress`, because when you deploy it on the VPS/remote machine it will get exposed. This is a mandatory step designed to protect your main `keyStore` from where you register the Pillar.

- Deposit `QSR` and create a Pillar Slot to register the Pillar named `myAwesomePillar` that will produce momentums from `producerAddress` and collect the rewards in `rewardAddress`, with the following reward percentages: `giveBlockRewardPercentage` and `giveDelegateRewardPercentage`:

```bash
./znn-cli pillar.register myAwesomePillar z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt z1qprpnyv6xmc4mu5d405jgxjqfd79ggf8fjdewr 2 3 --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase 
```

- Before completing the Pillar registration operation, it must be confirmed:

```bash
Creating a new Pillar will burn the deposited QSR required for the Pillar slot
Do you want to proceed? (y/N):
```

- After registering the Pillar, in order to have a functional Pillar that is producing momentums, one should use the `znn-controller` in order to configure the registered Pillar.

```bash
Registering Pillar ...
Done
Check after 2 momentums if the Pillar was successfully registered using pillar.list command
```

- If you want to collect the `QSR` staking rewards from your pillar:

```bash
./znn-cli pillar.collect --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase
```

- Revoke a pillar by `name`:

```bash
./znn-cli pillar.revoke myAwesomePillar --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase
```

- If a Pillar with the indicated name is registered, it will be revoked:

```bash
Revoking Pillar myAwesomePillar ...
Use receiveAll to collect back the locked amount of ZNN
Done
```

## Delegation commands

- Delegate *ALL* your `ZNN` balance to the Pillar called `myAwesomePillar`:

```bash
./znn-cli pillar.delegate myAwesomePillar --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Delegating to pillar myAwesomePillar ...
Done
```

- You can use `pillar.list` command to see that you indeed delegated the ZNN to the designated Pillar, increasing the Pillar's total delegated weight:

```bash
./znn-cli pillar.list --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

#1 Pillar myAwesomePillar has a delegated weight of 94900.00000000 ZNN
    Producer address z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt
    Momentums 2224 / expected 2230
```

- Undelegate *ALL* your `ZNN` balance from the Pillar you delegated them to:

```bash
./znn-cli pillar.undelegate --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Undelegating ...
Done
```

## Wallet commands

The updated `wallet.dat` format is called a `keyStore`. In order to create a `keyStore` you need a passphrase that will be used for encryption.

- Use this command to create a new wallet using a `passphrase`. Please store the passphrase *offline* in a *secure location*.

```bash
./znn-cli wallet.createNew yourComplexPassphrase

keyStore successfully created: z1qqde9hspn3p9u3es4fzy0h9m242rxe2wpjxqgq
```

- Use this command to restore a `keyStore` from a `mnemonic` and the corresponding `passphrase`:

```bash
./znn-cli wallet.createFromMnemonic "test blood draw trade body item chef electric rural possible letter staff" yourComplexPassphrase

keyStore successfully created from mnemonic: z1qqlfqlffxcd8er5fujxfgerzvds9rstn0af0tr
```

- Use this command to dump the mnemonic of a `keyStore` using the corresponding `passphrase`:

```bash
./znn-cli wallet.dumpMnemonic --keyStore z1qqlfqlffxcd8er5fujxfgerzvds9rstn0af0tr --passphrase yourComplexPassphrase

Mnemonic for keyStore File: '~/.znn/wallet/z1qqlfqlffxcd8er5fujxfgerzvds9rstn0af0tr'
test blood draw trade body item chef electric rural possible letter staff
```

- Use this command to list all the wallet files i.e. `keyStores` from the default wallet folder:

```bash
./znn-cli wallet.list

Available keyStores:
zc144fb31ae4ff9b78ef5f01d931ac1683e8cca21c25a5e9d2d4dcc0ac93
```

- Use this command to derive new key pairs from a decrypted wallet `keyStore`, starting from `left` index to `right` index

```bash
./znn-cli wallet.deriveAddresses 0 3 --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Addresses for keyStore File: '~/.znn/wallet/z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x'
  0	z1qph8dkja68pg3g6j4spwk9re0kjdkul0amwqnt
  1	z1qz0tppwyavmge8vg6qvfx9dkus6rynaf3h747l
  2	z1qqk5q0vmxdaylyem399thvwcnt4wn7wpmxz87w
```

## ZTS commands

The Alphanet implements the new ZTS for all tokens created on the network. One can issue, mint, burn or transfer ownership for tokens with a set of simple commands described below.

- Use this command to list all tokens available on the network:

```bash
./znn-cli token.list

Coin QuasarCoin with symbol QSR and standard zts1qsrxxxxxxxxxxxxxmrhjll
   Created by z1qxemdeddedxstakexxxxxxxxxxxxxxxxjv8v62
   Coin QuasarCoin has 8 decimals, is mintable, can be burned, and is a utility coin
   The total supply is 30003055000.00000000 and the maximum supply is 46116860184.27388000
   Domain `zenon.network`
Coin ZenonCoin with symbol ZNN and standard zts1znnxxxxxxxxxxxxx9z4ulx
   Created by z1qxemdeddedxpyllarxxxxxxxxxxxxxxxsy3fmg
   Coin ZenonCoin has 8 decimals, is mintable, can be burned, and is a utility coin
   The total supply is 30000380616.00000000 and the maximum supply is 46116860184.27388000
   Domain `zenon.network`
...
```

- Use this command to display information about a token using its ZTS id:

```bash
./znn-cli token.getByStandard zts1znnxxxxxxxxxxxxx9z4ulx

Coin ZenonCoin with symbol ZNN and standard zts1znnxxxxxxxxxxxxx9z4ulx
   Created by z1qxemdeddedxpyllarxxxxxxxxxxxxxxxsy3fmg
   The total supply is 30000380616.00000000 and a maximum supply is 46116860184.27388000
   The token has 8 decimals can be minted and can be burned
```

- Use this command to display information about a token using its owner address:

```bash
./znn-cli token.getByOwner z1qxemdeddedxpyllarxxxxxxxxxxxxxxxsy3fmg

Coin ZenonCoin with symbol ZNN and standard zts1znnxxxxxxxxxxxxx9z4ulx
   Created by z1qxemdeddedxpyllarxxxxxxxxxxxxxxxsy3fmg
   The total supply is 30000380616.00000000 and a maximum supply is 46116860184.27388000
   The token has 8 decimals can be minted and can be burned
```

- Use this command to issue a token; keep in mind that issuing a new token burns `1 ZNN`:

```bash
./znn-cli token.issue "ZenonWikiToken" "ZWT" "domain.com" "1234567890" "123456789000" 8 1 1 1 -k zd967721f683287575ad410da8ee4a021f9e18f02651007ae3922f08bfc24 -i 0 -p yourComplexPassphrase 

Issuing a new ZTS token will burn 1 ZNN
Do you want to proceed? (y/N): y
Issuing ZenonWikiToken ZTS token ...
Done
```

- Use this command to mint:

```bash
./znn-cli token.mint zts17ejv0drss06n5qz7ccad24 123456789 zd967721f683287575ad410da8ee4a021f9e18f02651007ae3922f08bfc24 -k zd967721f683287575ad410da8ee4a021f9e18f02651007ae3922f08bfc24 -i 0 -p yourComplexPassphrase 

Minting ZTS token ...
Done
```

- Use this command to burn:

```bash
./znn-cli token.burn zts17ejv0drss06n5qz7ccad24 1000000000 -k zd967721f683287575ad410da8ee4a021f9e18f02651007ae3922f08bfc24 -i 0 -p yourComplexPassphrase 

Burning zts17ejv0drss06n5qz7ccad24 ZTS token ...
Done
```

- Use this command to transfer the ownership of a token:

```bash
./znn-cli token.transferOwnership zts17ejv0drss06n5qz7ccad24 z3a775d873fa847502bc1f94e2d023103dacce8ad0339c87270ac7d160070 -k zd967721f683287575ad410da8ee4a021f9e18f02651007ae3922f08bfc24 -i 0 -p yourComplexPassphrase

Transferring ZTS token ownership ...
Done
```

- Use this command to disable minting and burning for a token:

```bash
./znn-cli token.disableMint zts17ejv0drss06n5qz7ccad24 -k zd967721f683287575ad410da8ee4a021f9e18f02651007ae3922f08bfc24 -i 0 -p yourComplexPassphrase

Disabling ZTS token mintable flag ...
Done
```
