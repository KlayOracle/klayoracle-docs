# Install & run node locally

{% hint style="info" %}
This section is currently being developed. To contribute, open a pull request on the [documentation GitHub repo](https://github.com/KlayOracle/klayoracle-docs).
{% endhint %}

* Generate a selfsigned certificate for each node. [see guide using open ssl](https://github.com/KlayOracle/klayoracle-monorepo/blob/development/setup-guide/openssl).
* Use different authority or wildcard matching authority of each node you intend to run.
* In `node/config.yml`, set the `key` and the `pem` you used to generate the certificate.
* Give the certificate pem file to data providers for authenticating with your Node. You can also run your own data providers.
* Update your organization details in `node/config.yml`. Website must match authority used to sign certificate, an example will be if authority is `*.origineum.com`, website can be `node-1.origineum.com`. Otherwise, node service will fail to start.
* In `.env`, `HOST_IP` is the dns for reaching your node. If you're running a docker container, you can override it and other `env` variable by passing from terminal.
* In `.env` `PRIVATE_KEY` is your Node signer with enough Klay tokens to pay for request.
* In `.env` `COCKROACH_DNS_URL` is your full connection string to [https://cockroachlabs.cloud/](https://cockroachlabs.cloud/) cluster. A free account will suffice for a considerable period.
* In `.env` `OAUTH_TOKEN` is the Oauth token given to data providers using your Node.
* Run `make gomodtidy`, `make node-tables`, `make node-server-nolog HOST_IP=0.0.0.0:50054` or `make node-server HOST_IP=0.0.0.0:50054` on your local machine to test it locally.
