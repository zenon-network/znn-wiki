# Alphanet Swap Cycles

To prevent swap apathy, a novel Swap Decay mechanism is introduced in order to stimulate legacy network participants to transition to Alphanet as fast as possible. There are three main reasons for implementing the Swap Decay mechanism:

- Penalizes the swap apathy that can lead to a prolonged migration time, harming adoption, the stability of the network, and growth of the new ecosystem
- Strengthens the community by leveraging the cooperation of technical and non-technical users
- Enhances decentralization by encouraging the developers from the community to extend the capabilities of the swap library

## Swap Decay mechanism

The swap process will be available for 1 year (12 months), starting from the embedded timestamp found in the genesis momentum. There will be a total of 10 Swap Cycles.

The first cycle will last for 3 months with a Swap Ratio of 1:1. The next cycles until the end of the swap process will last 1 month each. Starting with Swap Cycle 2, the Swap Ratio will progressively decrease each cycle by 10% until it reaches 0. After the last swap cycle is completed, it won’t be possible to swap coins from the legacy network anymore.

The period for the first cycle will be three times longer compared to the rest of the cycles in order to provide legacy network participants sufficient time to accommodate and complete the swap without any penalizations.

## Swap Cycle examples

- Example Swap Cycle 1 (first three months)

```
Legacy network: 100 ZNN, 10 QSR => Alphanet: 100 ZNN, 10 QSR
```

- Example Swap Cycle 10 (month 12)

```
Legacy network: 100 ZNN, 10 QSR => Alphanet: 10 ZNN, 1 QSR
```

## How to swap?

Everyone, regardless of technical knowledge or expertise will be able to perform the swap: the recommended way to perform the swap is using the Syrius wallet.

- Wallet GUI (Syrius)

Download the latest version of `s y r i u s` wallet, navigate to `Settings -> Wallet Options -> Start Swap` and follow the steps to swap funds from the legacy network to Alphanet.

- Other swap tools can be developed based on the `znn_swap_utility` library.

## Risks

Please take extra measures of precaution when dealing with private keys. Beware of scams and phishing attempts and stay vigilant.
