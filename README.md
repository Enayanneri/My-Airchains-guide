# My Airchains guide
### 1. **Prepare Your Server**
   - **Recommended Specifications:**
     - CPU: 4 cores
     - RAM: 8 GB
     - Storage: 200 GB SSD
     - OS: Ubuntu 20.04 or 22.04

   - **Update your system:**
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

   - **Install necessary dependencies:**
     ```bash
     sudo apt-get install git curl build-essential jq gcc snapd lz4 tmux bc -y
     ```

### 2. **Install Go**
   - Remove old Go version if installed:
     ```bash
     sudo rm -rf /usr/local/go
     ```
   - Install the latest Go:
     ```bash
     wget https://go.dev/dl/go1.21.6.linux-amd64.tar.gz
     sudo tar -xvf go1.21.6.linux-amd64.tar.gz -C /usr/local
     echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> ~/.bash_profile
     source ~/.bash_profile
     ```

### 3. **Download and Configure Airchains Node Software**
   - Download the junction node binary:
     ```bash
     wget https://github.com/airchains-network/junction/releases/download/v0.1.0/junctiond
     chmod +x junctiond
     sudo mv junctiond /usr/local/bin
     ```
   - Initialize the node with your chosen moniker (node name):
     ```bash
     junctiond init <moniker>
     ```

### 4. **Configure Genesis and Peers**
   - Download the genesis file:
     ```bash
     wget https://github.com/airchains-network/junction/releases/download/v0.1.0/genesis.json
     cp genesis.json ~/.junction/config/genesis.json
     ```
   - Set up seed nodes and peers:
     ```bash
     SEEDS="" PEERS="2d1ea4833843cc1433e3c44e69e297f357d2d8bd@5.78.118.106:26656"
     sed -i -e "s/^seeds =.*/seeds = \"$SEEDS\"/" $HOME/.junction/config/config.toml
     sed -i -e "s/^persistent_peers =.*/persistent_peers = \"$PEERS\"/" $HOME/.junction/config/config.toml
     ```

### 5. **Start the Node**
   - Launch the node using `screen` to keep it running in the background:
     ```bash
     screen -S airchains-node
     junctiond start
     ```

### 6. **Create a Wallet**
   - Create a wallet for your validator:
     ```bash
     junctiond keys add <wallet-name>
     ```

### 7. **Request Faucet Tokens**
   - To run a validator, you'll need testnet tokens. Request tokens via the project's Discord bot or faucet service by sending your wallet address.

### 8. **Create the Validator**
   - After your node is fully synchronized (check with `junctiond status`), use the following command to create your validator:
     ```bash
     junctiond tx staking create-validator \
       --amount=1000000amf \
       --pubkey=$(junctiond tendermint show-validator) \
       --moniker=<YOUR_VALIDATOR_NAME> \
       --chain-id=junction \
       --fees=200amf \
       --from=<YOUR_WALLET_NAME>
     ```
