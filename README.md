# Berachain Node Setup
```markdown
# Berachain Validator Node Setup Guide  
*By Alezz1x*  

---

## Requirements  

### System Specifications  
- **Operating System:** Ubuntu 20.04/22.04  
- **CPU:** 4 cores or higher  
- **RAM:** 8 GB or higher  
- **Disk Space:** 200 GB SSD or higher  
- **Network:** Stable broadband with open ports  

### Installed Tools  
- **Git**  
- **Docker & Docker Compose**  
- **Go** (version 1.20+)  
- **Make**  

---

## Steps to Deploy  

### 1. Prepare Your Server  
Update the system and install required packages:  
```bash  
sudo apt update && sudo apt upgrade -y  
sudo apt install -y git curl build-essential jq unzip  
```  

Install Go:  
```bash  
wget https://go.dev/dl/go1.20.5.linux-amd64.tar.gz  
sudo tar -C /usr/local -xzf go1.20.5.linux-amd64.tar.gz  
echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.bashrc  
source ~/.bashrc  
go version  
```  

Install Docker:  
```bash  
sudo apt install -y docker.io docker-compose  
sudo systemctl enable --now docker  
sudo usermod -aG docker $USER  
```  

---

### 2. Clone the Repository  
Clone the official Berachain repository:  
```bash  
git clone https://github.com/berachain/berachain.git  
cd berachain  
```  

---

### 3. Build the Binary  
Build the blockchain binary:  
```bash  
make install  
```  
Verify the installation:  
```bash  
berad version  
```  

---

### 4. Initialize the Node  
Set up your node:  
```bash  
berad init "YourNodeName" --chain-id berachain-mainnet  
```  

---

### 5. Download Genesis and Configure Peers  
Download the genesis file:  
```bash  
wget -O $HOME/.berachain/config/genesis.json https://raw.githubusercontent.com/berachain/mainnet/main/genesis.json  
```  

Update peers in the `config.toml` file:  
```bash  
nano $HOME/.berachain/config/config.toml  
```  

Add persistent peers in the `persistent_peers` section. Check the latest peers [here](https://www.berachain.com/docs).  

---

### 6. Optimize Performance  
Enable pruning for better performance:  
```toml  
pruning = "custom"  
pruning-keep-recent = "100"  
pruning-keep-every = "0"  
pruning-interval = "10"  
```  

---

### 7. Start the Node  
Run the node:  
```bash  
berad start  
```  
Monitor logs:  
```bash  
journalctl -u berad -f  
```  

---

### 8. Create a Validator  
Generate a validator key:  
```bash  
berad keys add validator --keyring-backend os  
```  

Create the validator:  
```bash  
berad tx staking create-validator \  
  --amount=1000000ubera \  
  --pubkey=$(berad tendermint show-validator) \  
  --moniker="YourNodeName" \  
  --chain-id=berachain-mainnet \  
  --commission-rate="0.10" \  
  --commission-max-rate="0.20" \  
  --commission-max-change-rate="0.01" \  
  --min-self-delegation="1" \  
  --from=validator \  
  --fees=5000ubera \  
  --yes  
```  

---

### 9. Monitor and Maintain  
Check sync status:  
```bash  
berad status | jq '.SyncInfo'  
```  


Good luck with your validator journey! 🚀  
