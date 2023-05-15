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

* [Install & run data provider locally](../data-providers/install-and-run-data-providers-locally.md)
* Install & run node locally

## Installing from Docker

The [KlayOracle team](https://github.com/KlayOracle/klayoracle-monorepo#contributors) is actively working on Docker images for faster installation & setup of nodes and data providers. Pending a full launch, an experimental version has been released, along with a video guide to get started.

{% embed url="https://www.youtube.com/watch?v=Tu1QkayeXYk" %}

This documentation will be updated in the near future with step-by-step instructions for using KlayOracle with Docker.
