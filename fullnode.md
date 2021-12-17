# Zenon Full Node tutorial

There are several reasons to run your own full node:

- Security: trustlessly validate the dual-ledger and attest your `ZNN`, `QSR` and `ZTS` tokens holdings
- Decentralization: more full nodes means enhanced connectivity for everyone and better geographical distribution
- Privacy: querying a remote node can have serious privacy implications such as IP and metadata leaking
- Censorship resistance: your transactions will never get dropped or censored
- No downtime: you control the full node

Another benefit for running a full node is that you can use it as a reliable data source for your wallet.

Check the tutorial to start operating your own `znnd` full node.

- Open the `s y r i u s` wallet and navigate to Node Management -> Node selection -> Choose `127.0.0.1:35998`
- `znn-cli` tool defaults to `127.0.0.1:35998`

## 1. [Optional] Build from source

The minimum requirements to run a Zenon full node can be found [here](requirements.md).

Requirements: `Go >=1.16` and `git` installed on your system.

Install `git` using your favourite package manager:

- Linux: `sudo apt-get install git`
- MacOS: `brew install git`
- Windows: `choco install git`

In order to install `Go >=1.16`, follow this [link](https://go.dev/doc/install).

Open a terminal window and clone the `go-zenon` repo:

```bash
cd ~
git clone https://github.com/zenon-network/go-zenon.git
cd go-zenon/
```

Build the full node:

```bash
make znnd
```

Navigate to the `build/` directory:

```bash
cd build/
```

[Optional] Set the execute permission (Linux/MacOS):

```bash
sudo chmod +x znnd
```

Run the node:

```bash
./znnd
```

Windows:

```bash
./znnd.exe
```

## 2. Download the precompiled binaries form Github

Go to `https://github.com/zenon-network/` and download the bundle.

## 3. Unzip and run the full node

Unzip using `unzip` or other compatible program and run the node:

Linux/MacOS:

```bash
./znnd
```

Windows:

```bash
./znnd.exe
```
