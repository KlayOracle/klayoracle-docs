# Configuring Data Feeds

{% hint style="info" %}
This section is currently being developed. To contribute, open a pull request on the [documentation GitHub repo](https://github.com/KlayOracle/klayoracle-docs).
{% endhint %}

A data feed is a stream of structured data that provides smart contracts with up-to-date information from one or more data sources. Examples of data feeds include: the current market price of a particular token, e.g. KLAY, or the current weather in Seoul, South Korea, or the latest departing flight from JFK Airport.

In KlayOracle, data feeds are defined by data providers. Each data feed is a JSON file where the requests to data sources are configured, as well as the necessary manipulations to make on the returned data before being sent to the blockchain.

{% hint style="info" %}
KlayOracle currently supports only price feeds. Weather, sports and flight data feeds are coming soon.
{% endhint %}

For a quick&#x20;

*   Within the `data-provider/feeds` folder, create a feed file in JSON.

    💡 View the KLAY\_USD.json sample feed for a reference on how to define your feeds.💡 KlayOracle currently only supports price feeds. Weather, sports and flight data feeds are coming soon.

Properties

|   |   |
| - | - |
|   |   |
|   |   |
|   |   |

* The properties of the feed file are as follows:
  * `active`. This determines if the node will process the feed. For a node
  * name: The name of the price feed. This name must match the file name.
  *   `adapterId`: This is the unique ID for the feed. Because each data provider, the adapterID is how the node recognizes each feed being processed for the data provider.

      This should be left blank. An adapter ID will be generated when you run the generate adapter ID make command. Instructions for generating the adapter ID are here.
  * `oracleAddress`: This is the address of the deployed oracle contract for this feed. Instructions for deploying the oracle contract and getting the address here.
  * `category`: for price feeds, the category needs to be set as 2. Other feed categories are as follows:
    * 1 —
    * 2 —
  * frequency: this value, in milliseconds, defines how often you want the node to aggregate the data.
  *   feeds: an array of objects, where each object defines the data sources. Each data provider requests data from data sources through HTTP requests.

      Data source properties:

      * url: link to the API endpoint which provides the data for that feed.
      * request-type: designates the type of request being made to the API endpoint. 0 for GET, 1 for POST request
      * headers: array of objects. each object is a field
        * field: to pass additional context and metadata about the request or response from the data source’s API. Example of a field: `content-type: application/json`
        * Secret values used in request headers should be defined in the .env file (link to .env file), and referenced in the following way: `${SECRET_ENV_VALUE}`
      * payload: if a request payload is being sent for a POST request, it should be written here
      * reducers: array of objects, where each object defines the data manipulation function/method to run on each returned state. Read on the reducers section to understand the suitable properties to use within this object.
* Secret values used in field property values should be defined in the .env file (link to .env file), and referenced in the following way: `${SECRET_ENV_VALUE}`



`Data feeds wont run without:`

* listing them when running make adapter
* defining them in the config.yaml
