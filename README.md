<div align="center">

# 👨🏻‍💻 **Aztec Sequencer Guide** 👨🏻‍💻

</div>

# 🖥️ Device/System Requirements 

![image](https://github.com/user-attachments/assets/9e7e78a8-ddeb-4e0b-90a8-46f2ab5886b3)



# Pre-Requirements 🛠

- Docker & Docker Compose

   🔺(Let run your docker in background if u are using local Device)

- Aztec Tool

- Sepolia Rpc URL



# Install All Require Dependecies

```
sudo apt-get update && sudo apt-get upgrade -y
```

* Install Node.js 

```
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - && sudo apt update && sudo apt install -y nodejs
```

* Other Packages

```
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev screen ufw -y
```


# Install Docker & Docker Compose


```
sudo apt update && sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```
sudo apt update && sudo apt install -y docker-ce && sudo systemctl enable --now docker
```

```
sudo usermod -aG docker $USER && newgrp docker
```


```
sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r .tag_name)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && sudo chmod +x /usr/local/bin/docker-compose
```


*  Verify installation

```
docker --version && docker-compose --version
```



# Install the Aztec CLI

```
bash -i <(curl -s https://install.aztec.network)
```


* Lets Config it to your corrent Shell/Path

```
echo 'export PATH="$HOME/.aztec/bin:$PATH"' >> ~/.bashrc
```

```
source ~/.bashrc
```

* Verify the Installation with-

```
aztec -h
```


* Set the correct version for the testnet

```
aztec-up 2.1.2
```


# Load your wallet with Sepolia Faucet 

https://sepolia-faucet.pk910.de/

https://www.alchemy.com/faucets/ethereum-sepolia



# Allow Incoming connections on Ports 

```
sudo ufw allow 22
sudo ufw allow ssh
sudo ufw enable
```

```
sudo ufw allow 40400
sudo ufw allow 8080
```

# Get Seapolia and Beacon API Key's Free of Cost

* Go to - https://tinyurl.com/y8v7tm2d

* Manually Sign up with Email address- u will rcvd 100$ voucher- (Dont sign up with Google)

* Select Ethereum Sepolia

* Scroll Down and Select the Growth Plan for which RPC u need, (Sepolia or Beacon) 

* Click on `Select Voucher`

* Select `50$ Voucher`

* Now Click on `Pay with Balance`

* Here we go... U got your API key for free!

* U can see your Api key on API section!

![Screenshot 2025-05-15 172028](https://github.com/user-attachments/assets/409a5c1e-9c2d-4653-a7e0-2e24711d0470)


* More Other Sites!

https://drpc.org/dashboard

https://developer.metamask.io/key/active-endpoints

https://www.alchemy.com


<div  align="center">
   
#  Start Your Sequencer 🍥

</div>

* Create a Screen Session

```
screen -S aztec
```

#### 🔺🔺--- Execute below given command to Start Your node & Dont forget to make changes in it-

```
aztec start --node --archiver --sequencer \
  --network testnet \
  --l1-rpc-urls Eth_Sepolia_RPC \
  --l1-consensus-host-urls Eth-beacon_sepolia_RPC \
  --sequencer.validatorPrivateKeys 0xYourPrivateKey \
  --sequencer.coinbase YourAddress \
  --sequencer.governanceProposerPayload 0xDCd9DdeAbEF70108cE02576df1eB333c4244C666 \
  --p2p.p2pIp Your_ip
```


* Replace `Eth_Sepolia_RPC` with your actual one!         -follow above steps

* Replace `Eth-beacon_sepolia_RPC` with your actual one            -follow above steps


* Replace `0xYourPrivateKey` with your actual EVM wallet pvt key    🔺 (dont forget to add 0x at starting)

* Replace `YourAddress` with your actual evm wallet address

* Replace `Your_ip` with your `External IP`  ... 

     -U can get External IP by running  `curl ifconfig.me`


* It will take few times to download and Sync! 🥶

![Screenshot 2025-05-02 164041](https://github.com/user-attachments/assets/17dd3df2-3136-4dd0-8dde-70cf19291503)


* The Successfull Running Should Look like this 👇


![Screenshot 2025-05-02 172143](https://github.com/user-attachments/assets/37ae2455-8b98-4642-bf14-0f5e1ed90cf2)


# ♦️ Use this Template for saving data:

 ------👇Save These Info/Data👇 ------

Aztec Sequencer Node ( XXXXX dc)

• Ethereum sepolia rpc : 

• Beacon_sepolia_RPC : 

• PVT KEY : 

• MM Public Address : 

• IP ( cloud vps) : 

• Block Number : 

• Base64 encoded string : 

------ 👆Save These Info/Data👆 ------


# Detached and Attached From the Screen

* For detached from screen session - `ctrl` , `a` + `d`

* For Attach - 

```
screen -r aztec
```

<div  align="center">
   
# Get Apprentice Role In dc- 😙

</div>


📋 **Step 1: Get the latest proven block number**

```
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getL2Tips","params":[],"id":67}' \
http://localhost:8080 | jq -r ".result.proven.number"
```

* Save this block number for the next steps

* Example output: `12345`

🔍 **Step 2: Generate your sync proof**

```
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getArchiveSiblingPath","params":["BLOCK_NUMBER","BLOCK_NUMBER"],"id":67}' \
http://localhost:8080 | jq -r ".result"
```

* Replace both `BLOCK_NUMBER` with your: (check step1)

* This will output a long base64-encoded string - (Copy it completely)


✅ **Step 3: Register with Discord**


* join dc- https://discord.gg/aztec 

* Go to `#operators│start-here` Channel

* Type `/operator start` 

![image](https://github.com/user-attachments/assets/bb4985b0-f98a-43ed-b0c1-9f7e95f6de3c)

* Now it will promt u to enter `address` , `block number` , `proof`

* Place your evm wallet address in `address` section

* Place block-number From the `Step-1` 

* Place sync Proof from `Step-2` 


* Success message should look like this! & U will get the role!

![Screenshot 2025-05-02 175859](https://github.com/user-attachments/assets/5db4bbac-a2d5-463c-a9c1-ea7ae18b00a5)

![Screenshot 2025-05-02 180049](https://github.com/user-attachments/assets/cb25480d-01ae-45d7-9017-c269e2cc54a6)



---


<div align="center">

# 📈 Upgrade to v2.1.2  (Non-Validator)🧃

</div>

>All the Sequencers who didn't Verify the ZK-Passport Earlier to came under the Validator set they had to be upgrade the node with below commands: 

 ### 🪜Step-1) Move to Aztec Screen 

```
screen -r aztec
```

 ### 🪜Step-2) Stop your node if already running: with `ctrl+c`


### 🪜Step-3) Delete old data & Update with-:

```
rm -rf ~/.aztec/alpha-testnet/data/
```

```
aztec-up 2.1.2
```

 ### 🪜 Step-4) The Start Command got updated, so use the updated [Start Command](https://github.com/Mayankgg01/Aztec_Sequencer_Guide?tab=readme-ov-file#----execute-below-given-command-to-start-your-node--dont-forget-to-make-changes-in-it-) 


>📣Note-: If your logs are like this: Then you are good to go: Your sequencers working fine: 

 

<img width="2559" height="494" alt="image" src="https://github.com/user-attachments/assets/c9d55a76-99e5-4ad6-8f05-9f3e62ffdeb5" />


---


<div align="center">

# 📈 Upgrade to v2.1.2  (Validator)🧃

</div>

>Follow these Process if you were the Validator in previous network/Stage or had Verify the ZK-PASSPORT: So follow the below process to upgrade.

### 🪜Step-1) Move to Aztec Screen 

```
screen -r aztec
```

 ### 🪜Step-2) Stop your node if already running: with `ctrl+c`


### 🪜Step-3) Delete old data & Update with-:

```
rm -rf ~/.aztec/alpha-testnet/data/
```

```
aztec-up 2.1.2
```

### 🪜Step-4) Install Foundry

```
curl -L https://foundry.paradigm.xyz | bash
```

```
source ~/.bashrc
```

```
foundryup
```

> Check Version

```
cast --version
```


### 🪜Step-5) Approve the Aztec rollup

>To join you'll need to first approve the Aztec rollup to spend the 200k `STAKE` which is the balance required to join as a sequencer.


```
cast send 0x139d2a7a0881e16332d7D1F8DB383A4507E1Ea7A \
"approve(address,uint256)" \
0xebd99ff0ff6677205509ae73f93d0ca52ac85d67 \
200000ether \
--private-key 0xPRIVATE_KEY_OF_OLD_SEQUENCER \
--rpc-url Eth_Sepolia_RPC
```

* Replace `0xPRIVATE_KEY_OF_OLD_SEQUENCER` with your validator private key: which you used previously:

* Replace `Eth_Sepolia_RPC` with your Sepolia RPC:



### 🪜Step-6) Generate New BLS & ETH keys

>Why? We will now used the new generated key to setting-up our Validator:


* Create a sequencer keystore

```
aztec validator-keys new \
  --fee-recipient 0x0000000000000000000000000000000000000000000000000000000000000000
```


> This command Outputs your sequencer's attester addresses and BLS public keys. SAVE THEM (Check SS)


<img width="1379" height="224" alt="Screenshot 2025-11-09 025850" src="https://github.com/user-attachments/assets/0e9056fe-90eb-48d8-89ba-4d88160a7ccb" />



### 🪜Step-7) Save your validator-keys 

* Open & SAVE your Private keys of `ETH` & `BLS`

```
nano ~/.aztec/keystore/key1.json
```

>These keys gonna need for next processes: 


### 🪜Step-8) Join the Validator

>This process will deposit 200k Staking tokens & will register you as a Validator: 

```
aztec \
  add-l1-validator \
  --l1-rpc-urls ETH_RPC \
  --network testnet \
  --private-key 0xPRIVATE_KEY_OF_OLD_SEQUENCER \
  --attester ETH_ATTESTER_ADDRESS \
  --withdrawer ETH_ADDRESS \
  --bls-secret-key BLS_ATTESTER_PRIV_KEY \
  --rollup 0xebd99ff0ff6677205509ae73f93d0ca52ac85d67
```

* Replace `ETH_RPC` with your ETH sepolia RPC

* Replace `0xPRIVATE_KEY_OF_OLD_SEQUENCER` with your old private key, which you have run this node/validator before

* Replace `ETH_ATTESTER_ADDRESS` from the [STEP-6](https://github.com/Mayankgg01/Aztec_Sequencer_Guide?tab=readme-ov-file#step-6-generate-new-bls--eth-keys), Which we have saved (USE ETH Address)

* Replace `ETH_ADDRESS` with any ETH Address, (You can use your same wallet addres which you have put the PVT key)

* Replace `BLS_ATTESTER_PRIV_KEY` with the BLS Private key From [STEP-7](https://github.com/Mayankgg01/Aztec_Sequencer_Guide?tab=readme-ov-file#step-7-save-your-validator-keys)   (❌DONT USE ETH, USE BLS ✔️ )


<img width="2559" height="762" alt="Screenshot 2025-11-09 000410" src="https://github.com/user-attachments/assets/aef3bd84-7e21-49ba-850a-3e9bd088fb0e" />




### 🪜Step-9) Start The Validator/Sequencer

>Now we will start the validator: Read carefully and adjust all the parameters carefully: dont miss-match!

```
aztec start --node --archiver --sequencer \
  --network testnet \
  --l1-rpc-urls Eth_Sepolia_RPC \
  --l1-consensus-host-urls Eth-beacon_sepolia_RPC \
  --sequencer.validatorPrivateKeys 0xYourPrivateKey \
  --sequencer.coinbase YourAddress \
  --sequencer.governanceProposerPayload 0xDCd9DdeAbEF70108cE02576df1eB333c4244C666 \
  --p2p.p2pIp Your_ip
```


* Replace `Eth_Sepolia_RPC` with your 

* Replace `Eth-beacon_sepolia_RPC` with your 

* ⚠️ Replace `0xYourPrivateKey` with the newly created ETH PVT key from [STEP-7](https://github.com/Mayankgg01/Aztec_Sequencer_Guide?tab=readme-ov-file#step-7-save-your-validator-keys)    (DONT USE YOUR OLD PVT KEY HERE)❌

* ⚠️ Replace `YourAddress` with the newly creted ETH Address from [STEP-6](https://github.com/Mayankgg01/Aztec_Sequencer_Guide?tab=readme-ov-file#step-6-generate-new-bls--eth-keys)          (DONT USE YOUR OLD WALLET'S ADDRESS HERE) ❌

* Replace `Your_ip` with your VPS IP

  
DONE✔️✔️✔️

---

👉 Join TG for more Updates: https://telegram.me/cryptogg

If U have any issue then open a issue on this repo or Dm me on TG~

Thank U! 👨🏻‍💻

Happy Coding💗
