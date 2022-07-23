# Getting useful information from node by RPC on port 3030

## Install tools

```bash
sudo apt install curl jq
```

## Common Commands:

### Check your node version:

```bash
curl -s http://127.0.0.1:3030/status | jq .version

# response
{
  "version": "trunk",
  "build": "crates-0.14.0-234-g0f81dca95",
  "rustc_version": "1.62.0"
}
```

### Check Delegators and Stake

```bash
near view timur.factory.shardnet.near get_accounts '{"from_index": 0, "limit": 10}' --accountId timur.shardnet.near

# response
View call: timur.factory.shardnet.near.get_accounts({"from_index": 0, "limit": 10})
[
  {
    account_id: 'timur.shardnet.near',
    unstaked_balance: '200000000000000000000000000',
    staked_balance: '250005707377335497912998823',
    can_withdraw: false
  },
  {
    account_id: '0000000000000000000000000000000000000000000000000000000000000000',
    unstaked_balance: '0',
    staked_balance: '2723201909581499999999',
    can_withdraw: true
  }
]
```

### Check Reason Validator Kicked
```bash
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.prev_epoch_kickout[] | select(.account_id | contains ("boot1.near"))' | jq .reason

# result
{
  "NotEnoughBlocks": {
    "expected": 10871,
    "produced": 8329
  }
}
```

### Check Blocks Produced / Expected
```bash
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.current_validators[] | select(.account_id | contains ("boot1.near"))' | jq

# result
{
  "account_id": "boot1.near",
  "is_slashed": false,
  "num_expected_blocks": 414,
  "num_expected_chunks": 1268,
  "num_produced_blocks": 414,
  "num_produced_chunks": 1252,
  "public_key": "ed25519:AHhWwLjDc9ohGmxACaFWUdBw1CSaubTyPU5YmnUD2DmP",
  "shards": [
    0
  ],
  "stake": "530000000000000000000000000000"
}

```

# Setup tools for monitoring node status

|                                        |                                       |
| -------------------------------------- | ------------------------------------- |
| [⏮ Challenge 003 ](./challenge_003.md) | [Challenge 005 ⏭](./challenge_005.md) |
