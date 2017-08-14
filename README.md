# Vault-Container
Vault running in Docker for Mac with consul backend.
Methods found in this repo will work on non MacOS machines, see caveats where noted.

## Running Vault
### Vault server
The vault server is run through docker compose.  `git clone` the repo to get the docker-compose file. Then, start the vault server:

`docker-compose up -d`

Running `docker ps -a` should now show 3 running containers: vault, consul1, consul2.

### Vault client
You can `brew install vault` to get the vault client.  You will then need to set the vault addr so the client knows where to talk to the server:

`export VAULT_ADDR='http://127.0.0.1:8200'`

If you are not using MacOS then alternative methods of installing the vault cli client can be found at https://www.vaultproject.io/downloads.html.
### Accessing the vault

Run `vault init`.  It will show you 5 Unseal Keys and a Root Token.  Copy them somewhere safe.


To unseal vault and do anything to it you will have to give it 3 different unseal keys:
```
vault unseal   # give it one of the keys
vault unseal   # give it a different key
vault unseal   # give it a third key
```

If you give it an invalid key, you will have to **start over**.  If you give it the same key twice, it only counts as one unseal attempt.

Once it is unsealed, you can log in and do stuff with the vault:

`vault auth`

It will prompt you for the token, it wants  the root token that was provided from vault init.

The ui can be accessed through a web browser at http://127.0.0.1:8500
Further instructions for using vault can be found at https://www.vaultproject.io/docs/index.html

## Starting HA configuration
A second docker compose file called HA-docker-compose.yaml is located in this repo that can be called by specifying the file name as on the command line.

`docker-compose -f HA-docker-compose -d`

This configuration will create two Consul data centers and two instances of Vault. To access the secondary instance of Vault set you VAULT_ADDR variable as follows.

`export VAULT_ADDR='http://127.0.0.1:28200'`

You will also be able to query each datacenter for the active, standby and all vault instances in a data center by querying that data centers's consul service as follows

```
dig +short active.vault.service.consul @127.0.0.1 -p 8600
dig +short standby.vault.service.consul @127.0.0.1 -p 8600
dig +short vault.service.consul @127.0.0.1 -p 8600
```
