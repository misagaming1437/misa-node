# init dir

    sudo mkdir /cess/.hedge 

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

    docker run -d -p 9094:9090 -p 26663:26657 -p 26664:26656 -p 1318:1317 -v /cess/.hedge:/root/hedge --env-file  /cess/.hedge/.env  --name hedge  hedgeblock/berberis:v0.1

Stop docker
        
        sudo docker stop hedge

edit 

    sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025uhedge\"/;" /cess/.hedge/berberis-1/config/app.toml
    peers=""
    sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" /cess/.hedge/berberis-1/config/config.toml
    seeds="7879005ab63c009743f4d8d220abd05b64cfee3d@54.92.167.150:26656"
    sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" /cess/.hedge/berberis-1/config/config.toml
    sed -i 's/max_num_inbound_peers =.*/max_num_inbound_peers = 50/g' /cess/.hedge/berberis-1/config/config.toml
    sed -i 's/max_num_outbound_peers =.*/max_num_outbound_peers = 50/g' /cess/.hedge/berberis-1/config/config.toml

    


