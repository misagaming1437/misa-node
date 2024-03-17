# init dir

    sudo mkdir /cess/hedge 

Create an Environment File
Create a file named .env and specify the necessary environment variables. Here is an example:


    cd /cess/.hedge
    sudo nano .env
 
 copy   

    # Mandatory: Specify the RPC  value 
    RPC=https://www.rpc-berberis.hedgeblock.io
    
    # Optional: Specify the REST_PORT value
    REST_PORT=1317
    
    # Optional: Specify the MINIMUM_GAS_PRICES value
    MINIMUM_GAS_PRICES=0.0025uhedge
    
    # Optional: Specify the NODE_NAME value
    NODE_NAME=nguyentien1437
    
    # Optional: Specify the CHAIN_ID value
    CHAIN_ID=berberis-1
    
    # Optional: Specify the CHAIN_ROOT value
    CHAIN_ROOT=/root/hedge
    
    # Specify the PERSISTENT_PEERS value (if not set, it will be obtained dynamically)
    PERSISTENT_PEERS=15c4b9e4c180dffe096d56aca0a084e551208c87@54.92.167.150:26656
    
    # Optional: Specify the EXTERNAL_ADDRESS value (if not set, it will be obtained dynamically)
    EXTERNAL_ADDRESS=210.18.155.21
    
    # Set SWAGGER_ENABLE to true to enable Swagger documentation for the REST API
    # The default value is false
    SWAGGER_ENABLE=true


Running the Docker Container

    docker run -d -p 9094:9090 -p 26663:26657 -p 26664:26656 -p 1318:1317 -v /cess/hedge:/root/hedge --env-file  /cess/hedge/.env  --name hedge  hedgeblock/berberis:v0.1

Stop docker
        
        sudo docker stop hedge

edit 

    sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025uhedge\"/;" /cess/hedge/berberis-1/config/app.toml
    peers=""
    sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" /cess/hedge/berberis-1/config/config.toml
    seeds="7879005ab63c009743f4d8d220abd05b64cfee3d@54.92.167.150:26656"
    sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" /cess/hedge/berberis-1/config/config.toml
    sed -i 's/max_num_inbound_peers =.*/max_num_inbound_peers = 50/g' /cess/hedge/berberis-1/config/config.toml
    sed -i 's/max_num_outbound_peers =.*/max_num_outbound_peers = 50/g' /cess/hedge/berberis-1/config/config.toml

# To run sell in docker:

    docker exec -it hedge hedged --help

Add New Key

    docker exec -it hedge hedged keys add wallet

Recover Existing Key

    docker exec -it hedge hedged keys add wallet --recover
     
Delete Key

    docker exec -it hedge hedged keys delete wallet
Export Key (save to wallet.backup)

    docker exec -it hedge hedged keys export wallet
Import Key

    docker exec -it hedge hedged keys import wallet wallet.backup
Query Wallet Balance

    docker exec -it hedge hedged q bank balances $(docker exec -it hedge hedged keys show wallet -a) 
Check Balance:

    docker exec -it hedge hedged q bank balances $(docker exec -it hedge hedged keys show wallet -a)
GetPublic key validator:
        
    docker exec -it hedge hedged tendermint show-validator --home /root/hedge/berberis-1

Create a Validator:

    docker exec -it hedge hedged tx staking create-validator --amount=1000000uhedge --pubkey=$(docker exec -it hedge hedged tendermint show-validator --home /root/hedge/berberis-1) --moniker="nguyentien1437" --chain-id=berberis-1 --commission-rate=0.10 --commission-max-rate=0.20 --commission-max-change-rate=0.1 --min-self-delegation=1 --from=wallet --gas-prices=0.025uhedge --gas-adjustment=1.5 --gas=auto -y

Withdraw rewards:

    docker exec -it hedge hedged tx distribution withdraw-rewards $(hedged keys show wallet --bech val -a) --commission --from wallet --chain-id berberis-1 --gas-prices=0.025uhedge --gas-adjustment=1.5 --gas=auto -y
    
Delegate to your self:

    docker exec -it hedge hedged tx staking delegate $(docker exec -it hedge hedged keys show wallet --bech val -a) 1000000uhedge --from wallet --chain-id berberis-1 --gas-prices=0.025uhedge --gas-adjustment=1.5 --gas=auto -y
    
Unjail:

    docker exec -it hedge hedged tx slashing unjail --from wallet --chain-id=berberis-1 --gas-prices=0.025uhedge --gas-adjustment=1.5 --gas=auto -y 
Get Validator Info

    docker exec -it hedge hedged status 2>&1 | jq -r '.ValidatorInfo // .validator_info'
Get Denom Info

    docker exec -it hedge hedged q bank denom-metadata -oj | jq
Get Sync Status

    docker exec -it hedge hedged status 2>&1 | jq -r '.SyncInfo.catching_up // .sync_info.catching_up'
Get Latest Height

    docker exec -it hedge hedged status 2>&1 | jq -r '.SyncInfo.latest_block_height // .sync_info.latest_block_height'
Get Peer

    echo $(docker exec -it hedge hedged tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.hedge/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
Reset Node

    docker exec -it hedge hedged tendermint unsafe-reset-all --home /cess/.hedge/berberis-1 --keep-addr-book






