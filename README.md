# quai-compose

Run a containerized instance of Quai Network on a local machine.

## Install Dependencies

### Docker Compose

There are two methods to install Docker Compose:

1. Install [Docker Desktop](https://docs.docker.com/compose/install/), which bundles Docker Compose. (**recommended**)
2. Install the [Docker Compose plugin](https://docs.docker.com/compose/install/linux/) via CLI.

If you're unsure which method is best for you, visit the [Installing Docker Compose](https://docs.docker.com/compose/install/) page for more information.

### Local Node Runner

Clone the `quai-compose` repository:

```bash
git clone https://github.com/dominant-strategies/quai-compose
cd quai-compose
```

## Run

To start the local network instance in a container, run:

```bash
docker-compose up
```

After running this command, you'll see each of the processes start up. One go-quai container, and 9 instances of quai-cpu-miner (one for each shard) should initialize like so:

```bash
[+] Running 10/10
 ⠿ Container quai-compose-paxos2-quai-cpu-miner-1   Started
 ⠿ Container quai-compose-cyprus1-quai-cpu-miner-1  Started
 ⠿ Container quai-compose-cyprus2-quai-cpu-miner-1  Started
 ⠿ Container quai-compose-hydra2-quai-cpu-miner-1   Started
 ⠿ Container quai-compose-paxos1-quai-cpu-miner-1   Started
 ⠿ Container quai-compose-cyprus3-quai-cpu-miner-1  Started
 ⠿ Container quai-compose-go-quai-1                 Started
 ⠿ Container quai-compose-paxos3-quai-cpu-miner-1   Started
 ⠿ Container quai-compose-hydra1-quai-cpu-miner-1   Started
 ⠿ Container quai-compose-hydra3-quai-cpu-miner-1   Started
```

To stop the network instance, run:

```bash
docker-compose down
```

## Interacting With The Local Network

To interact with the local network instance, you can use the [Quais SDK](https://docs.qu.ai/sdk/introduction) or [JSON RPC API](https://docs.qu.ai/build/playground/overview). The network is accesible via specific networking ports on `localhost`.

| Shard    | HTTP Port | WS Port |
| -------- | --------- | ------- |
| Prime    | 9001      | 8001    |
| Cyprus   | 9002      | 8002    |
| Paxos    | 9003      | 8003    |
| Hydra    | 9004      | 8004    |
| Cyprus 1 | 9200      | 8200    |
| Cyprus 2 | 9201      | 8201    |
| Cyprus 3 | 9202      | 8202    |
| Paxos 1  | 9220      | 8220    |
| Paxos 2  | 9221      | 8221    |
| Paxos 3  | 9222      | 8222    |
| Hydra 1  | 9240      | 8240    |
| Hydra 2  | 9241      | 8241    |
| Hydra 3  | 9242      | 8242    |

### SDK

You can use the Quais SDK to create a provider connected to your local network instance and get chain data and send transactions.

```ts
import { quais } from 'quais'

// create local provider
const provider = new quais.JsonRpcProvider('http://localhost')

// get block number on cyprus 1
const currentBlock = await provider.getBlockNumber('Cyprus1')
```

### JSON RPC API

You can make requests to your local network using the [JSON RPC API](/build/playground/overview) and [cURL](https://curl.se/):

```bash
# get paxos 3 gas price
curl --request POST \
  --url http://localhost:9222 \
  --header 'Content-Type: application/json' \
  --data '{
  "jsonrpc": "2.0",
  "method": "quai_gasPrice",
  "params": [],
  "id": 1
}'
```

## Accounts

The local network we just spun up starts with a number of accounts pre-loaded with Quai and Qi tokens for each shard. A full list of accounts with pre-loaded balances, sorted by shard and ledger, can be found in the [genallocs](https://github.com/dominant-strategies/quai-compose/tree/main/genallocs) directory of the quai-compose repository.

**These accounts are for local testing purposes and should not be used in any production environment. They are not secure and should only be used for testing.**

For quick reference, 1 account for each shard and ledger can be found below:

### Quai Accounts

| Shard   | Address                                    | Private Key                                                        | Balance    |
| ------- | ------------------------------------------ | ------------------------------------------------------------------ | ---------- |
| Cyprus1 | 0x000001273B55E9e5998328216dB1b130c231221C | 0xb3c87ca9645adcf75be5c4d6609fd257f897fd53849c7ecca81acae9966a6ec0 | 10000 Quai |
| Cyprus2 | 0x010001D025371794a6eDb5feE8aC2F384EdD7463 | 0xcea8ebb619f8e4ea11ee75cb72221a6f39591a99d7cf688de9f30832809fb751 | 10000 Quai |
| Cyprus3 | 0x02000415996A1B0cFF4b2FD078c779cF6C3E9AaC | 0xfc86fae56f462d2ae43bca8f819b1137c9e3150ba3ff79d4d5068b6e39c1c975 | 10000 Quai |
| Paxos1  | 0x10000197Ec7c3e6ce4D9a832c0641528c5e268A6 | 0x01d872f0bc5f94490c2ed9026d58a116656dcb3c997f42062d01799e5b458062 | 10000 Quai |
| Paxos2  | 0x11000207A6723c18085c12F50F62929bb78932c4 | 0x1d7a3a668ac8b20bf538a6d1060de043f10b1db5f1df2140fa0e8d479820e763 | 10000 Quai |
| Paxos3  | 0x120000833E752B14A00eBB2c00eF0FD7C90C2123 | 0xc0061e94c526e7d9d97a2874f129e72e4f821a8f78ca2fd8198c005bc14e2a92 | 10000 Quai |
| Hydra1  | 0x2000011eCb5FDEA6Eb074cbe60b6Ad372948d99a | 0xc77fb4c5b1612f702ef097561f75ee5876987ef547977020a86528fbc9f7bbbc | 10000 Quai |
| Hydra2  | 0x2100042dEf9D880e029Ca948f97C180c202bd743 | 0x44af4c54c44d96bbf3d9c602967822246381d2287fe544fd5480d05b25b80bb9 | 10000 Quai |
| Hydra3  | 0x2200035A5A89846AD708d8732B6c85dFAbE35489 | 0x8cd878d69b1b848b3c98c623e4e56e3b0ed1035984f6f721a99ef716637e3382 | 10000 Quai |

### Qi Accounts

| Shard   | Address                                    | Private Key                                                        | Balance   |
| ------- | ------------------------------------------ | ------------------------------------------------------------------ | --------- |
| Cyprus1 | 0x0080038f1C9a96b196914939301F9a46d7E27e7E | 0x5b18d340e2e5172a90dfcc43c800519cf4a77b82750b8964b3f421dc929eac53 | 100000 Qi |
| Cyprus2 | 0x01800Daf0f10f7d0CceF2B77893d5a7f86D6D406 | 0x4500c9cef91ac29479cf50a00c49d27ba14e84df83b8e332db34fa1cbdd799b7 | 100000 Qi |
| Cyprus3 | 0x02800a7c404409166D903Dd3421595112f1DaF4D | 0xbc144acdd4d53488e4005d404f42450a879eb1015a6f4bbf80956b4810b5a068 | 100000 Qi |
| Paxos1  | 0x1080049337283F6c3aDfD535835258a03CF2b921 | 0x511930ce12cbf86325eb2652459b6a47b75ddc892a09549f094f0011062a11f4 | 100000 Qi |
| Paxos2  | 0x1180008531658Aa78161D00689267725C771aD3d | 0x89086104efd789c7de9ee69beb44ad70818a772a90b6d3f3ec26da930a47a2da | 100000 Qi |
| Paxos3  | 0x12800163E05c66e107864D9c18067468cf79990f | 0xa7995e0942e6510a2fd3461c0dbe081fc8e79c6e891fd2fcde5897e41700789a | 100000 Qi |
| Hydra1  | 0x208000B3ED3a21D2018E72533D55679760C7a8d2 | 0x36f009591733e706085ba4b6c3c6bbc8e35fa5e9f075979650523c18f18dacec | 100000 Qi |
| Hydra2  | 0x218000a39174Db63f0047DC571CB2279d6D29434 | 0xf07d9c6d2ee5cd1f5661d87afea21cd21e4a52ff1ea4851b7507b4ef673c4311 | 100000 Qi |
| Hydra3  | 0x228003fD91A5B55dBAfc95384e3A8DBf30B21AA8 | 0x2c1b28175b12d8fdeb8355918fcb7492fcbc7b118ecf301cd928477815084312 | 100000 Qi |
