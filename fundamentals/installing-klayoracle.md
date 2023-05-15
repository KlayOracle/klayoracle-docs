# Installing KlayOracle

_KlayOracle v1_ is still in active development, therefore the recommended installation method is by running from source.

## Installing from source

### System Requirements

* [Install Go](https://go.dev/doc/install) version 18 and above.
* Mac/Linux machine, with at least 4GB of RAM and 10GB of storage
* Docker desktop

To get started installing KlayOracle, clone the [klayoracle-monorepo](https://github.com/KlayOracle/klayoracle-monorepo.git) locally.

```bash
git clone https://github.com/KlayOracle/klayoracle-monorepo.git
cd klayoracle-monorepo
```

KlayOracle is written in Go. Go code is grouped into packages, and packages are grouped into modules. Each module specifies dependencies needed to run the code, including the Go version and the set of other modules it requires.

While installing from source, the data provider and nodes need to be configured and run through their respective packages within the monorepo.

* Install & run data provider locally
* Install & run node locally

## Installing from Docker

The [KlayOracle team](https://github.com/KlayOracle/klayoracle-monorepo#contributors) is actively working on Docker containers to speed up the installation & setup of nodes & data providers. The estimated launch date for this is June 1, 2023.
