# **Install Dependencies**
```
$ sudo apt install -y git clang curl libssl-dev llvm libudev-dev make protobuf-compiler
```
# **Open port**
```
$ sudo ufw allow ssh; sudo ufw allow 30333
```
# **Download Binary & Copy to /usr/bin**
```
$ wget -O /usr/bin/tangle https://github.com/webb-tools/tangle/releases/download/v0.6.1/tangle-testnet-linux-amd64  && sudo chmod +x tangle /usr/bin/tangle
```
# **Create a service**
Change  All <b>YourName</b> to your name
```
sudo tee /etc/systemd/system/tangle.service > /dev/null << EOF
[Unit]
Description=Tangle Validator Node
After=network-online.target
StartLimitIntervalSec=0

[Service]
User=$USER
Restart=always
RestartSec=3
ExecStart=/usr/bin/tangle \
  --base-path $HOME/.tangle/data/validator/YourName \
  --name YourName \
  --chain tangle-testnet \
  --auto-insert-keys \
  --port 30333 \
  --telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
  --validator \
  --no-mdns

[Install]
WantedBy=multi-user.target
```
# **Enable service**
```
$ sudo systemctl daemon-reload
$ sudo systemctl enable tangle
$ sudo systemctl start tangle
```

# **Check the logs**
```
$ sudo journalctl -u tangle -f --no-hostname -o cat
```
- Example output :
```
2023-03-22 14:55:51 Tangle Standalone Node
2023-03-22 14:55:51 ✌️  version 0.6.1-xxxx-aarch64-macos
2023-03-22 14:55:51 ❤️  by Webb Technologies Inc., 2017-2023
2023-03-22 14:55:51 📋 Chain specification: Tangle Testnet
2023-03-22 14:55:51 🏷  Node name: cooing-morning-2891
2023-03-22 14:55:51 👤 Role: FULL
2023-03-22 14:55:51 💾 Database: RocksDb at /Users/local/Library/Application Support/tangle-standalone/chains/local_testnet/db/full
2023-03-22 14:55:51 ⛓  Native runtime: tangle-standalone-115 (tangle-standalone-1.tx1.au1)
2023-03-22 14:55:51 Bn254 x5 w3 params
2023-03-22 14:55:51 [0] 💸 generated 5 npos voters, 5 from validators and 0 nominators
2023-03-22 14:55:51 [0] 💸 generated 5 npos targets
2023-03-22 14:55:51 [0] 💸 generated 5 npos voters, 5 from validators and 0 nominators
2023-03-22 14:55:51 [0] 💸 generated 5 npos targets
2023-03-22 14:55:51 [0] 💸 new validator set of size 5 has been processed for era 1
2023-03-22 14:55:52 🔨 Initializing Genesis block/state (state: 0xfd16…aefd, header-hash: 0x7c05…a27d)
2023-03-22 14:55:52 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2023-03-22 14:55:53 Using default protocol ID "sup" because none is configured in the chain specs
2023-03-22 14:55:53 🏷  Local node identity is: 12D3KooWDaeXbqokqvEMqpJsKBvjt9BUz41uP9tzRkYuky1Wat7Z
2023-03-22 14:55:53 💻 Operating system: macos
2023-03-22 14:55:53 💻 CPU architecture: aarch64
2023-03-22 14:55:53 📦 Highest known block at #0
2023-03-22 14:55:53 〽️ Prometheus exporter started at 127.0.0.1:9615
2023-03-22 14:55:53 Running JSON-RPC HTTP server: addr=127.0.0.1:9933, allowed origins=["http://localhost:*", "http://127.0.0.1:*", "https://localhost:*", "https://127.0.0.1:*", "https://polkadot.js.org"]
2023-03-22 14:55:53 Running JSON-RPC WS server: addr=127.0.0.1:9944, allowed origins=["http://localhost:*", "http://127.0.0.1:*", "https://localhost:*", "https://127.0.0.1:*", "https://polkadot.js.org"]
2023-03-22 14:55:53 discovered: 12D3KooWMr4L3Dun4BUyp23HZtLfxoQjR56dDp9eH42Va5X6Hfgi /ip4/192.168.0.125/tcp/30304
2023-03-22 14:55:53 discovered: 12D3KooWNHhcCUsZTdTkADmDJbSK9YjbtscHHA8R4jvrbGwjPVez /ip4/192.168.0.125/tcp/30305
2023-03-22 14:55:53 discovered: 12D3KooWMr4L3Dun4BUyp23HZtLfxoQjR56dDp9eH42Va5X6Hfgi /ip4/192.168.88.12/tcp/30304
2023-03-22 14:55:53 discovered: 12D3KooWNHhcCUsZTdTkADmDJbSK9YjbtscHHA8R4jvrbGwjPVez /ip4/192.168.88.12/tcp/30305
```
- Example Output after sync:
```
2021-06-17 03:07:39 🔍 Discovered new external address for our node: /ip4/10.26.16.1/tcp/30333/ws/p2p/12D3KooWLtXFWf1oGrnxMGmPKPW54xWCHAXHbFh4Eap6KXmxoi9u
2021-06-17 03:07:40 ⚙️  Syncing 218.8 bps, target=#5553764 (17 peers), best: #24034 (0x08af…dcf5), finalized #23552 (0xd4f0…2642), ⬇ 173.5kiB/s ⬆ 12.7kiB/s
2021-06-17 03:07:45 ⚙️  Syncing 214.8 bps, target=#5553765 (20 peers), best: #25108 (0xb272…e800), finalized #25088 (0x94e6…8a9f), ⬇ 134.3kiB/s ⬆ 7.4kiB/s
2021-06-17 03:07:50 ⚙️  Syncing 214.8 bps, target=#5553766 (21 peers), best: #26182 (0xe7a5…01a2), finalized #26112 (0xcc29…b1a9), ⬇ 5.0kiB/s ⬆ 1.1kiB/s
2021-06-17 03:07:55 ⚙️  Syncing 138.4 bps, target=#5553767 (21 peers), best: #26874 (0xcf4b…6553), finalized #26624 (0x9dd9…27f8), ⬇ 18.9kiB/s ⬆ 2.0kiB/s
2021-06-17 03:08:00 ⚙️  Syncing 37.0 bps, target=#5553768 (22 peers), best: #27059 (0x5b73…6fc9), finalized #26624 (0x9dd9…27f8), ⬇ 14.3kiB/s ⬆ 4.4kiB/s
```
# **Bond TNT and setup validator Account**
- Go to <a href="https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc-archive.tangle.tools#/accounts">PolkadotJS</a>
- Create 2 New Account, 1 for Stash 1 for Controller
<img src="https://bafkreie3p7fux2fh355v33aabjhosnc3eutb647kl7gxqtumzgdn2zy5hu.ipfs.nftstorage.link/">
- Choose Stash & Controller account, then fill the amount you want to stake
- Then back to your VPS
- Generate session key
```
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9944
```
- Copy the 0x Output
- Back to PolkadotJS
- Click that, Set Session Key
<img src="https://bafkreicijclnnpavidqimo6lqepbeviiol7jutkrugsnw2kadtliwjxlum.ipfs.nftstorage.link/">
- Fill it with the 0x Output before
- Then Validate
# **Setting identity**
- Go to <a href="https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc-archive.tangle.tools#/accounts">PolkadotJS</a>
- Open the 3 dots next to your address: Set on-chain Identity
- Enter all fields you want to set.
# **Request judgment**
- Go to <a href="https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc-archive.tangle.tools#/accounts">PolkadotJS</a>
- Select your account and extrinsic type: identity | requestJudgment

<img src="https://airdrops.io/wp-content/uploads/2019/03/Tangle-LOGO.png">

# **Contact Me🔥☕**
<p align="left">
<a href="https://www.github.com/0xGilang"><img height="40" width="40" align="center" src="https://avatars.githubusercontent.com/u/94946818?v=4"></a>
<a href="https://www.facebook.com/Gilangbuanasultoni" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/facebook.svg" alt="AryaTrickers2020" height="30" width="40" /></a>
<a href="https://wa.me/6282131561458?text=Halo+Bang+Gilang" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/whatsapp.svg" alt="6289694295787" height="30" width="40" /></a>
<a href="https://twitter.com/Gilangsultoni"><img height="40" width="40" align="center" src="https://static.dezeen.com/uploads/2023/07/x-logo-twitter-elon-musk_dezeen_2364_col_0.jpg"></a>

------
------
