
# bitcoin-transaction-workshop
* references
    * https://www.oreilly.com/library/view/mastering-bitcoin-2nd/9781491954379/
    * https://www.manning.com/books/grokking-bitcoin
    * https://genesisblockhk.com/what-is-p2pkh/
    * https://bitcoin.stackexchange.com/questions/24651/whats-asm-in-transaction-inputs-scriptsig
    * https://blog.softwaremill.com/debugging-bitcoin-script-failures-f7f27b2a0ec4
    * https://www.bitpanda.com/academy/en/lessons/what-is-segwit-segregated-witness-and-how-does-it-work/
    * https://river.com/learn/what-is-segwit/
    * https://medium.com/softblocks/segregated-witness-for-dummies-d00606e8de63
    * https://seebitcoin.com/2017/02/segwit-facts-not-anyone-can-spend-so-stop-saying-they-can/
    * https://river.com/learn/bitcoins-utxo-model/
    * https://en.bitcoin.it/wiki/Transaction_malleability
    * https://en.wikipedia.org/wiki/Transaction_malleability_problem
    * https://www.coindesk.com/markets/2014/02/12/what-the-bitcoin-bug-means-a-guide-to-transaction-malleability/
    * https://github.com/ruimarinho/docker-bitcoin-core
    * https://www.willianantunes.com/blog/2022/04/bitcoin-node-with-regtest-mode-using-docker/
    * https://github.com/willianantunes/tutorials/tree/master/2022/04/bitcoin-node-regtest-mode-docker
    * https://medium.com/coinmonks/segwit-depth-overview-719ca5af56c9
    * https://en.bitcoinwiki.org/wiki/Transaction_Malleability
    * https://101blockchains.com/segregated-witness/
    * https://medium.com/lightningto-me/bitcoin-transactions-malleability-segwit-and-scaling-258af8ed9cbf
    * https://medium.com/stably-blog/what-is-a-crypto-wallet-7363d3118ce5
    * https://medium.com/blockchain/crypto-wallets-explained-f9199e621366

## transaction
* exemplary transaction
    * id: 7b3facb9dfdfb95fa158acea7b1cefb5559fdc163b6f02912cd964c6c7ddc822
* block explorer
    * https://www.blockchain.com/btc/tx/7b3facb9dfdfb95fa158acea7b1cefb5559fdc163b6f02912cd964c6c7ddc822
    * transaction written in the blockchain looks very different from what is available
    from a block explorer
        * most of the high-level constructs do not actually exist in the bitcoin system
        * for example: balance, transaction fee
* transaction written in the blockchain
    * first we get a hex representation: https://bitcoindata.science/bitcoin-raw-transaction-hex.html
        ```
        0200000001e579dda4990e47e3222b39dd791fd0b91c0e2f9b9aadcb4a00f0c2fffa55b96a000000006a473044022013c82115b3d1bf55ee233d5de00ca664c8b94dee813e203076fc9fea993640a9022012f4d990bc1f34e6b8ba24d7005043fa68e2e108fe477121c8e744c416720630012102d1e67e217d15f3ead0da6daf7cf5c20bf96178bf1cac323e94d548e8a0f8853ffeffffff028ccdf22c000000001976a914a9023863ec4241f808fc154d9e9118e0be96631688ac3b9817000000000017a9149a314ecfaa4566b1cab5094a929164686129489b8700000000
        ```
* then we decode hex to json: https://explorer.btc.com/tools/tx/decode
    ```
    {
        "txid": "7b3facb9dfdfb95fa158acea7b1cefb5559fdc163b6f02912cd964c6c7ddc822",
        "hash": "7b3facb9dfdfb95fa158acea7b1cefb5559fdc163b6f02912cd964c6c7ddc822",
        "version": 2,
        "size": 223,
        "vsize": 223,
        "weight": 892,
        "locktime": 0,
        "vin": [
            {
                "txid": "6ab955faffc2f0004acbad9a9b2f0e1cb9d01f79dd392b22e3470e99a4dd79e5",
                "vout": 0,
                "scriptSig": {
                    "asm": "3044022013c82115b3d1bf55ee233d5de00ca664c8b94dee813e203076fc9fea993640a9022012f4d990bc1f34e6b8ba24d7005043fa68e2e108fe477121c8e744c416720630[ALL] 02d1e67e217d15f3ead0da6daf7cf5c20bf96178bf1cac323e94d548e8a0f8853f",
                    "hex": "473044022013c82115b3d1bf55ee233d5de00ca664c8b94dee813e203076fc9fea993640a9022012f4d990bc1f34e6b8ba24d7005043fa68e2e108fe477121c8e744c416720630012102d1e67e217d15f3ead0da6daf7cf5c20bf96178bf1cac323e94d548e8a0f8853f"
                },
                "sequence": 4294967294
            }
        ],
        "vout": [
            {
                "value": 7.54109836,
                "n": 0,
                "scriptPubKey": {
                    "asm": "OP_DUP OP_HASH160 a9023863ec4241f808fc154d9e9118e0be966316 OP_EQUALVERIFY OP_CHECKSIG",
                    "hex": "76a914a9023863ec4241f808fc154d9e9118e0be96631688ac",
                    "address": "1GQdrgqAbkeEPUef1UpiTc4X1mUHMcyuGW",
                    "type": "pubkeyhash"
                }
            },
            {
                "value": 0.01546299,
                "n": 1,
                "scriptPubKey": {
                    "asm": "OP_HASH160 9a314ecfaa4566b1cab5094a929164686129489b OP_EQUAL",
                    "hex": "a9149a314ecfaa4566b1cab5094a929164686129489b87",
                    "address": "3FkK44B85onL1QMvHbu95Bg66T8wcHehaR",
                    "type": "scripthash"
                }
            }
        ]
    }
    ```

## outputs
* chunks of bitcoin value move forward from owner to owner in a chain of transactions
    * transaction consumes previously recorded UTXO and creates new UTXO
        * UTXO = unspent transaction output
            * UTXO is an output of a Bitcoin transaction
            * UTXO exists until it is used as an input in a subsequent transaction
                * at which point it is no longer unspent
        * full nodes track all available and spendable outputs (UTXO)
            * The UTXO set grows as new UTXO is created and shrinks when UTXO is consumed
    * exception: coinbase transaction
        * reason: brand-new bitcoin payable to that miner as a reward for mining
            * does not consume UTXO => has a special type of input called the "coinbase"
        * first transaction in each block
* indivisible units - unspent output can only be consumed in its entirety
    * however, bitcoin can be divided down to eight decimal places as satoshis
        * like dollars: can be divided down to two decimal places as cents
    * if UTXO is larger than the desired value => it must still be consumed and
    change must be generated in the transaction
        * most bitcoin transactions will generate change
    * example
        * you have UTXO: 10 bitcoin
            * note that wallet application will select from the user’s available UTXOs to compose
            an amount >= desired transaction amount
        * you want to pay: 1 bitcoin
        * transaction
            * consumes 10-bitcoin UTXO
            * produces two outputs
                * desired recipient: 1 bitcoin
                * change: 9 bitcoin back to your wallet
    * analogy
        * UTXO cannot be cut in half <=> dollar bill cannot be cut in half
        * shopper buying a $2 beverage
            * choose exact amount if available
                * for example: combination of smaller denominations (eight quarters)
            * larger unit if exact amount not available
                * for example: $5 and will expect $3 change
* vout
    * amount of bitcoin, denominated in satoshis
    * locking script
        * spending condition placed on an output
            * specifies the conditions that must be met to spend the output in the future
        * also called: scriptPubKey, witness script
        * example
            ```
            OP_DUP OP_HASH160 a9023863ec4241f808fc154d9e9118e0be966316 OP_EQUALVERIFY OP_CHECKSIG
            ```
        * most often: address (that is essentially - a public key)
            * then from the input side, we will provide signature to verify that we own address (we have private key)
        * asm strands for assembly, which is the symbolic representation of the Bitcoin's Script language op-codes
* serialization
    * 8 bytes - Bitcoin value in satoshis
        * little-endian = reversed
    * 1–9 bytes - locking script length in bytes
    * next byes - locking script
    * example
        * https://www.blockchain.com/btc/tx/7b3facb9dfdfb95fa158acea7b1cefb5559fdc163b6f02912cd964c6c7ddc822
            * output 0
                * amount: 7.54109836 BTC -> 754109836 satoshis -> hex = 2CF2CD8C -> little-endian (hex reversed) = 8CCDF22C
                * script: 76a914a9023863ec4241f808fc154d9e9118e0be96631688ac
        * serialized transaction
            0200000001e579dda4990e47e3222b39dd791fd0b91c0e2f9b9aadcb4a00f0c
            2fffa55b96a000000006a473044022013c82115b3d1bf55ee233d5de00ca664
            c8b94dee813e203076fc9fea993640a9022012f4d990bc1f34e6b8ba24d7005
            043fa68e2e108fe477121c8e744c416720630012102d1e67e217d15f3ead0da
            6daf7cf5c20bf96178bf1cac323e94d548e8a0f8853ffeffffff02**8ccdf22c**0
            000000019**76a914a9023863ec4241f808fc154d9e9118e0be96631688ac**3b98
            17000000000017a9149a314ecfaa4566b1cab5094a929164686129489b8700000000

## inputs
* txid: hash reference to transaction that contains the UTXO being spent
    * we don’t know any detail about this UTXO
        * for example: amount, locking script
        * every validating node will need to retrieve the UTXO in order to validate the transaction
    * is a sha256d hash of all the fields of the transaction data
        * txid depends on scriptSig
* vout: index identifying which UTXO from that transaction is referenced (first one is 0)
    * references transaction can have many UTXO
* scriptSig: unlocking script
    * satisfy the spending conditions placed on an output by a locking script
    * most often: digital signature and public key proving ownership of the bitcoin
* sequence: most transactions set this value to the maximum integer value (0xFFFFFFFF = 4294967295)
    * it is ignored by the bitcoin network
* serialization
    * 32 bytes: transaction hash pointer
    * 4 bytes: output index
    * 1–9 bytes: unlocking script length in bytes
    * next bytes: unlocking script
    * 4 bytes: sequence number: (0xFFFFFFFF)
    * example
        * https://www.blockchain.com/btc/tx/7b3facb9dfdfb95fa158acea7b1cefb5559fdc163b6f02912cd964c6c7ddc822
            * input 0
                * txid: 6ab955faffc2f0004acbad9a9b2f0e1cb9d01f79dd392b22e3470e99a4dd79e5
                    * reversed: e579dda4990e47e3222b39dd791fd0b91c0e2f9b9aadcb4a00f0c2fffa55b96a
                * unlocking script: 473044022013c82115b3d1bf55ee233d5de00ca664c8b94dee813e203076fc9fea993640a9022012f4d990bc1f34e6b8ba24d7005043fa68e2e108fe477121c8e744c416720630012102d1e67e217d15f3ead0da6daf7cf5c20bf96178bf1cac323e94d548e8a0f8853f
                * sequence: 4294967294 -> FFFFFFFE -> EFFFFFFF
        * serialized transaction
            0200000001**e579dda4990e47e3222b39dd791fd0b91c0e2f9b9aadcb4a00f0c
            2fffa55b96a**000000006a**473044022013c82115b3d1bf55ee233d5de00ca664
            c8b94dee813e203076fc9fea993640a9022012f4d990bc1f34e6b8ba24d7005
            043fa68e2e108fe477121c8e744c416720630012102d1e67e217d15f3ead0da
            6daf7cf5c20bf96178bf1cac323e94d548e8a0f8853f**f**effffff**028ccdf22c0
            00000001976a914a9023863ec4241f808fc154d9e9118e0be96631688ac3b98
            17000000000017a9149a314ecfaa4566b1cab5094a929164686129489b8700000000
* vin asm
    * notice that the hex and the asm are almost the same except the `47` in the beginning and `0121` towards
        the end
            * these are op-codes that tell the interpreter to put a specific amount of bytes to the stack
            * so in the case of the signature it tells to put 71 bytes (0x47) to the stack and 33 bytes (0x21)
            for the public key

## transaction scripts and script language
* reverse-polish notation stack-based execution language
* example: locking/unlocking scripts
* Pay-to-Public-Key-Hash script
    * most popular script
    * example: https://www.blockchain.com/btc/tx/5cc1983995e6b7c5369d1a2a05c0448fa7ac5d7246b00fc80fd4cb2985716bb2
    * locking script: locks the output to a public key hash (bitcoin address)
    * can be unlocked (spent) by presenting a public key and a digital signature
    created by the corresponding private key
* no loops or complex flow (only conditional flow control)
    * not Turing Complete
    * limited complexity and predictable execution times
    * limited language prevents the transaction validation mechanism from being
    used as a vulnerability
        * example: no loops => no infinite loop
* stateless
    * no state prior or no state saved
    * if transaction is valid => it will be valid for everyone
* script execution stack
    * scripting language uses a stack
        * example
            * OP_ADD
                1. pop two items
                1. add them
                1. push the resulting sum
            * OP_EQUAL
                1. pops two items from the stack
                1. pushes
                    * TRUE (represented by the number 1) if they are equal
                    or FALSE (represented by zero) if they are not equal
            * https://siminchen.github.io/bitcoinIDE/build/editor.html
                * 2 3 OP_ADD 5 OP_EQUAL
    * most locking scripts refer to a public key hash, thereby requiring proof of ownership
    to spend the funds
        * the script does not have to be that complex
            * any combination of locking and unlocking scripts that results in a TRUE value is valid
                * example
                    * 3 OP_ADD 5 OP_EQUAL
                    * can be satisfied by a transaction containing an input with the unlocking script: 2

## validation
* executing the locking and unlocking scripts together
    * each input contains
        * an unlocking script
        * reference to a previously existing UTXO
    * validation software will retrieve the locking script from UTXO referenced by the input
    * scripts are executed separately with the stack transferred between the two executions
        * transactions are valid <=> top result on the stack is
            * TRUE ({0x01})
            * any other nonzero value
            * if the stack is empty after script execution
        * transactions are invalid <=> top value on the stack is
            * FALSE (a zero-length empty value, noted as {} )
            * if script execution is halted explicitly by an operator
                * example: OP_VERIFY, OP_RETURN, OP_ENDIF
        * in the original bitcoin client, the unlocking and locking scripts were concatenated
        and executed in sequence
            * for security reasons, this was changed in 2010
                * malformed unlocking script could push data onto the stack and corrupt the locking script

## fees
* motivation
    * incentive to include (mine) a transaction into the next block
        * miners prioritize transactions based on many different criteria
        * note that transaction with insufficient or no fees might be delayed or not processed at all
    * disincentive against abuse of the system by imposing a small cost on every transaction
        * making it economically infeasible for attackers to flood the network with transactions
* most wallets calculate and include transaction fees automatically
    * dynamic fees
        * example
            * https://bitcoinfees.earn.com/
            * https://bitcoinfees.earn.com/api/v1/fees/recommended
        * calculated based on
            * size of the transaction in kilobytes
            * fees offered by "competing" transactions
* data structure of transactions does not have a field for fees
    * any excess amount that remains after all outputs have been deducted from all inputs
    * fee = sum(inputs) – sum(outputs)
    * example
        * input: 10-bitcoin UTXO
        * output: 1-bitcoin
        * change 9-bitcoin
            * otherwise 9-bitcoin "leftover" will be collected by the miner

## SegWit (Segregated Witness)
* problem with old transactions
    * security
        * cryptographic truism: signatures can't be part of the thing that is signed
            * node can mutate scriptSig in such a way, that the signature will stay valid, the transaction will
            have the same effect, but txid will change
                * example: add a OP_NOP operation (that does nothing)
                * example: add OP_DUP OP_DROP
                    * first one is duplicating the signature on the stack, and the second one removes it again
        * use case
            * attacker can intercept a normal transaction and distribute the modified version through the network
            * with some probability the miners will include this modified transaction instead of the original one
        * transaction chains in one block
            * one can transfer funds from A to B. And then, without waiting for any confirmations, transfer
            bitcoins from B to C
                * if txid of the first transaction can change, the Previous tx field of the second transaction
                should also change
                * it means that the second transaction can only be created when the first transaction is
                included in a block (and therefore its txid is fixed)
        * MtGox exchange in 2013
            * an attacker was withdrawing the funds from the exchange, intercepting the transaction, and changing its txid
            * the transaction was still valid and the attacker got the bitcoins
            * but the exchange saw that txid was never included in the block and did not decrease the attackers balance
    * scalability
        * Bitcoin network takes around 10 minutes on average to validate every new block
        * therefore, the block size plays a crucial role in determining the number of transactions each block can confirm
        * more than half of the data is scriptSig
* is basically a protocol upgrade for the Bitcoin blockchain network
    * primary idea behind the working of Segregated Witness focuses on reorganizing block data
    * by applying SegWit, you would separate signatures from transaction data
    * segregation of witnesses or signatures from the transaction data helps in storing more transactions in a single block
    * separates a transaction into two sections
        * first: the wallet addresses of the sender and receiver
        * second: transaction signatures or witness data
        * separating this data, we essentially allow transactions to be smaller
            * of course this is cheating, because we still need to store all these signatures
            * but now this data does not count towards the block size limit
                * this was the only way to implement SegWit as a soft fork, and not a hard fork
                    * soft fork = its updates can be ignored
                    * hard fork = old nodes are being excluded from the network
                    * SegWit enables old nodes to see SegWit transactions as "anyone-can-spend"
* example
    ![alt text](img/segwit.png)
* lightning network
    * problem
        * every bitcoin transaction ever made is stored in bitcoin blockchain
        * thousands of computers around the world are storing information about me buying coffee for 0.001 bitcoins
            * large fees and wait for ten minutes for the confirmation of even a tiny transaction
    * solution
        * have some small transactions executed off the main chain, and then sometimes sync the balance
        * called: second layer network
        * implementation: Lightning network
    * overview
        1. two persons open a micropayment channel between each other, both have some amount of funds locked
        1. they execute large number of small transactions between each other
        1. honest behaviour is guaranteed by some very clever crypto-magic, conflicts are automatically resolved from the locked funds
        1. as soon as the channel is closed — the locked funds are unlocked, the balances are synced to the main blockchain
    * SegWit context
        * makes the implementation much easier
        * micropayment channels rely on double-signed transactions to lock the initial deposits
            * funds from both parties are sent to one double-signed address
            * transaction should be double-signed before any funds are actually sent there
            * one needs to collect the outputs of transactions that were not yet synced to the main blockchain
                * it's exactly what we described in "transaction chains in one block"

## wallet
* store the keys that enable you to access your cryptocurrencies
* cryptocurrencies don’t physically sit in crypto wallet
* crypto wallets consist of three parts: a public key, a private key, and a public receiving address
* whenever someone sends crypto from their wallet, they must use their private key to "sign", or confirm, the transaction
* concept of a balance is created by the wallet application
* calculates the user’s balance by scanning the blockchain and aggregating the value of any UTXO
the wallet can spend with the keys it controls
    * uses a database service to store a quick reference set of all the UTXO they can spend with the keys they control
* user’s wallet has "received" bitcoin = wallet has detected a UTXO that can be spent with
one of the keys controlled by that wallet
* several strategies to satisfy the purchase amount
    * combining several smaller units
    * finding exact change
    * using a single unit larger than the transaction value and making change
* types
    * Hardware Wallet
        * stores your coins offline and is generally considered the safest wallet type as it can’t be hacked, this is what is known as a cold wallet
    * Software Wallet
        * can also be called an online wallet
        * it refers to an app or online wallet which you only need the internet in order to access
        * less safe than a hardware wallet as they can be hacked, however they are usually more user-friendly, and less likely to be lost

## workshops
* https://github.com/BlockchainCommons/Learning-Bitcoin-from-the-Command-Line/blob/8598756ae138608b21082d210f4f638a4507c67d/A3_0_Using_Bitcoin_Regtest.md#generate-blocks
* docker-compose up bitcoin-core-regtest -d
* log into a container: docker-compose exec --user bitcoin bitcoin-core-regtest sh
* create wallets
    * bitcoin-cli -regtest createwallet "iago"
    * bitcoin-cli -regtest createwallet "jafar"
    * verify that list of addresses are empty
        * bitcoin-cli -regtest -rpcwallet=iago listreceivedbyaddress 1 true
* create addresses for each wallet
    * bitcoin-cli -regtest -rpcwallet=iago getnewaddress
    * bitcoin-cli -regtest -rpcwallet=jafar getnewaddress
    * verify that addresses are created correctly
        * bitcoin-cli -regtest -rpcwallet=jafar getnewaddress
* inspect `/btc` dir (in the root of this project)
* send 50 btc to one of the wallets
    * https://github.com/BlockchainCommons/Learning-Bitcoin-from-the-Command-Line/blob/8598756ae138608b21082d210f4f638a4507c67d/A3_0_Using_Bitcoin_Regtest.md#generate-blocks
    * bitcoin-cli -regtest generatetoaddress 101 bcrt1q7c6h8a4n6tvmkfcwhpe9gvpefgyfec2t75vwru
    * verify balance: bitcoin-cli -regtest -rpcwallet=iago getbalance
* transfer coins from one wallet to another
    * bitcoin-cli -regtest -rpcwallet=iago -named sendtoaddress \
        address=bcrt1qj28d8v3528ygqpjt2dp436sx3dhn7x5qwxnyc9 \
        amount=15 \
        fee_rate=1000
      d153b10e8eb4b348ab384e1888984c73f7dfc47508a1953eab3b63da65094a76
    * verify the transaction details
        * bitcoin-cli -regtest -rpcwallet=iago gettransaction d153b10e8eb4b348ab384e1888984c73f7dfc47508a1953eab3b63da65094a76
* verify target account balance
    * bitcoin-cli -regtest -rpcwallet=jafar getbalance
    * bitcoin-cli -regtest -rpcwallet=jafar getunconfirmedbalance
    * mine block (RegTest requires at least one confirmation)
        * bitcoin-cli -regtest -rpcwallet=iago -generate 1
        * verify transaction details once again (take a look at confirmations)
            * bitcoin-cli -regtest -rpcwallet=iago gettransaction d153b10e8eb4b348ab384e1888984c73f7dfc47508a1953eab3b63da65094a76
