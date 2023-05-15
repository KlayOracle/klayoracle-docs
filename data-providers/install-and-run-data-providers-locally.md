# Install & run data providers locally

## **System requirements**

* Mac/Linux machine, with at least 4GB of RAM and 10GB of storage.
* [Go](https://go.dev/doc/install) version 18 and above.
* [Cockroach DB](https://www.cockroachlabs.com/docs/stable/install-cockroachdb-mac.html), an SQL database system highly compatible with Go.

## **Installation Steps**

After cloning the [klayoracle-monorepo](https://github.com/KlayOracle/klayoracle-monorepo) as described in the [fundamentals](broken-reference), follow these steps to install & run your data provider:

### 1. Define data feeds

From the monorepo, navigate to the `feeds` folder within the `data-provider` package.

```bash
# from monorepo folder
cd data-provider/feeds
```

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt="" width="253"><figcaption><p><code>feeds</code> folder in monorepo</p></figcaption></figure>

Configure the data feeds (_aka_ adapters) which your data provider will use to fetch and serve data to subscribed consumer contracts. Each data feed includes options for connecting to your data source via HTTP(S), filtering your data and returning a single value using reducers, and sending the value to your oracle contract.

Visit the [Configuring Data Feeds](broken-reference) section of this documentation for step-by-step instructions on defining each of your data feeds.

{% hint style="info" %}
We've also provided sample data feeds for [KLAY\_USD](https://github.com/KlayOracle/klayoracle-monorepo/blob/development/data-provider/feeds/KLAY\_USD.json) and [WEMIX\_USD](https://github.com/KlayOracle/klayoracle-monorepo/blob/development/data-provider/feeds/WEMIX\_USD.json), which you can edit in order to get started quickly.
{% endhint %}

### 2. Generate a unique adapter ID for each feed

Using the `make` command, run the `adapter-id-gen` utility, which generates a unique 32-byte string identifier for each feed defined as a variable within the `ADAPTERS` argument.

```bash
make adapter-id-gen ADAPTERS="ETH_USD.json WEMIX_USD.json"
```

For example, the above command generates separate string identifiers for the `ETH_USD.json` and `WEMIX_USD.json` feeds. Each string is saved within its respective feed as an `adapterId` property.

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption><p>run <code>adapter-id-gen</code></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p><code>adapterId</code> in feed</p></figcaption></figure>

{% hint style="info" %}
Running the `adapter-id-gen` utility compresses and uglifies the feed's JSON. Use a [JSON Formatter](https://jsonformatter.curiousconcept.com/) to revert the file's contents to a more readable format.&#x20;
{% endhint %}

### 3. Deploy OracleProvider contract

Once the feeds' adapter IDs have been created, follow the instructions in the video below to deploy the data provider's oracle contract. This contract sends data on-chain, which is then [consumed by valid data subscribers](../fundamentals/architecture.md).

{% embed url="https://www.youtube.com/watch?v=7cW9Ryro6pI" %}

An separate `OracleProvider` contract should be deployed for each feed. Once deployed, replace the `oracleAddress` key in each feed with the key of its deployed contract.

<figure><img src="../.gitbook/assets/image (5).png" alt="" width="563"><figcaption><p><code>oracleAddress</code> in <code>LTC_USD</code> feed</p></figcaption></figure>

### 4. Do a dry run

After defining your data feeds and generating an adapter ID, the next step is to do a dry run.

A dry run verifies that the configured data feeds are valid and will function correctly when the data provider is [connected to a KlayOracle node](broken-reference) in a production environment.

It does this by simulating the execution of a node to fetch, aggregate and return data using the properties defined within the data feed.

Run the `adapter-dry-run` command, passing the list of data feeds you want to verify to the `ADAPTERS` argument. For example:

```bash
make adapter-dry-run ADAPTERS="ETH_USD.json WEMIX_USD.json"
```

A successful dry run fetches data from defined data sources, filters the data using the reducers defined in the adapter, and returns the single value which the oracle contract consumes in a production environment.

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption><p>output of <code>adapter-dry-run</code> command</p></figcaption></figure>

{% hint style="info" %}
`adapter-id-gen` and `adapter-dry-run` are two of several data provider utilities defined within KlayOracle. Visit the [Utilities](broken-reference) page to learn more.
{% endhint %}

### 5. Define parameters to connect with node

In order to connect successfully, node runners are required to provide connecting data providers with specific config values, to be used within the data provider's `config.yml` & `data-providers/.env` files.

These parameters include:

* the node's IP address.
* the node's public certificate
* the node's OAuth token

<details>

<summary>Node IP address</summary>

The connected node's IP address is defined in the data provider's `data-provider/config.yml` file as a `service_node` parameter.

<img src="../.gitbook/assets/image (4).png" alt="" data-size="original">

</details>

<details>

<summary>Node SSL public certificate</summary>

Data providers require the node's self-signed public certificate to communicate securely with the node via SSL (Secure Socket Layer).

Once provided by the node, the certificate file should be saved by the data provider within the `data-provider/certs/node` folder. For example, in the image below, the public certificate file `535797165684.pem` is saved in the `x509` folder (`x509` being an arbitrary name given to the node by the data provider).

![](<../.gitbook/assets/image (7).png>)

Once saved, the file path should also be defined within the `data-provider/config.yml` file as a `certificate` value.

![](<../.gitbook/assets/image (6).png>)

</details>

<details>

<summary>Node OAuth Token</summary>

This credential authenticates the data provider to connect to the node and communicate securely via gRPC.&#x20;

This token should be kept secret and stored as an environmental variable in the `data-providers/.env` file.

<img src="../.gitbook/assets/image (2).png" alt="" data-size="original">

</details>

{% hint style="info" %}
To learn more about nodes and data providers communicating securely via SSL & OAuth, see [Node & data provider communication](../fundamentals/node-and-data-provider-communication.md).
{% endhint %}

### 6. Define other config & environmental variables

Apart from the connected node's parameters, the `data-providers/config.yml` file and `data-providers/.env` file need to be configured with other details of the data provider.

The mandatory parameters include:

* the data provider's organization details (name, website, and organization ID).
* the list of feeds you want the node to aggregate. Any feed name not defined in this list will not be sent to the node.
* the DNS details of the data provider, particularly its IP address.

<figure><img src="../.gitbook/assets/image (8).png" alt="" width="375"><figcaption><p>data-providers/config.yml</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="375"><figcaption><p>data-providers/.env</p></figcaption></figure>

See [Configuration Files & Environmental Variables](how-it-works/configuration-files-and-environmental-variables.md) for a comprehensive guide on setting up these parameters.

### 7. Start data provider

Once the above have been completed, run the following commands to start the data provider on your local machine.

The `HOST_IP` parameter must match the one set in your `.env` file, as described in the [previous step](install-and-run-data-providers-locally.md#5.-define-other-config-and-environmental-variables).

```bash
make gomodtidy
# start data provider without logging to console
make dp-client-nolog HOST_IP=0.0.0.0:50002
# to start with comprehensive logging
make dp-client HOST_IP=0.0.0.0:50002
```
