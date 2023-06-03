## Liquid Staking Module Workshop

Demo Steps!

### Pre-checks

Before continuing, make sure you've done steps outlined in [README.md](/README.md), such as:

1. Build or install the app binaries
2. Set the local config values
3. Import key to keyring from mnemonics (sent by email)

Config values:

```
persistenceCore config node https://lsm-devnet-rpc.core-1.dev:443
persistenceCore config chain-id lsm-workshop
```

Explorer for the transactions and new blocks is available at:
https://lsm-devnet.core-1.dev/lsm-workshop/block

### Note about Tx

To be ensure that the transactions will be sent correctly, you should have the following flags with each tx you send:

```
--from workshop -y --gas auto --gas-adjustment 1.5
```

Additional `--keyring-backend=test --chain-id=lsm-workshop` can be ommited, because set in the config.

<hr />

### Step 1: Check your delegations

```
persistenceCore q staking delegations YOUR_ADDRESS
```

Replace `YOUR_ADDRESS` with your workshop key address, to list all local keys:

```
persistenceCore keys list

persistenceCore keys show workshop -a
```

### Step 2: Tokenize your delegation

```
persistenceCore tx staking tokenize-share persistencevaloper1dvxmv2ghefusunnf7vsxhstptql5ggdn6m3ltz 10000000stake YOUR_ADDRESS --from test -y --gas auto --gas-adjustment 1.5
```

Replace `YOUR_ADDRESS` with your workshop key address, to list all local keys:

```
persistenceCore keys list

persistenceCore keys show workshop -a
```

### Step 3: Check bank balance

```
persistenceCore q bank balances YOUR_ADDRESS
```

### Step 4: Check all delegators progress of tokenizing

```
persistenceCore q staking total-tokenize-share-assets
```

### Step 5: Check all records owned

```
persistenceCore q staking tokenize-share-records-owned YOUR_ADDRES
```

### Step 6: Check record’s accumulated rewards

```
persistenceCore q distribution tokenize-share-record-rewards RECORD_ID
```

`RECORD_ID` must be from your tokenized position, see the list from previous step!

### Step 7: Transferring half of the shares to Max

```
persistenceCore tx bank send YOUR_ADDRESS persistence1mn2d9z62l9zqaz3gtz7hrfwg50sclrr7agrkmm 5000000persistencevaloper1dvxmv2ghefusunnf7vsxhstptql5ggdn6m3ltz/YOUR_RECORD_ID --from test -y --gas auto --gas-adjustment 1.5
```

Replace `YOUR_ADDRESS` and `YOUR_RECORD_ID` with values obtained before.

### Step 8: Check Max’ delegations

```
persistenceCore q staking delegations persistence1mn2d9z62l9zqaz3gtz7hrfwg50sclrr7agrkmm
```

### Step 9: Transferring the record to Max

```
persistenceCore tx staking transfer-tokenize-share-record YOUR_RECORD_ID persistence1mn2d9z62l9zqaz3gtz7hrfwg50sclrr7agrkmm --from test -y --gas auto --gas-adjustment 1.5
```

Replace `YOUR_RECORD_ID` with numeric ID of your record.

### Step 10: Redeem the remaining half of shares

```
persistenceCore tx staking redeem-tokens
5000000persistencevaloper1dvxmv2ghefusunnf7vsxhstptql5ggdn6m3ltz/YOUR_RECORD_ID --from test -y --gas auto --gas-adjustment 1.5
```

Replace `YOUR_RECORD_ID` with numeric ID of your record.

### Last Step

```
persistenceCore q staking total-tokenize-share-assets
```

Should be zero!

```
persistenceCore q staking all-tokenize-share-records
```

Should be empty!


### Post-checks

```
persistenceCore q staking delegations-to persistencevaloper1dvxmv2ghefusunnf7vsxhstptql5ggdn6m3ltz
```

Query all delegations made to one validator. Includes both module accounts and direct delegations.

