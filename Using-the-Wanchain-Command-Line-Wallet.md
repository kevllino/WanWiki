## Installing NodeJS

**Pre-requisites:**
- Node v8+
- GWAN (Wanchain Node)

**Mac OS**
**Step 1**: Install Brew using terminal: https://brew.sh

**Step 2**: Run `brew install node`

**Linux**
**Step 1**: Install the node through your package manager. For example on Debian/Ubuntu,  run `apt-get install node`

## Downloading & Running the Wanchain Node

**Step 1**: Download **GWAN (Wanchain Node)** for your OS (in this guide we’ll use MacOS but the process is similiar for Linux): https://github.com/wanchain/go-wanchain/releases/

**Step 2**: Run `wget https://github.com/wanchain/go-wanchain/releases/download/v1.0.2/gwan-mac-amd64-1.0.2-196c3d6d.tar.gz`

**Step 3**: Extract the file: `tar -xvf gwan-mac-amd64-1.0.2-196c3d6d.tar.gz`

**Step 4**: Enter the GWAN folder: `cd gwan-mac-amd64-1.0.2-196c3d6d`

**Step 5**: Run gwan and add `--testnet` if you wish to run on the test network

## Downloading & installing the CLI Wallet

**Step 1**. Download the CLI Wallet: `git clone https://github.com/wanchain/Wanchain_Command_Line_Wallet.git`

**Step 2**: Enter the wallet folder: `cd Wanchain_Command_Line_Wallet`

**Step 3**: Install required packages: `npm install`

**Step 4**: You are now ready to use the Wanchain CLI Wallet: `cd Wanchain_Command_Line_Wallet`

**Note**: If you are using testnet you should edit line 2 in config.js and change “var wanchainNet = '';” to “var wanchainNet = 'testnet';”. Leave blank for mainnet.

## Creating your WAN wallet

**Step 1**: Run `node src/createKeystore.js`

**Step 2**: You will be prompted to enter your password and again for verification
 
Once completed you will see your **Public** and **Private** address displayed which you can use to receive WAN.

## Sending and Receiving WAN

You can send WAN through one command: node src/send.js --address "" --toaddress "" --amount "" --FeeSel "1" --submit "Y" --password ""

* **address**: Enter the Public address that you wish to send from
* **toaddress**: Enter the Public address that you wish to send to
* **amount**: Enter the amount you wish to transfer
* **FeelSel**: Enter the gas price (default or custom options available). Change --FeeSel to "2" if you wish to specify custom gas/gas price and add options --gasLimit "" --gasPrice ""
* **submit**: Confirm the transaction
* **password**: Enter your account password

To Receive WAN, simply share your **Public Address** with the sender. The WANs will automatically appear in your account once the transaction has been processed. 

## Sending Private Transactions

A private WAN transaction can be made using the following command: node src/sendPrivacy.js --address "" --waddress "" --PrivacyAmount "" --FeeSel "1" --submit "Y" --password ""

* **address**: Enter the Private address that you wish to send from
* wtoaddress**: Enter the one time Private address that you wish to send to
* **PrivacyAmount**: Enter the amount you wish to transfer: 10, 20, 50, 100, 200, 500, 1000, 5000, 50000
* **FeelSel**: Enter the gas price (default or custom options available). Change --FeeSel to "2" if you wish to specify custom gas/gas price and add options --gasLimit "" --gasPrice ""
* **submit**: Confirm the transaction
* **password**: Enter your account password

## Receiving Private Transactions

**Step 1**: To receive private address you should provide the sender with your OTA address (waddress in cli - long format address)

**Step 2**: Once the WANs have been sent you can now scan OTAs to update your OTA balance. Run the following command in a new terminal: `cd backend && node wanChainBlockScan.js`
    
**Step3**: Now update your OTA balance by running `node fetchMyOTA.js`

## Backing up your wallet
        
When creating a new wallet, the keystore file is stored in the same directory as when you are using the Wanchain GUI wallet:

* MacOS: /Users/USERNAMEHERE/Library/Wanchain/keystore
* Linux: ~/.wanchain/keystore
* Windows: C:\Users\USERNAM\AppData\Roaming\wanchain

We recommend you store a copy of the keystore folder once you create new wallets in a secure offline location.

## List of support commands

#### Run each `*.js` file as `node *.js or node <filename> without .js` in command line. 
#### For example,

    $ node createKeystore.js
    or
    $ node createKeystore

| File          | Purpose       |   Parameters  |  Command  |
| ------------- | ------------- |-------------|---------|
| createKeystore.js | create new account | `--password  --repeatPass` | ```node createKeystore.js --password  --repeatPass```|
| send.js | send a transaction | `--address  --toaddress --amount --FeeSel  --gasLimit --gasPrice --submit --password` | ```node send.js --address  --toaddress --amount --FeeSel  --gasLimit --gasPrice --submit --password```|
| sendPrivacy.js | send with privacy | `--address  --waddress --PrivacyAmount --FeeSel  --gasLimit --gasPrice --submit --password` | ```node sendPrivacy.js --address  --waddress --PrivacyAmount --FeeSel  --gasLimit --gasPrice --submit --password```|
| transactionList.js | Print transaction list and its details | `--address --transHash` | ```node transactionList.js --address --transHash```|
| fetchMyOTA.js ** | fetch account OTA | `--address --password` | ```$ node fetchMyOTA.js --address --password```|
| refundOTAs.js | refund OTA | `--address  --OTAaddress --FeeSel  --gasLimit --gasPrice --submit --password` | ```node refundOTAs.js --address  --OTAaddress --FeeSel  --gasLimit --gasPrice --submit --password```|
| ordinaryBalance.js | fetch ordinaray balance info | `--address` | ```node ordinaryBalance.js --address```|
| watchToken.js | fetch watch Token balance info | `--address --tokenAddress` | ```node watchToken.js --address --tokenAddress```|
| tokensend.js | send a token transaction | `--address  --tokenAddress --toaddress --amount --FeeSel  --gasLimit --gasPrice --submit --password` | ```node tokensend.js --address  --tokenAddress --toaddress --amount --FeeSel  --gasLimit --gasPrice --submit --password```|
| version.js | print Wanchain_Command_Line_Wallet version |  | ```node version.js```|
| keystorePath.js | print Wanchain keystore path |  | ```node keystorePath.js```|
| tokenBuyStamp.js | buy Stamp used in token privacy transaction| `--address  --stampBalance --FeeSel  --gasLimit --gasPrice --submit --password` | ```node tokenBuyStamp.js --address  --stampBalance --FeeSel  --gasLimit --gasPrice --submit --password```|
| tokenSendPrivacy.js | send a token privacy transaction| `--address  --contractBalance --waddress  --amount --stampOTA --submit --password` | ```node tokenSendPrivacy.js --address  --contractBalance --waddress  --amount --stampOTA --submit --password```|
| fetchTokenOTA.js | fetch token privacy OTAs| `--address  --password` | ```node fetchTokenOTA.js --address  --password```|
| TokenTransactionList.js | list Token privacy Transactions send from me| `--address` | ```node TokenTransactionList.js --address```|
| watchTokenOTA.js | fetch My OTA Balance from Token address and OTA address| `--address  --tokenAddress --OTAAddress` | ```node watchTokenOTA.js --address  --tokenAddress --OTAAddress```|
