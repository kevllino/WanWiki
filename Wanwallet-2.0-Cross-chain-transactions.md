**Notes:**

* With 2.0 release, Wanwallet now supports **cross-chain transactions with Ethereum**. 

* Previous versions of Wanwallet still work as usual after release of Wanwallet 2.0, but new features like cross-chain transactions require Wanwallet 2.0 or later.

* For users of previous versions, we strongly recommend **_backing up keystore files_** in a safe location before upgrading to Wanwallet 2.0 to prevent any kind of data loss, though the upgrading process will not alter existing keystore files in any way.

* This instruction guide is for Wanwallet 2.0 GUI in which users can send ETH to Wanchain and withdraw ETH from Wanchain to Ethereum.

* Users can choose between ETH/WAN main network and test network.

* This version combines ETH light wallet and WAN full node wallet.

**Terminology**

* **WETH**: Wanchain Ethereum Crosschain Token, ETH symbol on Wanchain.

* **HTLC**: Hashed Timelock Contracts.




# 1) Download

Download Wanwallet 2.0 installation file from https://wanchain.org/product

*Wanwallet 2.0 supports **Linux, Windows, MacOS**, please download corresponding installation file for your OS.

# 2) Installation and launch

Extract the file, run, and click the "LAUNCH APPLICATION" button to start the wallet

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_launch.PNG)


For Linux users: 

Setup by CLI: `sudo dpkg -i WanWalletGui-linux64-2.X.X.deb`

* Start by CLI for main network: `wanwalletgui`                

* Start by CLI for test network: `wanwalletgui --network testnet`

* Or click Wanwalletgui to start under `/usr/local/bin/`

# 3) Toggle between main net and test network

Wanwallet 2.0 works with both main and test networks of Wanchain and Ethereum.

Below you can see the Wanchain accounts overview page (also the default start-up page), you can access it anytime by clicking on the Wanchain logo on the upper-left corner

Use the menu option below (Develop--->Network) to switch between main network (default network) and test network.

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_toggle_network.PNG)



# 4) Create/Import accounts

These 2 options allow you to create new WAN or new ETH account. 

The "CREATE ACCOUNT" button in the upper-right corner only creates new WAN account

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_create_account.PNG)

You can also import existing WAN or ETH account using wallet keystore file

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_create_account.PNG)

Backup feature opens local keystore files location of WAN or ETH accounts



# 5) Cross-chain transactions

Crosschain transactions are under this tab

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_crosschain.PNG)

Before you make a crosschain transaction, please check WAN and ETH balance in your account in Wanwallet GUI or with links below.

Main network：

* ETH: https://etherscan.io/

* WAN: https://www.wanscan.org/


Test network：

* ETH: https://rinkeby.etherscan.io/

* WAN: http://13.58.108.244/


# 6) Send ETH to Wanchain

Click the "ETH >> WETH" tab below to send ETH to Wanchain

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_ETHtoWanchain.PNG)

Enter your password then press “OK” button to send the transaction.

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_sendTransaction.PNG)


# 7) Confirm/Cancel the transaction

In the "Transaction history" tab, click on the “**Confirm**" button to finalize the cross-chain transaction process once it _**turned red**_.

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_confirm_cancel_transaction.PNG)

If you do not confirm before the HTLC countdown ends, it means you choose to cancel the transaction and refund the ETH from the locked account. 
The "Confirm" button changes to "**Cancel**" and you can click it to cancel the transaction once it _**turned red**_


***

Once the “Confirm” button turned **red**, click it to access “**Confirm Transaction**” page.

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_confirm_transaction_1.PNG)

Enter the password then click “OK” button to finalize transaction 

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_confirm_transaction_2.PNG)


***

Once the “Cancel” button turned **red** (after countdown ends), click it to access “**Cancel Transaction**” page.

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_cancel_transaction_1.PNG)

Enter the password then click “OK” button to cancel the transaction 

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_cancel_transaction_2.PNG)


# 8) Send WETH to Ethereum

If you have ETH balance on Wanchain (WETH balance), you can send WETH back to Ethereum.

Click the "WETH >> ETH" tab below to perform this kind of cross-chain transaction.    

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_WETHtoETH.PNG)

The process is similar to the ETH to Wanchain one, please refer to section 7 for details about how to confirm or cancel the transaction.


# 9) Normal Ethereum transactions

You can also perform normal Ethereum transaction in Wanwallet GUI.

Click the "Normal transaction" tab below, fill source and destination accounts, transaction amount, fee preference, then click "SEND"

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_ETHtoETH.PNG)
