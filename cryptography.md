# Cryptography

## Wallet

The wallet data is stored inside a `KeyStore`

`AES-256-GCM` for symmetric encryption

`Argon2.IDkey` as Key Derivation Function

* time: `1`

* memory: `64 * 1024`

* threads: `4`

* key_length: `32`

An encrypted `KeyStore` is referred to as `KeyVault`.

### KeyVault example

```json
{
  "baseAddress":"z1qqjnwjjpnue8xmmpanz6csze6tcmtzzdtfsww7",
  "crypto":{
    "argon2Params": {
      "salt":"0x1563f86f7afa59dd0be150ffeac10896"
    },
    "cipherData":"0xe81dbb96f5b9839f3741ceff8362e1ebf09e05a8311354658ddf979577ffd0dd34df55757d1fd37e677e3ae7d055b2ed",
    "cipherName":"aes-256-gcm",
    "kdf":"argon2.IDKey",
    "nonce":"0xb7cc39120d5b87a3b7af58b7"
  },
  "timestamp":1633548746,
  "version":1
}
```

___

## Digital Signing Algorithm

`Ed25519` is a deterministic signature scheme using `curve25519`

## Key derivation

`Ed25519` which is compatible with `BIP44`

* key parameter: `ed25519 seed`
* hardened offset: `0x80000000`

### BIP44 format

`m / purpose' / coin_type' / account' / change / address_index`

Alphanet (Network of Momentum Phase 0) uses `coin_type` value `73404`
___

## Alphanet Address Format

`bech32`

* human readable part: `z`

`data`

* user account prefix byte: `0`

* embedded contract prefix byte: `1`

* core: first `19` bytes of `sha3(pubKey)`

address: `bech32(hrp, data)`

### Example

#### Entropy

```bash
bc827d0a00a72354dce4c44a59485288500b49382f9ba88a016351787b7b15ca
```

#### Mnemonic

```bash
route become dream access impulse price inform obtain engage ski believe awful absent pig thing vibrant possible exotic flee pepper marble rural fire fancy
```

#### BIP44 path

```bash
m/44'/73404'/0'
```

#### Private key

```bash
d6b01f96b566d7df9b5b53b1971e4baeb74cc64167a9843f82d04b2194ca4863
```

#### Public key

```bash
3e13d7238d0e768a567dce84b54915f2323f2dcd0ef9a716d9c61abed631ba10
```

#### User prefix byte

```bash
00
```

#### Core bytes

```bash
25374a419f32736f61ecc5ac4059d2f1b5884d
```

#### Address

```bash
z1qqjnwjjpnue8xmmpanz6csze6tcmtzzdtfsww7
```

### Zenon Dart SDK example

#### Code

```javascript
import 'package:hex/hex.dart';
import 'package:znn_sdk_dart/znn_sdk_dart.dart';

Future<void> main() async {
  final mnemonic =
      'route become dream access impulse price inform obtain engage ski believe awful absent pig thing vibrant possible exotic flee pepper marble rural fire fancy';

  var keyStore = KeyStore.fromMnemonic(mnemonic);
  var keyPair = keyStore.getKeyPair(0);
  var privateKey = keyPair.getPrivateKey();
  var publicKey = await keyPair.getPublicKey();
  var address = await keyPair.address;

  print('Entropy: \n${keyStore.entropy}\n');
  print('Mnemonic: \n${keyStore.mnemonic}\n');
  print('BIP44 path: \n${Derivation.getDerivationAccount(0)}\n');
  print('Private key: \n${HEX.encode(privateKey!)}\n');
  print('Public key: \n${HEX.encode(publicKey!)}\n');
  print('User & Core bytes: \n${HEX.encode(address!.core!)}\n');
  print('Address: \n$address');
}
```

#### Output

```bash
Entropy:
bc827d0a00a72354dce4c44a59485288500b49382f9ba88a016351787b7b15ca

Mnemonic:
route become dream access impulse price inform obtain engage ski believe awful absent pig thing vibrant possible exotic flee pepper marble rural fire fancy

BIP44 path:
m/44'/73404'/0'

Private key:
d6b01f96b566d7df9b5b53b1971e4baeb74cc64167a9843f82d04b2194ca4863

Public key:
3e13d7238d0e768a567dce84b54915f2323f2dcd0ef9a716d9c61abed631ba10

User & Core bytes:
0025374a419f32736f61ecc5ac4059d2f1b5884d

Address:
z1qqjnwjjpnue8xmmpanz6csze6tcmtzzdtfsww7
```
