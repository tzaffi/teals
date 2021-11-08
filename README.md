✍️

# teals - Examples and Explorations of Algorand Smart Contracts

## Links

* Dec 2019 [medium article](https://medium.com/algorand/understanding-algorand-smart-contracts-b9fc743e7a0f) by Jason Weathersby

## Examples

### Simple

```
❯ goal clerk compile simple.teal
simple.teal: 6Z3C3LDVWGMX23BMSYMANACQOSINPFIRF77H7N3AWJZYV6OH6GWTJKVMXY
❯ ls
simple.teal     simple.teal.tok
```

### Escrow

Based on [Weathersby's 4/21/21 Tutorial on Simple Smart Contracts](https://developer.algorand.org/tutorials/writing-simple-smart-contract)

0. `teals/passphrase.teal` -> `~/node/teal`
1.  the tutorial provides a python command to validate that the `sha256` of the pass phrase is as expected
2. `goal clerk compile passphrase.teal` -> creates contract account `RZ2CUMV2VG3NCVFEVUVHVBYQUABQIBWAT3AC736Y6YL7PPDNAD7K3TVNAI`
3. [Dispense](https://bank.testnet.algorand.network/) into the contract account
4. `goal account balance -a  RZ2CUMV2VG3NCVFEVUVHVBYQUABQIBWAT3AC736Y6YL7PPDNAD7K3TVNAI -d ~/node/testnetdata/` (60000000 micro-algos)
5. [base 64 encoding RFC](https://datatracker.ietf.org/doc/html/rfc4648#page-5)
6. echo -n "weather comfort erupt verb pet range endorse exhibit tree brush crane man" | base64
7. `goal clerk send -a 30000 --from-program passphrase.teal  -c STF6TH6PKINM4CDIQHNSC7QEA4DM5OJKKSACAPWGTG776NWSQOMAYVGOQE --argb64 d2VhdGhlciBjb21mb3J0IGVydXB0IHZlcmIgcGV0IHJhbmdlIGVuZG9yc2UgZXhoaWJpdCB0cmVlIGJydXNoIGNyYW5lIG1hbg==  -t STF6TH6PKINM4CDIQHNSC7QEA4DM5OJKKSACAPWGTG776NWSQOMAYVGOQE -o out.txn -d ~/node/testnetdata`
  * notice that `-c` the _closeout_ account is identitcal to `-t` the _target_ or _receiver_ account (as required by the contract)
  * notice the `--from-program
8. `goal clerk dryrun -t out.txn` # we are now confident of success
9. SUBMIT IT:

```bash
❯ goal clerk rawsend -f out.txn -d ~/node/testnetdata
Raw transaction ID PWC2MSWU6QELLQTYBPIX4JF2K2RCVBVRBEF44COTINDRI2MYQ2UQ issued
Transaction PWC2MSWU6QELLQTYBPIX4JF2K2RCVBVRBEF44COTINDRI2MYQ2UQ still pending as of round 17762317
Transaction PWC2MSWU6QELLQTYBPIX4JF2K2RCVBVRBEF44COTINDRI2MYQ2UQ committed in round 17762319

❯ goal account balance -a  RZ2CUMV2VG3NCVFEVUVHVBYQUABQIBWAT3AC736Y6YL7PPDNAD7K3TVNAI -d ~/node/testnetdata
0 microAlgos

# Not gonna succeed again, cause nothing left in that account:
❯ goal clerk rawsend -f out.txn -d ~/node/testnetdata
Warning: Couldn't broadcast tx with algod: HTTP 400 Bad Request: TransactionPool.Remember: transaction already in ledger: PWC2MSWU6QELLQTYBPIX4JF2K2RCVBVRBEF44COTINDRI2MYQ2UQ
Encountered errors in sending 1 transactions:
  PWC2MSWU6QELLQTYBPIX4JF2K2RCVBVRBEF44COTINDRI2MYQ2UQ: HTTP 400 Bad Request: TransactionPool.Remember: transaction already in ledger: PWC2MSWU6QELLQTYBPIX4JF2K2RCVBVRBEF44COTINDRI2MYQ2UQ
Rejected transactions written to out.txn.rej
```
