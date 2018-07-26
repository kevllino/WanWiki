**Notes:**

With 2.0 release, Wanwallet now supports cross-chain transactions with Ethereum. 

Previous versions of Wanwallet still work as usual after release of Wanwallet 2.0, but new features like cross-chain transactions require Wanwallet 2.0 or later.

For users of previous versions, we strongly recommend **backing up keystore files** in a safe location before upgrading to Wanwallet 2.0 to prevent any kind of data loss, though the upgrading process will not alter existing keystore files in any way.

This instruction guide is for Wanwallet 2.0 GUI in which users can send ETH to Wanchain and withdraw ETH from Wanchain to Ethereum.

Users can choose between ETH/WAN main network and test network.

This version combines ETH light wallet and WAN full node wallet.

**Terminology**

**WETH**: Wanchain Ethereum Crosschain Token, ETH symbol on Wanchain.

**HTLC**: Hashed Timelock Contracts.




# 1) Download

Download Wanwallet 2.0 installation file from https://wanchain.org/product

*Wanwallet 2.0 supports **Linux, Windows, MacOS**, please download corresponding installation file for your OS.

# 2) Installation and launch

Extract the file, run, and click the "LAUNCH APPLICATION" button to start the wallet

![](https://raw.githubusercontent.com/albert-fu/images_for_github/master/test/Wanwallet_launch.PNG)


For Linux users: 

Setup by CLI: `sudo dpkg -i WanWalletGui-linux64-2.X.X.deb`

Start by CLI for main network: `wanwalletgui`                

Start by CLI for test network: `wanwalletgui --network testnet`

Or click Wanwalletgui to start under `/usr/local/bin/`

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



