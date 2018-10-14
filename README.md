Bitcoin Private (Rebase)
=====================================

[![Build Status](https://travis-ci.com/BTCPrivate/BTCP-Rebase.svg?branch=master)](https://travis-ci.com/BTCPrivate/BTCP-Rebase)

IN DEVELOPMENT - FOR TESTING ONLY - DO NOT USE IN PRODUCTION
===========
This is currently in development, not all consensus rules have been implemented. **You should NOT rely on this in production yet**, for production use cases please use https://github.com/BTCPrivate/BitcoinPrivate/

What is the Bitcoin Private Rebase?
----------------

Bitcoin Private is currently based primarily on the Zcash codebase, which itself is a snapshot of Bitcoin dating to mid-2015. Most z protocol coins all rely on this same code, which is now missing roughly 3 years of blockchain upgrades that have been integrated into Bitcoin. Based on the structure of Zcash, and because there’s now a 3 year code divergence, integrating blockchain upgrades developed in bitcoin core into any z protocol coin has become increasingly cumbersome.

We endeavor a complete refactoring of Bitcoin Private to allow for more facile incorporation of Bitcoin upstream changes. This release will coincide with activation of full SegWit support, giving the BTCP more scale both on chain as well as enabling off-chain protocols such as the lightning network and cross-chain swaps. Additionally, it will bring another 3 years of upgrades to the core node including stability and speed improvements as well as a number of usability enhancements to the reference wallet including support for hierarchical deterministic wallets (for shielded and transparent addresses) and wallet encryption.

Learn more about Bitcoin Private and the rebase at https://btcprivate.org.

This README provides build and sync BTCP mainnet chain instructions for Linux and Mac, plus notes on config and usage to cover most deployment scenarios. Please note again that wallet code and test suite are not fully working yet, details of this below.

Build Instructions
-------

For any build, make sure your server/machine is up to date (eg, on Linux has run `sudo apt-get update && sudo apt-get upgrade`), trying to build otherwise may result in errors.

#### Linux (>= Ubuntu 16.04):
```
cd scripts
./build.sh
```

#### MacOS (High Sierra):
You must first install libomp (Apple's version of Clang does not have support for OpenMP included); you can do that via the following Terminal commands:

Install XCode:
```
xcode-select --install
```
Install Homebrew:
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
Install libomp:
```
brew install libomp
```

Then, run `scripts/build-mac.sh` to compile.

Build Options
------

There are few build options you can also use:
- `--disable-wallet`
- `--without-gui`
- `--help`

ZCash Ceremony
------

Towards the end of the build it will inform you that you haven't yet added the ZCash ceremony and provide the command to use to download and apply this. It also notes the extra `-testnet` param to use with this if you wish to run a testnet rather than mainnet.

Config
------

You can add parameters such as `-testnet` and `-daemon` when running the Bitcoin Private daemon, however, you may want to add these (eg `testnet=1` and `daemon=1`) to the config file in `btcp.conf` so there is no need to add as parameters on the command line each time you run the daemon and a useful place to add nodes using `addnode=<node ip>`

Networks
------

There are 3 main networks:
- `regtest` provides a self contained testnet for the server/machine you are running on at present, ideal if you don't need to connect to external nodes.
- `testnet` is the testnet but we allow external nodes to connect to this node and form part of a network.
- `mainnet` is the main network ready for production use.

Ports
------
If you wish to run the daemon under `testnet` or `mainnet` chains, you may need to open up your firewall rules on the server/machine (also check any firewalls your hosting provider may have in place). The ports are:

**testnet**
- 17932 = rpc port
- 17933 = p2p port

**mainnet**
- 7932 = rpc port
- 7933 = p2p port

You can open the relevant port up using `sudo ufw allow <num>/tcp`

Running the Daemon
------

Whatever parameters you decide to apply when running your blockchain, you'll need to start your node using:
`./src/bitcoind`

Using the CLI RPC methods
------

To use RPC methods via the CLI, you can call upon this (and pass `help` to it for a list of all RPC methods):
`./src/bitcoin-cli`

Testing
------

There are many tests available from within the `tests` dir to help validate that everything works as expected, necessary if you make changes to this repos codebase or wish to confirm something is working correctly.

The IP for the official BTCP testnet you can add as a node is **209.97.137.47** plus there is also a web interface at https://testnet.btcprivate.org if you wish to try some RPC commands there as a quick test. 

Testnet BTCP coins
------

You can mine Bitcoin Private in order to generate coins to perform transactions with, but there is also a faucet repo at https://github.com/ch4ot1c/zfaucet where you can get BTCP also. 

Low RAM Servers/Machines
------

Due to the nature of the blockchain, it can be a little memory intensive when building or mining (for both, 1GB RAM and a reasonly good CPU should be fine), but a solution where that isn't available is to use swapspace when you run out of RAM:

```
cd /
sudo dd if=/dev/zero of=swapfile bs=1M count=$1
sudo mkswap swapfile
sudo chmod 0600 /swapfile
sudo swapon swapfile
echo "/swapfile none swap sw 0 0" | sudo tee -a etc/fstab > /dev/null
cd ~
```

Notes
-------

The build typically takes 30-45 minutes depending on your server/machine. There will be a significant slowdown in blocks being validated from 272991 for around 5000 blocks, as these are far larger blocks created around the time of the snapshot and far greater processing is needed on these.

Bug Reports
-------

We would greatly appreciate any bug reports and development to help further improve and fix any issues on this codebase. In all cases it is best to raise a GitHub issue for bugs and put in a pull request for any fixes you provide. We can review and discuss changes before merging into master branch. 

License
-------

Bitcoin Private is released under the terms of the MIT license. See [COPYING](COPYING) for more
information or see https://opensource.org/licenses/MIT.
