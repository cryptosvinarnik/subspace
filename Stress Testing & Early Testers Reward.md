# Update node for Stress Testing


**Setting the node name**

```NODE_NAME="node name"```

**Stop the node**

```sudo systemctl stop subspace-node```

**Update the service file**

```
printf "[Unit]
Description=Subspace Node
After=network.target
[Service]
Type=simple
User=subspace
ExecStart=$(which subspace-node) \\
            --chain testnet \\
            --wasm-execution compiled \\
            --execution wasm --bootnodes \"/dns/farm-rpc.subspace.network/tcp/30333/p2p/12D3KooWPjMZuSYj35ehced2MTJFf95upwpHKgKUrFRfHwohzJXr\" \\
            --rpc-cors all \\
            --rpc-methods unsafe \\
            --ws-external \\
            --validator \\
            --telemetry-url \"wss://telemetry.polkadot.io/submit/ 1\" \\
            --telemetry-url \"wss://telemetry.subspace.network/submit 1\" \\
            --name $NODE_NAME
Restart=always
RestartSec=10
LimitNOFILE=10000
[Install]
WantedBy=multi-user.target" > /etc/systemd/system/subspace-node.service
```

Restart the node

```
sudo systemctl daemon-reload
sudo systemctl restart subspace-node
sudo systemctl restart subspace-farmer
```

Sometimes there may be errors with the user and blocks. I advise you to study the errors in more detail, but you can still use my method:

*Fix error with a group*
```
sudo addgroup p2p 
sudo adduser subspace --ingroup p2p --disabled-password --disabled-login --shell /usr/sbin/nologin --gecos ""
```

*Fix error with a blocks*
```
subspace-farmer erase-plot
```

After that we do a restart.

## Useful Commands

Logs:
```
journalctl -u subspace-node -f

journalctl -u subspace-farmer -f
```

Address output to the console:
```subspace-farmer identity view```

Help:
```subspace-farmer --help```
