## Download binary
### linux
Download from our download page
### windows
TBD
### mac
TBD

## Build from source
Download source code from github

`git clone https://github.com/wanchain/go-wanchain.git`

Install latest distribution of Go (v1.8) if you don't have it already

Building geth requires Go and C compilers to be installed

`sudo apt-get install -y build-essential golang`

Finally, build the geth program using the following command.

`cd go-wanchain`

`make geth`

You can now run build/bin/geth to start your node.

## Build in Docker
Install [Docker](https://www.docker.com/)if you don't have it yet.

Execute the following command to enter docker container using our prebuild docker image
`./dev_setup.sh`

Or, you can build yourself's docker image and enter docker environment.
`./dev_mkimg.sh`

