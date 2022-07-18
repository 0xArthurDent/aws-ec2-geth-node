# Part 1 - Seed data

## Initial setup

Update 
```
sudo apt-get update -y
sudo apt-get clean
sudo apt-get autoclean
```

Verify local storage is labeled as nvme0n1 (if otherwise, modify commands)
```
lsblk
```

Mount volume
```
sudo mkdir /mnt/nvm/
sudo mkfs -t ext4 /dev/nvme0n1
sudo mount -t ext4 /dev/nvme0n1 /mnt/nvm
sudo mkdir /mnt/nvm/ether
sudo chown ubuntu:ubuntu /mnt/nvm/ether
```

Install Geth
```
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update -y
sudo apt-get install ethereum -y
```

Create a **geth.service** file with the contents of [geth.seed-mainnet.service](./geth.seed-mainnet.service)
```
sudo vim /usr/lib/systemd/system/geth.service
```

Start Geth
```
sudo systemctl enable geth
sudo systemctl start geth
```

### Useful Commands

**follow node logs**
```
sudo journalctl -f -u geth
```

**attach to get node**
```
geth attach /mnt/nvm/ether/geth.ipc

/usr/bin/geth attach /mnt/nvm/ether/geth.ipc
```
> :clock4: this may take 1-2 days...please be patient and ensure the node is still running during this period

## Copy data

Format and mount new EBS volume
```
sudo mkdir /mnt/export/
sudo mkfs -t ext4 /dev/xvdf
sudo mount -t ext4 /dev/xvdf /mnt/export/
sudo mkdir /mnt/export/ethereum
sudo chown ubuntu:ubuntu /mnt/export/ethereum
```

Stop geth (we do not want to sync new data whilst the copy is happening)
```
sudo systemctl status geth.service
sudo systemctl disable geth.service
sudo systemctl stop geth.service
```

Copy data
```
cp -r /mnt/nvm/ether/* /mnt/export/ethereum/
```

> :clock4: this may take 1-3 hours - depenings on the IOPS of the EBS Volume

Verify number of files copied match
```
ls /mnt/nvm/ether/geth/chaindata/ | wc -l
ls /mnt/export/ethereum/geth/chaindata/ | wc -l
```

Unmount and detach volume
```
sudo umount /mnt/export
```

At this point we will no longer use the seed node (we will continue running the node on a cheaper EC2 instance). You may go ahead and terminate this EC2 instance unless you would like to seed further blockchain data, eg. Ethereum testnets, then you can continue reading [Seed Testnet data](../readme.md#testnets).