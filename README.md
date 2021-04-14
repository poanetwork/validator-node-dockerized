# How to launch POA Core/Sokol validator node with Docker Compose and OpenEthereum

1. Install Docker Engine and Docker Compose following the original instructions https://docs.docker.com/get-docker/ and https://docs.docker.com/compose/install/

2. Clone this repo:

    ```bash
    $ git clone https://github.com/poanetwork/validator-node-dockerized
    $ cd validator-node-dockerized
    ```

3. If you are POA Core validator, change the following lines in the `docker-compose.yml`:

    - `--chain=poasokol` to `--chain=poacore`
    - `- ./key:/root/data/keys/poasokol/key` to `- ./key:/root/data/keys/poacore/key`
    - `WS_SERVER: "https://sokol-netstat.poa.network/api"` to `WS_SERVER: "https://core-netstat.poa.network/api"`

4. To be a validator, you need to have a mining address and a private key for it. Name your JSON keystore file as `key` and put it to the `validator-node-dockerized` directory. Put keystore's password to `password` file.

5. Copy `.env.example` to `.env` and configure the `.env` file. There are a few settings you need to define:

    ```
    ETHSTATS_ID=[validator_name]
    ETHSTATS_CONTACT=[contact_email]
    ETHSTATS_SECRET=[netstat_secret_key]
    EXT_IP=[server_external_ip]
    ACCOUNT=[0x_your_mining_address]
    ```

    - `ETHSTATS_ID` - The displayed name of your validator in [Sokol Netstats](https://sokol-netstat.poa.network/) or [Core Netstats](https://core-netstat.poa.network/).
    - `ETHSTATS_CONTACT` - Validator's contact (e.g., e-mail).
    - `ETHSTATS_SECRET` - Secret key to connect to Netstat (should be provided by POA team, please, request it).
    - `EXT_IP` - External IP of the current server.
    - `ACCOUNT` - Your mining address (with leading `0x`).

6. Start your node and Netstat service.

    ```bash
    $ docker-compose up -d
    ```

After docker containers are created, the node will sync with the chain (may take a while).

To restart you need to use `docker-compose stop` and `docker-compose start` being in the `validator-node-dockerized` directory.
