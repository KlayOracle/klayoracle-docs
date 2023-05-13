# Data provider utilities

KlayOracle contains a couple of utitilties written as Makefiles

Some of the key utilities&#x20;

*   generate adapter ID

    For each feed defined by the data provider, an adapter ID needs to be generated. This ID uniquely identifies that feed when being processed by the node. This also ensures that the oracle contract defined can uniquely identify the adapter.

    To generate the adapter ID:

    * For each data feed you want to define, create the adapter JSON file [within the feed path](https://www.notion.so/For-data-providers-2f23c3a0b2ef47bca10f495851640090). All of the required properties for the adapter file should have been defined, except for the _adapterID_, which should be an empty string.
    * Let’s assume we’ve created two data feeds, KLAY\_USD.json and WEMIX\_USD.json within the feeds folder of our data provider package
    * Run the following command:
      * ADAPTERS=KLAY\_USD.json WEMIX\_USD.json make adapter-id-gen

    The generate adapter ID generates an ID for each of the feeds defined in the ADAPTERS array.

    Once the command runs successfully, the feed file is compressed into 1 line.

    If you’d like to decompress or prettify the output, copy the contents of the file and head to JSONFormatter.
*   dryrun

    This command mimics the [aggregation mechanism](https://www.notion.so/KlayOracle-Docs-31bff55098ce4b6e96c36df329a9d9de) of the data provider and verifies that the adapter configuration defined is correct and would run correctly on the node.

    To dry run,

    * Run the following command:
      * ADAPTERS=KLAY\_USD.json make adapter-dry-run
    * This generates an output
