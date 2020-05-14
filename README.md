![Unification](https://raw.githubusercontent.com/unification-com/mainchain/master/unification_logoblack.png "Unification")

## Unification Mainchain

[![Go Report Card](https://goreportcard.com/badge/github.com/unification-com/mainchain)](https://goreportcard.com/report/github.com/unification-com/mainchain)
[![Join the chat at https://gitter.im/unification-com/mainchain](https://badges.gitter.im/unification-com/mainchain.svg)](https://gitter.im/unification-com/mainchain?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Official Golang implementation of Unification Mainchain.

Mainchain is the backbone of the Unification Network. It is a Tendermint based chain, and is where WRKChains and BEACONs submit their hashes, and FUND transactions take place.

## Quick start installation

See [Documentation](https://docs.unification.io) for full guides.

There are several options for installing the binaries

### Pre-compiled binaries

The quickest way to obtain and run the `und` and `undcli` applications is to download
the latest pre-compiled binaries from [latest release](https://github.com/unification-com/mainchain/releases)

Once downloaded, you can verify the SHA256 checksum against those listed in the release's `checksums.txt`, for example:

```bash
openssl dgst -sha256 undcli_v1.4.5_linux_x86_64.tar.gz
SHA256(undcli_v1.4.5_linux_x86_64.tar.gz)= 239559182183648379164f5ff0faa0b3fe0e404f5368e1995e56b2269c93ec05
```

### Install from source

**IMPORTANT**: if you are connecting to MainNet, it is highly recommended you clone and 
build from the [latest release tag](https://github.com/unification-com/mainchain/releases/latest) and **not** the `master` branch.

Clone latest release for running on MainNet

```bash
git clone -b [latest-release-tag] https://github.com/unification-com/mainchain
```

Or clone master for development/testing:

```bash
git clone https://github.com/unification-com/mainchain
```

Then run:

```bash
$ cd mainchain
$ make install
```

### Build from source

Clone latest release for running on MainNet

```bash
git clone -b [latest-release-tag] https://github.com/unification-com/mainchain
```

Or clone master for development/testing:

```bash
git clone https://github.com/unification-com/mainchain
```

Then run:

```bash
$ cd mainchain
$ make build
```

`und` and `undcli` binaries and output to `./build`. This is useful for development and testing.

### Dockerised `und` and `undcli`

The Dockerised binaries can be used instead of installing locally. The Docker container will use the latest release tag to build the binaries.

Build the container:

```bash
docker build -t undd .
```

Example commands, with mounted data directories:

```bash
$ docker run -it -p 26657:26657 -p 26656:26656 -v ~/.und_mainchain:/root/.und_mainchain -v ~/.und_cli:/root/.und_cli undd und init [node_name]
$ docker run -it -p 26657:26657 -p 26656:26656 -v ~/.und_mainchain:/root/.und_mainchain -v ~/.und_cli:/root/.und_cli undd und start
```

## DevNet Development Enviroment

A complete DevNet environment, comprising of 3 EVs, a REST server, a reverse proxy server and several test wallets loaded with FUND is available via Docker Compose compositions  for development and testing purposes. See [DevNet documentation](docs/local-devnet.md) for more detailed information.

## Unit Tests & Chain Simulation

>**Important**: New modules and features should be committed with corresponding unit tests and simulation operations.

### Unit Tests

Unit tests can be run via `go`:

```bash
go test -v ./...
```

or the `make` target:

```bash
make test
```

### Chain Simulation

The `simapp` can be used to simulate a running chain, which is particularly useful during development and testing to check that new features are working as expected in a simulated live chain environment (i.e. many different transactions being executed against the chain). The simulation will produce the specified number of blocks, using the specified number of operations (transactions) per block to simulate a full running chain environment.

For example, the following command will simulate 500 blocks, each with 200 randomly generated transaction operations, checking for invariants every block.

The parameters used to generate the chain, along with the final chain state export and simulation statistics will be saved to the specified `ExportParamsPath`, `ExportStatePath` and `ExportStatsPath` paths respectively.

```
go test -mod=readonly ./simapp \
    -run=TestFullAppSimulation \
    -Enabled=true \
    -NumBlocks=500 \
    -BlockSize=200 \
    -Commit=true \
    -Seed=24 \
    -Period=1 \
    -PrintAllInvariants=true \
    -ExportParamsPath=/path/to/.simapp/params.json \
    -ExportStatePath=/path/to/.simapp/state.json \
    -ExportStatsPath=/path/to/.simapp/statistics.json \
    -Verbose=true \
    -v \
    -timeout 24h
```

### Benchmark testing

CPU and RAM benchmarks can also be generated using the `simapp`, which are useful for checking resources used by modules and features and resolving resource issues. For example, the following will generate a CPU benchmark for a full simulation, using the default block/blocksize values:

```
go test -mod=readonly \
    -benchmem \
    -run=^$ github.com/unification-com/mainchain/simapp \
    -bench ^BenchmarkFullAppSimulation \
    -Commit=true \
    -cpuprofile cpuprofile.out \
    -memprofile memprofile.out \
    -v \
    -timeout 24h
```

#### pprof tools

The profile output can then be analysed using the `pprof` tool:

```
go tool pprof cpuprofile.out
```

using, for example, the following `pprof` commands:

```
(pprof) top
(pprof) list [function]
(pprof) web
(pprof) quit
```
