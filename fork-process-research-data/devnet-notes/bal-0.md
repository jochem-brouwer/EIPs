# bal-devnet-0 spec
:::info
:mega: bal-devnet-0 targets to launch on end of Oct 2025.
(launched [2025-Nov-04 05:02:44 PM UTC](https://github.com/ethpandaops/bal-devnets/blob/master/network-configs/devnet-0/metadata/config.yaml#L27C1-L27C30))
:::


## EIP List for bal-devnet-0

[EIP-7928: Block-Level Access Lists](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-7928.md) :new:

**Key:**
- :up:, EIP has updated!
- :new:, new EIP added.

### Test Releases

**Consensus Specs:** [`v1.6.0-beta.0`](https://github.com/ethereum/consensus-specs/releases/tag/v1.6.0-beta.0) :heavy_check_mark:  

**Execution Specs:**  EEST Block-Level Access Lists (BAL) pre-release [v1.8.0](https://github.com/ethereum/execution-spec-tests/releases/tag/bal%40v1.8.0) :heavy_check_mark: 
**Execution Test Progress:** Client TDD progress: [pokeball](https://pokebal.raxhvl.com/)


### Spec versions required & Open PRs
**Beacon Metrics**

**Execution Metrics**

**Beacon API** 

**Builder Specs**


**Consensus Specs**

**Execution APIs**

BAL: https://github.com/ethereum/execution-apis/pull/691

**Execution Spec PRs**



## Kurtosis Interop Conifg (Pre-devnet testing)

### Working configs

```yaml=
participants:
  - cl_type: lodestar
    cl_image: ethpandaops/lodestar:bal-devnet-0
    supernode: true
    el_type: geth
    el_image: ethpandaops/geth:bal-devnet-0
    el_extra_params: ["--history.state=0", "--gcmode=archive", "--syncmode=full"]
    count: 1
  - cl_type: lodestar
    cl_image: ethpandaops/lodestar:bal-devnet-0
    supernode: true
    el_type: besu
    el_image: ethpandaops/besu:bal-devnet-0
    el_extra_params: ["--Xbal-trust-state-root=true"]
    count: 1
  - cl_type: lodestar
    cl_image: ethpandaops/lodestar:bal-devnet-0
    supernode: true
    el_type: nethermind
    el_image: ethpandaops/nethermind:bal-devnet-0
    count: 1
  - cl_type: lodestar
    cl_image: ethpandaops/lodestar:bal-devnet-0
    supernode: true
    el_type: nimbus
    el_image: ethpandaops/nimbus-eth1:bal-devnet-0
    count: 1
  - cl_type: lodestar
    cl_image: ethpandaops/lodestar:bal-devnet-0
    supernode: true
    el_type: reth
    el_image: ethpandaops/reth:bal-devnet-0
    count: 1
global_log_level: 'debug'
network_params:
  genesis_delay: 20
  fulu_fork_epoch: 0
  gloas_fork_epoch: 1
  seconds_per_slot: 6
  num_validator_keys_per_node: 32
snooper_enabled: true
dora_params:
  image: ethpandaops/dora:eip7928-support
additional_services:
  - dora
  - tracoor
  - tx_fuzz
  - spamoor
port_publisher:
  additional_services:
    enabled: true
    public_port_start: 64400
spamoor_params:
  image: ethpandaops/spamoor:master
  spammers:
    - scenario: evm-fuzz
      config:
        throughput: 20
    - scenario: eoatx
      config:
        throughput: 2
    - scenario: uniswap-swaps
      config:
        throughput: 2

```

### Debug Endpoints

#### Besu debug endpoint
Besu offers a debug endpoint that returns a block and the access list besu generated:
```
curl -X POST \
--data '{"jsonrpc":"2.0","method":"debug_getBadBlocks","params":[],"id":1}' \
http://localhost:8545 \
-H "Content-Type: application/json"
```

response structure: 

```
curl -X POST \
--data '{"jsonrpc":"2.0","method":"debug_getBadBlocks","params":[],"id":1}' http://localhost:34255 -H "Content-Type: application/json" \
| jq '.result[] | keys' \
[
  "block",
  "generatedBlockAccessList",
  "hash",
  "rlp"
]
```

This makes it possible to compare the blocks access list against the generatedBlockAccessList from besu.

#### Geth debug endpoint
Getting a block access list from geth:

```
curl -X POST \
--data '{"jsonrpc":"2.0","method":"debug_getBlockAccessList","params":["0x27"],"id":1}' \
http://localhost:34245 \
-H "Content-Type: application/json" | jq
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0xf902e2da940000000000000000000000000000000000000005c0c0c0c0c0f89f9400000961ef480eb55e80d19ad83579a64c007002c0f884a00000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000001a00000000000000000000000000000000000000000000000000000000000000002a00000000000000000000000000000000000000000000000000000000000000003c0c0c0f89f940000bbddc7ce488642fb579f8b00f3a590007251c0f884a00000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000001a00000000000000000000000000000000000000000000000000000000000000002a00000000000000000000000000000000000000000000000000000000000000003c0c0c0f862940000f90827f1c53a10cb7a02335b175320002935f847f845a00000000000000000000000000000000000000000000000000000000000000026e3e280a05e9c2172450b547db675a481b9ea70667b768629ecb4a12470f8856bafca67a3c0c0c0c0f8a994000f3df6d732807ef1319fb7b8bb8522d0beac02f88ef845a00000000000000000000000000000000000000000000000000000000000000d6fe3e280a000000000000000000000000000000000000000000000000000000000690b64faf845a00000000000000000000000000000000000000000000000000000000000002d6ee3e280a0282240274ce23c0fdc7d13b326ce456854ee1482052270e1fc52639711cfc50fc0c0c0c0e89439ad5577f842342a00497be24ce546734fff6a8dc0c0cbca01884561bdc156f38d02c3c20102c0e2944909b9c2b5e860ec011740cb4fffa8e5bacedec8c0c0c5c40182bb16c3c20101c0e9948943545177806ed17b9f23f0a21ee5948ecaa776c0c0cfce018c033b2e3ca1d03cec6ad6f96ac0c0"
}
```

### Open Issues


Prysm does not manage to peer with lodestar after the Gloas fork. All Prysm blocks are orphaned:

```yaml
participants:
  - cl_type: lodestar
    cl_image: ethpandaops/lodestar:bal-devnet-0
    supernode: true
    el_type: reth
    el_image: ethpandaops/reth:bal-devnet-0
    count: 1
  - cl_type: lodestar
    cl_image: ethpandaops/lodestar:bal-devnet-0
    supernode: true
    el_type: besu
    el_image: ethpandaops/besu:bal-devnet-0
    el_extra_params: ["--bonsai-parallel-tx-processing-enabled=false"]
    count: 1
  - cl_type: prysm
    cl_image: ethpandaops/prysm-beacon-chain:bal-devnet-0
    supernode: true
    vc_type: prysm
    vc_image: ethpandaops/prysm-validator:bal-devnet-0
    el_type: geth
    el_image: ethpandaops/geth:bal-devnet-0
    count: 1
network_params:
  genesis_delay: 20
  fulu_fork_epoch: 0
  gloas_fork_epoch: 1
  seconds_per_slot: 6
  num_validator_keys_per_node: 32
snooper_enabled: true
dora_params:
  image: ethpandaops/dora:eip7928-support
additional_services:
  - dora
  - tracoor
port_publisher:
  additional_services:
    enabled: true
    public_port_start: 64400


```

## Test Vectors

- Tests where the coinbase receives ETH through a [EIP-4895](https://eips.ethereum.org/EIPS/eip-4895) withdrawal. In this case, the coinbase address should be included in the BAL, even though there are 0 transactions.
- External STEEL tests runs against the devnet & against kurtosis testnets
- Send transactions with diffferent types 0x1, [0x2](https://eips.ethereum.org/EIPS/eip-1559), [0x3](https://eips.ethereum.org/EIPS/eip-4844), [0x4](https://eips.ethereum.org/EIPS/eip-7702). Especially 0x4 is a interesting test vector.
- Run the EEST tests against a running testnet/devnet.
- TxFuzz & Spammor with a evm fuzzer scenario
- Transaction ordering tests
