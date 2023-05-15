# Configuration Files & Environmental Variables

The data provider's configuration files and environmental variables are both mechanisms for conveying the data provider's configuration information to its operating environment (e.g. the host server, connecting node, or KlayOracle protocol). However, unlike config files, environmental variables contain secret values and [shouldn't be committed to your code repository](https://dev.to/somedood/please-dont-commit-env-3o9h).&#x20;

In KlayOracle, data provider configuration files are defined within the `data-providers/config.yml` file, while environmental variables are defined in the `data-providers/.env` file.

{% hint style="info" %}
To get up and running quickly, edit the sample [config](https://github.com/KlayOracle/klayoracle-monorepo/blob/development/data-provider/config.yaml) file, and copy the contents of [env.example](https://github.com/KlayOracle/klayoracle-monorepo/blob/development/data-provider/.env.example) to your `data-providers/.env` file.
{% endhint %}

config.yml

env

service\_node



config.yaml

<table data-full-width="false"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td>env</td><td>specifies the environment in which the data provider is currently running.<br><br>Choices:<br>- dev<br>- stage<br>- prod</td></tr><tr><td>service_node</td><td>the IP address of the node which the data provider is connected to.</td></tr><tr><td><strong>feed</strong><br><br><code>dictionary</code></td><td></td></tr><tr><td>    path<br><br>    <code>string</code></td><td>Path to the folder where the data provider's adapters (<em>aka</em> data feeds) are defined.<br><br>This path is relative to the data-providers folder, </td></tr><tr><td>adapters<br><br><code>list</code></td><td>list of data feeds (aka adapters) you want the node to aggregate. <br><br>each data feed must match the file name in the folder defined in the "path" property. <br><br>Any adapter not defined within this list will not be aggregated by the node, even if its <a href="../configuring-data-feeds.md"><code>active</code></a> property is set to true. the </td></tr><tr><td>organization</td><td></td></tr><tr><td>k_org_id</td><td></td></tr><tr><td>name</td><td></td></tr><tr><td>website</td><td>Your organization website.</td></tr><tr><td>ssl</td><td></td></tr><tr><td>key</td><td>This should be left blank</td></tr><tr><td>certificate</td><td>Data providers require the node's self-signed public certificate to communicate securely with the node via SSL (Secure Socket Layer).<br><br>This file should be saved within the <code>data-provider/certs/node</code> folder.<br><br><img src="../../.gitbook/assets/image (2) (1).png" alt=""></td></tr></tbody></table>

* Relative path to the folder where the adapters (or data feeds) are defined. This path is&#x20;
* The list of adapters

To reference environmental variables as values in your config.yaml file (or other , use the syntax ${ENV\_VAR}.

