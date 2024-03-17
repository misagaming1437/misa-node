# Create link

    sudo ln -s /cess/.hedge ~

Create an Environment File
Create a file named .env and specify the necessary environment variables. Here is an example:


cd ~/.hedge
sudo nano .env

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

docker run -d -p 9094:9094 -p 26663:26663 -p 26664:26664 -p 1318:1318 -v /cess/.hedge:/root/hedge --env-file  /path/to/env-file  --name hedge_node hedgeblock/berberis:v0.1 
