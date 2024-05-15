#Instal Dependency
~~~
sudo apt update
sudo apt-get install -y make git-core libssl-dev pkg-config libclang-12-dev build-essential protobuf-compiler -y
~~~

#Install Go
~~~
cd $HOME
wget -c -O go1.22.3.linux-amd64.tar.gz https://go.dev/dl/go1.22.3.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.22.3.linux-amd64.tar.gz && sudo rm go1.22.3.linux-amd64.tar.gz
echo 'export GOROOT=/usr/local/go' >> $HOME/.bash_profile
echo 'export GOPATH=$HOME/go' >> $HOME/.bash_profile
echo 'export GO111MODULE=on' >> $HOME/.bash_profile
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile && . $HOME/.bash_profile
sudo cp $(which go) /usr/local/bin
go version
~~~

#Build binary
~~~
cd $HOME
git clone https://github.com/airchains-network/junction.git
cd junction
git checkout v0.1.0
make install
sudo mv junctiond /usr/local/bin
junctiond --help
~~~

#Init validator
~~~
junctiond init <your_moniker> --chain-id junction
~~~

#Download Genesis
~~~
wget https://github.com/airchains-network/junction/releases/download/v0.1.0/genesis.json
cp genesis.json ~/.junction/config/genesis.json
~~~

#Set Minimum Gas Price
~~~
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.001uamf\"|" ~/.junction/config/app.toml
~~~

#Add Seed and Peer
~~~
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"7ebc08bbef4bd2b4da8d881474710a2500854c2b@airchain-t-rpc.noders.services:31656\"/" ~/.junction/config/config.toml
~~~

#Create Service
~~~
tee /etc/systemd/system/junctiond.service > /dev/null << EOF
[Unit]
Description=Junctiond Node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which junctiond) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
~~~

#Enable and Restart Service
~~~
systemctl daemon-reload
systemctl enable junctiond
systemctl start junctiond
journalctl -u junctiond -f -o cat
~~~

#Create Key
~~~
junctiond keys add <your_wallet>
~~~

#Request Faucet
~~~
Go to Airchains Discord and request faucet https://discord.com/invite/airchains
~~~

#Create Validator
~~~
junctiond tx staking create-validator \
  --amount 1000000uamf \
  --commission-max-change-rate "0.05" \
  --commission-max-rate "0.10" \
  --commission-rate "0.05" \
  --min-self-delegation "1" \
  --pubkey=$(junctiond tendermint show-validator) \
  --moniker "<your_moniker>" \
  --website "https://fairstaking.com" \
  --identity "" \
  --details "FairStaking is a professional validator Cosmos universe" \
  --security-contact="" \
  --chain-id junction \
  --from <your_wallet>
~~~
