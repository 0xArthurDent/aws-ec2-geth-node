# AWS EC2 Geth Node

---

# Ethereum Mainnet

## Initial data sync

- Provision an AWS i3.xlarge instance (for a fast initial data sync using its local storage) - Ubunutu 22.04 LTS x64 was used for this install
- SSH into the server
- Run the commands found in [Part 1 - Seed data](./mainnet/part-1-seed-data.md#initial-setup)

> :exclamation: Continue once the node reaches highest block

## Copy blockchain data to EBS Volume

- Provision an EBS Volume - for purposes of this install a 1TB (gp2) volume was provisioned
- Attach volume to the running EC2 instance
- Run the commands found in [Part 1 - Copy Data](./mainnet/part-1-seed-data.md#copy-data)

## Prepare to run node
- Provision a new AWS instance, for the purposes of this install an m5.xlarge EC2 instance was chosen
- SSH into the server
- Run the commands found in [Part 2 - Run Node](./mainnet/part-2-run-node.md#initial-setup) to install geth
- Run the commands to install nginx and create a basic config [Part 2 - Run Node](./mainnet/part-2-run-node.md#install-nginx)
- It is recommend to secure any servers you are running, further security configuration can be found on [Nginx Security](./nginx-hardening.md)
- Mount EBS Volume with copied blockchain data [Part 2 - Run Node](./mainnet/part-2-run-node.md#run-node)
- Configure Geth to run as a service
- Start Geth

## Using your node

### Adding to Metamask as custom network:

RPC URL: https://*nginx_UserName* **:** *nginx_Password* **@** *your domain name* 

example: https://user:password@node.example.com

---


# Testnets

> :exclamation: Optional steps in order to sync ethereum testnets

One option of getting a testnet node would be to follow the above but replace the geth.service fileto run with a testnet argument [testnets](./testnets/)

Another option would be to continue the process right after syncing the and re-using the provisioned resources. The below follows the latter and is an example for Goerli testnet

# Georli Testnet

## Initial data sync
- No need to provision anything, will re-use the i3.xlarge EC2 instance
- SSH into server
- Run the commands found in [Part 1 - Seed data](./testnets/goerli/part-1-seed-data.md#initial-setup)

## Copy blockchain data to EBS Volume

- Provision an EBS Volume - for purposes of this install a 200GB (gp2) volume was provisioned
- Attach volume to the running EC2 instance
- Run the commands found in [Part 1 - Copy Data](./testnets/goerli/part-1-seed-data.md#copy-data)

## Prepare to run node

- Provision a new AWS instance, for the purposes of this install a t3.medium instance was chosen to keep running costs low.
- SSH into the server
- Run the commands found in [Part 2 - Run Node](./testnets/goerli/part-2-run-node.md#initial-setup) to install geth
- Run the commands to install nginx and create a basic config [Part 2 - Run Node](./testnets/goerli/part-2-run-node.md#install-nginx)
- It is recommend to secure any servers you are running, further security configuration can be found on [Nginx Hardening](./nginx/nginx-hardening.md)
- Mount EBS Volume with copied blockchain data [Part 2 - Run Node](./testnets/goerli/part-2-run-node.md#run-node)
- Configure Geth to run as a service
- Start Geth

---
