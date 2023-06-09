## Liquid Staking Module Workshop 2023-06-03

Video recording of this Workshop: <br/>
<a href="https://www.youtube.com/watch?v=MTEOv4WMVtM">
<img width="400px" src="https://i.ibb.co/JcGQSnv/Screenshot-2023-06-09-at-10-22-33.png" />
</a>

### Programme

What if a liquid staking primitive has been added into the default Cosmos SDK staking module? Become one of the first Cosmos users to get hands-on experience with LSM, a modification proposed by [ADR-061](https://docs.cosmos.network/v0.47/architecture/adr-061-liquid-staking) that enables stake transfers without explicit unbonding.

**During this workshop you will learn:**
* The key difference between LSM approach and other LS solutions
* Technical details overview for the current implementation
* How to do Tokenizing, Redemption and collect Rewards in LSM paradigm
* Stake transfer basics using CLI of a Cosmos node that has LSM enabled
* Security considerations when using LSM on a live chain

### Preparation

This workshop includes an interactive part, where we ask audience to launch console and use CLI of the persistenceCore node to try out the transactions. To participate, you need to do only few steps.

#### 1. Get an executable for your OS

* **Linux x86_64:** https://github.com/persistenceOne/lsm-workshop-gateway23/releases/download/v8.0.0-rc4/persistenceCore-linux-amd64.tar.gz
* **MacOS (M1 Chip):** https://github.com/persistenceOne/lsm-workshop-gateway23/releases/download/v8.0.0-rc4/persistenceCore-macos-m1.tar.gz
* **MacOS x86_64:** https://github.com/persistenceOne/lsm-workshop-gateway23/releases/download/v8.0.0-rc4/persistenceCore-macos-amd64.tar.gz

<details>
<summary>CLICK HERE for (Alternative) Compiling from Git source 🚀</summary>
<br>
Ensure having Go at least 1.19: https://dl.golang.org

```bash
git clone https://github.com/persistenceOne/persistenceCore.git
cd persistenceCore
git checkout v8.0.0-rc4
go install -mod=readonly ./cmd/persistenceCore

# if it fails, try:
LEDGER_ENABLED=false && make install
```

The executable **persistenceCore** will be available in your `$HOME/go/bin`, you may want to add it to `$PATH`:

```bash
export PATH=$HOME/go/bin:$PATH
persistenceCore --help
```

</details>

After getting the archive, unpack it and make sure it's available in `$PATH`:

```bash
mkdir -p ~/go/bin
tar xf persistenceCore-macos-m1.tar.gz
mv persistenceCore-macos-m1 ~/go/bin/persistenceCore

export PATH=$HOME/go/bin:$PATH
persistenceCore --help
```

**NOTICE ⚠️** MacOS users (both Apple M1 and x86_64) need additionally install `libwasvm` because static linking on macOS is not supported properly.

```
wget https://github.com/CosmWasm/wasmvm/releases/download/v1.2.3/libwasmvm.dylib
sudo mv libwasmvm.dylib /usr/local/lib/libwasmvm.dylib
```

#### 2. Adding a test key

You should have received some mnemomnic in the email before workshop, use the following command to import it in the **testing** keyring:

```bash
persistenceCore config keyring-backend test
```

```bash
persistenceCore keys add workshop --recover
> Enter your bip39 mnemonic

```

Copy the mnemonic fully and press enter. The output should be similar to this one, but, of course, with your address.

```bash
- address: persistence1ssxksvxg7dfvcynau32p4chzw82h3ys598mr6n
  name: workshop
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"AgTM572sItbocQ//1rBLnfbRCF/9NgkA22HOlPYXPuRS"}'
  type: local
```

Make sure you copy the address, at any time you can list keys in your testing keyring:

```bash
persistenceCore keys list
```

#### 3. Setting the node URL

Last thing to prepare, the config of the client to connect to our devnet, specifically spawned for this LSM workshop.

```bash
persistenceCore config node https://lsm-devnet-rpc.core-1.dev:443
persistenceCore config chain-id lsm-workshop
```

Finally checking the config:

```bash
persistenceCore config

{
	"chain-id": "lsm-workshop",
	"keyring-backend": "test",
	"output": "text",
	"node": "https://lsm-devnet-rpc.core-1.dev:443",
	"broadcast-mode": "sync"
}
```

You also should be able to check your balance. Replace the address with the one you have. 

```bash
persistenceCore q bank balances persistence1mn2d9z62l9zqaz3gtz7hrfwg50sclrr7agrkmm

balances:
- amount: "10"
  denom: stake
```
