# Retiring a stake pool

### Retire a stake pool

{% hint style="warning" %}
Pool deposit is refunded to rewards address. Do not de-register the stake address before you get the deposit back. \
\
Make sure to read  [Retire a stake pool](https://github.com/input-output-hk/cardano-node/blob/master/doc/stake-pool-operations/12\_retire\_stakepool.md#retiring-a-stake-pool)
{% endhint %}

```
cardano-cli stake-pool deregistration-certificate \
  --cold-verification-key-file cold.vkey \
  --epoch <future epoch> \
  --out-file pool-deregistration.cert
```

```
cardano-cli transaction build \
--testnet-magic 2 \
--tx-in $(cardano-cli query utxo --address $(cat payment.addr) --testnet-magic 2 --out-file  /dev/stdout | jq -r 'keys[0]') \
--change-address $(cat payment.addr) \
--witness-override 2 \
--certificate-file  pool-deregistration.cert \
--out-file tx.raw
```

```
cardano-cli transaction sign \
--testnet-magic 2 \
--tx-body-file tx.raw \
--signing-key-file payment.skey \
--signing-key-file cold.skey \
--out-file tx.signed
```

```
cardano-cli transaction submit \
--testnet-magic 2 \
--tx-file tx.signed
```
