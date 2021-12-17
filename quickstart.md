# Quickstart

The Zenon Alphanet binaries are available for Linux, MacOS and Windows `x86_64` CPU architecture operating systems.

## Linux Tutorial

`Ubuntu 20.04 LTS` was used for this tutorial.

## 1. Download the binaries

Download `znnd`, `znn-cli` and `znn-controller` from their respective GitHub repos. Please note that `znn-controller` is only available for Linux at the moment.

## 2. Verification (deprecated)

**Import the Zenon Testnet PGP key from `hkps.pool.sks-keyservers.net`**

```bash
gpg --keyserver hkps.pool.sks-keyservers.net --recv E2D8289C0A9D2C28A3320B62D2AAFD5D9088F1A2
```

If you don't have `gpg` installed, please use `sudo apt install gnupg`

```bash
gpg: key E2D8289C0A9D2C28A3320B62D2AAFD5D9088F1A2: public key "Zenon Testnet <portal@zenon.network>"
```

Verify the signature. The same command can be used on MacOS:

```bash
gpg --verify `path_to_sig`.sig `path_to_release_bundle`

gpg: Good signature from "Zenon Testnet <portal@zenon.network>"
```

## 3. Start the Node daemon

If you are on a Linux machine, use the `znn-controller` option `1) Deploy`:

```bash
./znn-controller
```

**Otherwise use:**

```bash
./znnd
```

## 4. Node RPC communication is enabled by default

Inspect the `config.json` file using your favorite text editor.

Navigate to `~/.znn/config.json`. Check if the `http`, `websocket` entries are enabled (`true`). By default, all `Endpoints` are public. The default `HTTPPort` is `35997` and `WSPort` is `35998`.

```json
{
  "RPC": {
    "EnableHTTP": true,
    "EnableWS": true,
    "Endpoints": null
  }
}
```

**You can also disable the RPC communication via `znn-cli`:**

```bash
./znn-cli disableRPC

RPC successfully disabled
Start znnd to use the new configuration
```

## 5. [optional] Generate a ZNN Alphanet address

```bash
./znn-cli wallet.createNew yourComplexPassphrase

Successfully created keystore: z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x
```

## 6. [optional] Check for pending funds

```bash
./znn-cli unreceived --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase
```

If you have pending funds you should see messages like these:

```bash
Unreceived 15000.00000000 ZNN from z1qqjr86g6220kmhn2jrelwhtk7u0mp4r5amjwf2.
Use hash b3e51142119174f8af3ea793a7353ec8b5b4e2208f9ebe50549ead5440fd6b49 to receive.

Unreceived 15000.00000000 QSR from z1qqjr86g6220kmhn2jrelwhtk7u0mp4r5amjwf2.
Use hash c68c674c81d4d9abc2b7a20c999352c21a8128033f9b34105b3616119f116e3c to receive.
```

## 7. [optional] Receive the pending funds

In order to receive the ZNN you need to issue the command `receive` with the corresponding hash:

```bash
./znn-cli receive b3e51142119174f8af3ea793a7353ec8b5b4e2208f9ebe50549ead5440fd6b49 --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase
```

If you don't have Plasma, you'll need to generate it via PoW, please wait.

```bash
Creating receive transaction ...
Generating Plasma, please wait ...
Done
```

Similarly, you'll receive the QSR by issuing the command `receive` with the corresponding hash:

```bash
./znn-cli receive c68c...6e3c --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Creating receive transaction ...
Generating Plasma, please wait ...
Done
```

## 8. [optional] Check your balance

```bash
./znn-cli balance --keyStore z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x --passphrase yourComplexPassphrase

Balance for account-chain z1qz8tylu88et6ffy227pw8gak5qvn5awg35l96x having height 2
150000.00000000 QSR
15000.00000000 ZNN
```

## 9. Deploy Pillar, Sentinel or full node

Download `znn-controller` to deploy a Sentinel, a Pillar or a full node.

```bash
./znn-controller
```

Follow the instructions to deploy a [Sentinel](deploy.md#Sentinel_deployment) or a [Pillar](deploy.md#Pillar_deployment).

## 10. Stop the Node daemon

**You can use the stop command from `znn-cli` if `znnd` is located at the same working directory:**

```bash
./znn-cli stop znnd
```

**If you deployed via `znn-controller` please use option 4 (Stop service)**

**Otherwise use:**

```bash
pkill -9 znnd
```
