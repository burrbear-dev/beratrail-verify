## Foundry

**Foundry is a blazing fast, portable and modular toolkit for Ethereum application development written in Rust.**

Foundry consists of:

- **Forge**: Ethereum testing framework (like Truffle, Hardhat and DappTools).
- **Cast**: Swiss army knife for interacting with EVM smart contracts, sending transactions and getting chain data.
- **Anvil**: Local Ethereum node, akin to Ganache, Hardhat Network.
- **Chisel**: Fast, utilitarian, and verbose solidity REPL.

## Documentation

https://book.getfoundry.sh/

## Usage

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```

### Format

```shell
$ forge fmt
```

### Gas Snapshots

```shell
$ forge snapshot
```

### Anvil

```shell
$ anvil
```

### Deploy

```shell

forge --version
forge 0.2.0 (7f41280 2024-11-30T00:24:04.195299000Z)

# this WORKS : deploys correctly on BARTIO and verifies the contract
forge script script/Counter.s.sol:CounterScript \
  --account X_DEPLOYER2 \
  --rpc-url ${BARTIO_RPC_URL} \
  --broadcast \
  -vvvvv \
  --verify --verifier custom \
  --verifier-url 'https://api.routescan.io/v2/network/testnet/evm/80084/etherscan' \
  --verifier-api-key "verifyContract" \
  --chain 80084


# this only deploys but the verification steps fails (at least sometimes) with:
#       Response: `NOTOK`
#       Details: `Error: contract does not exist 80084 0xA4f4C07A01F7995587044c9F1f97440DF3dBbFBe`
forge create script/Counter.s.sol:CounterScript --broadcast \
  --rpc-url ${BARTIO_RPC_URL} --account X_DEPLOYER2 --verify --verifier custom \
  --verifier-url 'https://api.routescan.io/v2/network/testnet/evm/80084/etherscan' --verifier-api-key "verifyContract" \
  --chain 80084

# Trying to verify the contract deployed above (but where verification failed) does not work (?!)
❯ forge verify-contract 0xA4f4C07A01F7995587044c9F1f97440DF3dBbFBe src/Counter.sol:Counter --watch \
  --verifier custom --verifier-url 'https://api.routescan.io/v2/network/testnet/evm/80084/etherscan/api/' \
  --verifier-api-key "verifyContract" \
  --chain 80084 -vvvvv --rpc-url $BARTIO_RPC_URL
Start verifying contract `0xA4f4C07A01F7995587044c9F1f97440DF3dBbFBe` deployed on 80084

Submitting verification for [src/Counter.sol:Counter] 0xA4f4C07A01F7995587044c9F1f97440DF3dBbFBe.
Submitted contract for verification:
        Response: `OK`
        GUID: `5fe8e033-fe0e-591e-a796-bff70a63a613`
        URL: https://api.routescan.io/v2/network/testnet/evm/80084/etherscan/address/0xa4f4c07a01f7995587044c9f1f97440df3dbbfbe
Contract verification status:
Response: `NOTOK`
Details: `Error: Compilation successful, but bytecode does not match with deployed bytecode for address 0xA4f4C07A01F7995587044c9F1f97440DF3dBbFBe, please check your parameters`

# another try, this time with more specific params for compiler and evm version
# in case forge verify-contract has a but and does not pick them up from the foundry.toml
# also no go :(
❯ forge verify-contract 0xA4f4C07A01F7995587044c9F1f97440DF3dBbFBe src/Counter.sol:Counter --watch \
  --verifier custom --verifier-url 'https://api.routescan.io/v2/network/testnet/evm/80084/etherscan/api/' \
  --verifier-api-key "verifyContract" \
  --num-of-optimizations 200 --evm-version "cancun" --compiler-version "0.8.13" \
  --chain 80084 -vvvvv --rpc-url $BARTIO_RPC_URL

Start verifying contract `0xA4f4C07A01F7995587044c9F1f97440DF3dBbFBe` deployed on 80084
Compiler version: 0.8.13
Optimizations:    200

Submitting verification for [src/Counter.sol:Counter] 0xA4f4C07A01F7995587044c9F1f97440DF3dBbFBe.
Submitted contract for verification:
        Response: `OK`
        GUID: `14c8391f-684f-5bec-8088-66db9ce58e6a`
        URL: https://api.routescan.io/v2/network/testnet/evm/80084/etherscan/address/0xa4f4c07a01f7995587044c9f1f97440df3dbbfbe
Contract verification status:
Response: `NOTOK`
Details: `Pending in queue`
Warning: Verification is still pending... (7 tries remaining)
Contract verification status:
Response: `NOTOK`
Details: `Pending in queue`
Warning: Verification is still pending... (6 tries remaining)
Contract verification status:
Response: `NOTOK`
Details: `Error: Compilation successful, but bytecode does not match with deployed bytecode for address 0xA4f4C07A01F7995587044c9F1f97440DF3dBbFBe, please check your parameters`

```

### Cast

```shell
$ cast <subcommand>
```

### Help

```shell
$ forge --help
$ anvil --help
$ cast --help
```
