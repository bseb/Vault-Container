# Vault-Container
Vault running in docker with consul backend

## Running Vault
### Vault server
The vault server is run through docker compose.  `git clone` the repo to get the docker-compose file. Then, start the vault server:

`docker-compose up -d`

Running `docker ps -a` should now show 3 running containers: vault, consul1, consul2.

### Vault client
You can `brew install vault` to get the vault client.  You will then need to set the vault addr so the client knows where to talk to the server:

`export VAULT_ADDR='http://127.0.0.1:8200'`

### Accessing the vault

Run `vault init`.  It will show you 5 Unseal Keys and a Root Token.  Copy them and do:

`export VAULT_TOKEN={root token you were given}`

To unseal vault and do anything to it you will have to give it 3 different unseal keys:
```
vault unseal   # give it one of the keys
vault unseal   # give it a different key
vault unseal   # give it a third key
```

If you give it an invalid key, you will have to **start over**.  If you give it the same key twice, it only counts as one unseal attempt.

Once it is unsealed, you can log in and do stuff with the vault:

`vault auth`

It will prompt you for the token, it is the same root token as above.
