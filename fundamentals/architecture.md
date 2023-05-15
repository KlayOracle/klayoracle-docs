# Architecture

KlayOracle is written in Go, a statically typed, high-level programming language known for its [speed and performance](https://www.bairesdev.com/blog/why-golang-is-so-fast-performance-analysis/).

KlayOracle has a highly modular architecture, where the main components of the network are broken into separate packages, each of which is responsible for a core functionality, and can be operated independently of other packages.

In most oracle implementations, data sources and nodes are tightly coupled. Node operators are responsible for subscribing to data sources, setting up data feeds, performing off-chain computations to aggregate data, as well as securely sending this data to the blockchain via the oracle contract, all within the same node.

This leads to the following challenges:

* it increases the amount of costs & resources involved to run nodes, as there is an additional responsibility to source and pay for data sources in order to run data feeds.
* in cases where nodes are unable to acquire enough data sources, CPU resources are wasted due to a low number of data feeds, hence making the idea of running nodes unattractive.
* it leaves the node as a single point of failure, such that a compromised node has a significant effect on the entire oracle network.

KlayOracle's goal is to decrease the amount of resources needed to be involved as a participant in an oracle network. It would be more difficult to achieve true scale and decentralization if one party (the node runner) is responsible for performing computations as well as incurring the costs required to subscribe to multiple data sources.

The KlayOracle network consists of the following components/participants:

* Data Provider - data providers configure data feeds, subscribe to multiple data sources via API requests to retrieve data for these feeds, and configuring oracle contracts to provide data to consumers, such as smart contracts.
* Node - data providers outsource their computation to nodes, which are responsible for retrieving data from the data sources configured by the data provider, and then filtering and aggregating the data.
* Oracle Contract - the oracle contract, which is configured by the data provider, is the main on-chain component of KlayOracle, and relays filtered and aggregated data to consuming smart contracts.
* Data Consumer (or subscriber) - data subscribers are entities (i.e., smart contracts) that need external information from oracles to complete specific on-chain actions. This external information is relayed through the oracle contract.

The relationship between the components is illustrated in the diagram below.

<figure><img src="../.gitbook/assets/image (1) (1).png" alt="" width="563"><figcaption><p>KlayOracle v1 Architecture</p></figcaption></figure>

### Features

* KlayOracle's architecture is highly modular in order to maximize the resources of each participant in the network. In KlayOracle, Data Providers run independently from nodes.
* Data providers are responsible for subscribing to multiple data feeds, configuring the data aggregation mechanism, and setting up the parameters of the oracle contract. Node operators are responsible for actually making network requests to data sources, and processing computations according to the data provider's configurations.
* Data providers can configure their respective data feeds and oracle contracts and subscribe to one or several **verified nodes** to fetch & compute these data based on the provider’s configuration.
* They subscribe to these node runners by paying them a monthly fee (either in KLAY, Klaytn’s native token, or KlayOracle’s soon-to-be-launched tokens) to run the computations necessary to fetch data feeds and maintain their truthiness on-chain.
* Data subscribers (smart contract, API services, Wallets) in turn subscribe to Data Providers in order to access on-chain data, by being whitelisted on the Oracle Contract deployed by the Data Provider.
* Data providers and nodes communicate securely using gRPC, a high-performance Remote Procedure Call (RPC) framework that uses the [Protocol Buffer format](https://grpc.io/docs/what-is-grpc/introduction/#working-with-protocol-buffers) to serialize data shared between nodes & data providers.
* Within the gRPC implementation, nodes and data providers use SSL and OAuth to authenticate, communicate securely and transmit data between each other.

{% hint style="info" %}
For more information on setting up SSL and OAuth credentials on a node for secure communications with data providers, read the [following guide](node-and-data-provider-communication.md).
{% endhint %}

With a highly modular architecture which separates the tasks and resources involved in participating within the network, KlayOracle aims to achieve the following:

* dramatically reduce&#x20;
