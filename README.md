# Nulink

Nulink Node Installation Instructions </br>
### [Official site](https://docs.nulink.org/products/stakers/nulink_worker)

### System requirements: </br>
CPU: 2 Core </br>
RAM: 4 Gb </br>
SSD: 30 Gb </br>
OS: Ubuntu 20.04 LTS </br>

You can use a one-line guide or install the node yourself according to the guide below:
```
wget -O Nulink.sh https://raw.githubusercontent.com/Dikci/Nulink/main/Nulink.sh && chmod +x Nulink.sh && ./Nulink.sh
```

    
### Required packages installation + docker. </br>
```
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl git jq lz4 build-essential
sudo apt install -y unzip logrotate git jq sed wget curl coreutils systemd
sudo apt install make curl tar wget jq build-essential -y
sudo apt install make clang pkg-config libssl-dev -y
apt install ufw -y
ufw allow ssh
ufw allow https
ufw allow http
ufw allow 9151
ufw enable
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```


### Install Node.
```
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.10.23-d901d853.tar.gz
tar -xvzf geth-linux-amd64-1.10.23-d901d853.tar.gz
cd geth-linux-amd64-1.10.23-d901d853/
./geth account new --keystore ./keystore
```

You will need to come up with and remember the password. We will also be given our public key and the path to the folder where the private key will be stored. Go to this directory and save the backup

### Install docker file.
```
docker pull nulink/nulink:latest
cd /root
mkdir nulink
cp /root/geth-linux-amd64-1.10.23-d901d853/keystore/* /root/nulink
chmod -R 777 /root/nulink
```

We come up with and prescribe passwords for the worker and storage
```
export NULINK_KEYSTORE_PASSWORD=your password
export NULINK_OPERATOR_ETH_PASSWORD=your password worker
```

### Initialize the node configuration.
```
docker run -it --rm \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
nulink/nulink nulink ursula init \
--signer keystore:///code/UTC--your name file \
--eth-provider https://bsc-testnet.publicnode.com \
--network horus \
--payment-provider https://bsc-testnet.publicnode.com \
--payment-network bsc_testnet \
--operator-address your adress worker (public key) \
--max-gas-price 10000000000
```

### Start node.
```
docker run --restart on-failure -d \
--name ursula \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
-e NULINK_OPERATOR_ETH_PASSWORD \
nulink/nulink nulink ursula run --no-block-until-ready
docker logs -f ursula
```

### Becoming a Validator

### We receive tokens from the tap in the [website](https://www.bnbchain.org/en/testnet-faucet)

### Create the Validator

Go to the [website](https://dashboard.testnet.nulink.org/)

Setting up a BSC test network

In the wallet tab, we request test tokens

Go to the staking tab and make the transfer application

Enter the number of coins that we will drain, press the Stake button

Scroll down, find the BOND WORKER button, click

In this window, enter the address from our WORKER (public key)

In an hour, our offline status will change to online.

### Update
```
There have been no updates at the moment, as soon as they come out, we will immediately add them to this section.

Current network:Ursula
Current version:v0.5.0
```

### Useful commands
```
sudo docker stop $(sudo docker ps -a -q)
sudo docker rm $(sudo docker ps -a -q)
sudo docker rmi $(sudo docker images -q)
Rm -rf nulink
```
