# Node Deployment Tutorials

## Pillar

For the Pillar deployment process one will need access to both a local and a remote machine. The local machine is represented by the controller wallet, where you create the Pillar Slot and register the Pillar on the network. The remote machine is usually represented by a VPS with some minimum recommended specs and a public IP that is actively participating in the consensus protocol.

You will need to generate 3 distinct addresses for the Pillar creation tutorial:

* `pillarAddress` = address from which the Pillar is registered that contains the necessary funds (15000 ZNN that will be locked and can be recovered after you disassemble the Pillar and 150000 QSR + additional 10000 QSR for every new Pillar Slot in the network that will be burned and cannot be recovered)
* `pillarRewardsAddress` = address used to collect the Pillar rewards
* `producerAddress` = address used to produce the momentums that is stored on the remote machine; must be generated from a different seed than your local wallet keyStore for security purposes

## Sentinel

The Sentinel deployment process is similar to the Pillar deployment process.

### Controller wallet

The latest version of the `s y r i u s` wallet is recommended to be used as controller wallet. Advanced users can use the `znn-cli` instead.

### Remote machine

These commands should be issued from your remote machine (e.g. VPS). If you won't use a VPS provider, you will need an
additional setup including enabling port forwarding. This tutorial is intended for a VPS with some [minimum recommended specs](requirements.md) and a public IP.

The `znn-controller` works only for `Pillars` for the moment. Sentinel support will be available after Alphanet launch.

### Get the `znn-controller` from GitHub

The `znn-controller` is only compatible with Linux OS x64.

### Run `znn-controller` as root

```bash
sudo ./znn-controller
```

After that follow the instructions from the terminal in order to setup the Sentinel, Pillar or full node.
