# Part 1 - Seed data

If you have been following this install, the actions below will be carried out on the i3.xlarge EC2 instance

## Cleaning from previous activities 

Unmount and format drive
```
sudo umount /mnt/nvm
sudo mkfs -t ext4 /dev/nvme0n1
sudo mount -t ext4 /dev/nvme0n1 /mnt/nvm
sudo mkdir /mnt/nvm/ether
sudo chown ubuntu:ubuntu /mnt/nvm/ether
```

Modify /usr/lib/systemd/system/geth.service with the contents of [geth.seed-goerli.service](./geth.seed-goerli.service).

Start geth
```
sudo systemctl enable geth
sudo systemctl start geth
```

> :clock4: this may take a few hours...in order to catch up to the highest block

## Copy Data to EBS Volume 

Format and mount new volume
```
sudo mkfs -t ext4 /dev/xvdf
sudo mount -t ext4 /dev/xvdf /mnt/export/
sudo mkdir /mnt/export/ethereum
sudo chown ubuntu:ubuntu /mnt/export/ethereum
```

Stop geth
```
sudo systemctl status geth.service
sudo systemctl disable geth.service
sudo systemctl stop geth.service
```

copy data
> :clock4: this may take 1-3 hours - also, smaller disks have less IOPS which might impact the copy
```
cp -r /mnt/nvm/ether/* /mnt/export/ethereum/
```

Verify number of files copied
```
ls /mnt/nvm/ether/geth/chaindata/ | wc -l
ls /mnt/export/ethereum/geth/chaindata/ | wc -l
```

Unmount and detach volume
```
sudo umount /mnt/export
```