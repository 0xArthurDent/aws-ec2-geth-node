# Part 2 - Run Node

## Initial setup : Part 1

Update 
```
sudo apt-get update -y
sudo apt-get clean
sudo apt-get autoclean
```

Install Geth
```
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update -y
sudo apt-get install ethereum -y
```

## Install Nginx : Part 2

Install Nginx 
> :exclamation: It is recommended to associate an Elastic IP to the EC instance. A domain(or sub-domain) is also needed to implement this solution. 
```
sudo apt-get install --reinstall ca-certificates
sudo update-ca-certificates
sudo apt-get update -y
sudo apt-get install nginx apache2-utils -y
sudo apt-get install python3-certbot-nginx -y
```

*Replace with your domain name*
> :exclamation: Needs port 80 and 443 allowing incoming traffic on the EC2 instance

Provision certificates
```
sudo certbot --nginx -d example.com
```

To automatically renew your certificate add this line to /etc/crontab file:
```
@monthly root certbot -q renew
```

Test cert renewal
```
sudo certbot renew --dry-run
```

Generate nginx user and password
```
sudo htpasswd -c /etc/nginx/htpasswd.users Your_User
```

Edit the NGINX configuration file /etc/nginx/sites-available/default and replace with the contents found here [Nginx Default](./nginx/default)


Test Nginx config and restart if successful
```
sudo nginx -t
sudo service nginx restart
sudo systemctl status nginx
```

**For further Nginx hardening please read** [Nginx Hardening](../../nginx/nginx-hardening.md)

## Run Node  : Part 3

To verify local storage is called nvme1n1 (if otherwise, modify next code section)
```
lsblk

sudo mkdir /mnt/ebs/
sudo mount -t ext4 /dev/nvme1n1 /mnt/ebs/
sudo chown ubuntu:ubuntu /mnt/ebs/ethereum
```

Create a **geth.service** file with the contents of [geth.run-goerli.service](./geth.run-goerli.service)
```
sudo vim /usr/lib/systemd/system/geth.service
```

Start Geth
```
sudo systemctl enable geth
sudo systemctl start geth
```