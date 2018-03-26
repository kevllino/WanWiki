## Download binary
download latest version from https://github.com/wanchain/go-wanchain/releases/

## Build from source
Download source code from github

`git clone https://github.com/wanchain/go-wanchain.git`

Install latest distribution of Go (v1.9) from golang official website if you don't have it already

Building gwan requires Go and C compilers to be installed

`sudo apt-get install -y build-essential `

Finally, build the gwan program using the following command.

`cd go-wanchain`

`make gwan`

You can now run build/bin/gwan to start your node.

## Docker quick start
One of the quickest ways to get gwan up and running on your machine is by using Docker:
```
docker run -d --name wanchain-node -v /home/ubuntu/ethereum:/root \
           -p 8545:8545 -p 17717:17717 \
           wanchain/client-go --rpc
```

