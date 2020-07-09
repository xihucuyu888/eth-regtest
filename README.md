# eth-regtest
Scripts to launch an etheruem local testnet and deploy ERC20

## install geth
```bash
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.9.15-0f77f34b.tar.gz
tar -xzvf geth-linux-amd64-1.9.15-0f77f34b.tar.gz
cp ./geth-linux-amd64-1.9.15-0f77f34b/geth /usr/local/bin/
rm -rf geth*
```
## generate genesis.json
```json
{
  "config": {
    "chainId": 123,
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip155Block": 0,
    "eip158Block": 0,
    "ByzantiumBlock": 0
  },
  "difficulty": "1",
  "gasLimit": "2100000",
  "alloc": {
    "0x4fe77cb0cdc5d302778a4096fad97c5c5313d4b5": {
      "balance": "20000000000000000000000000"
    },
    "0xd0da98b9d11a6632e0f534a5ff32152c8bc26629": {
      "balance": "200000000000000000000000000"
    },
    "0x04e98D7A5ca93d3DdcF88DbB0f9Dde1E2910061f": {
      "balance": "2000000000000000000000000000"
    },
    "0xc5E33b3CED9aF1c58875759cE1A179b9Ee06761d": {
      "balance": "2000000000000000000000000000"
    }
  }
}
```
## initialize 
```bash
mkdir -p /opt/eth && /opt/eth/gethDataDir
geth --datadir /opt/eth/gethDataDir/ init genesis.json
```

## start eth
```bash
geth --mine --miner.threads=1 --datadir /opt/eth/gethDataDir/ --networkid 123 --rpc --rpcaddr 0.0.0.0 --rpcport=8547 --rpcapi="db,eth,net,web3,personal,web3" --allow-insecure-unlock --miner.etherbase=0xEc67A59e54A393b702c7EcCe1faca731E4f0e601
```
## deploy ERC20
```bash
geth attach http://127.0.0.1:8547
```
Example:
```bash
Welcome to the Geth JavaScript console!

instance: Geth/v1.9.15-stable-0f77f34b/linux-amd64/go1.14.4
coinbase: 0xec67a59e54a393b702c7ecce1faca731e4f0e601
at block: 0 (Thu Jan 01 1970 08:00:00 GMT+0800 (CST))
 modules: eth:1.0 net:1.0 personal:1.0 rpc:1.0 web3:1.0

> web3.personal.importRawKey("51C2C48AE17D472631D8F2DF567EF9B44A625EBF55FA4D7BB6521E7E9D96E701","1")
"0xc5e33b3ced9af1c58875759ce1a179b9ee06761d"

> personal.listAccounts
["0xc5e33b3ced9af1c58875759ce1a179b9ee06761d"]

> loadScript('/opt/eth/nash.bin')
Unlock account 0xc5E33b3CED9aF1c58875759cE1A179b9Ee06761d
Passphrase:1
null [object Object]
undefined
> null [object Object]
Contract mined! address: 0x08fda414b3b90552e82c38d4029ab024068bf920 transactionHash: 0xd675c713b17c042ab22d5c819741957c71f28016d7bfe02c4901a49376c79959
```

## implements

generate abi and bin using remix:
http://remix.ethereum.org/#optimize=false&version=soljson-v0.4.25+commit.59dbf8f1.js








