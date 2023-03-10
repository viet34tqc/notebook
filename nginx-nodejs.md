# Setup nodejs project on ubuntu

## Install Node using NVM

<https://tecadmin.net/how-to-install-nvm-on-ubuntu-20-04/>

Install nvm

```bash
sudo apt install curl 
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash 
source ~/.bashrc // Reload the bash to apply the change
```

Install lts version of node. In order to install the latest one, use `nvm install node`

```bash
nvm install --lts 
```

To switch back to LTS version: `nvm use --lts`
To remove a Node version: `nvm uninstall [NODE_VERSION]`

With nvm, you can ignore `sudo` when using npm

## (Optional) Install Node without NVM

This step is optional, just in case you want to use `sudo npm` because `sudo npm` isn't recognized when you nvm
<https://www.hostinger.com/tutorials/how-to-install-node-ubuntu>

To install the current release, execute the following command. Remember to replace the 18.x value with your preferred Node.js version:

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
```

## Install and setup nginx

Install nginx

```bash
sudo apt update
sudo apt install nginx
```

Setup new project:

Create a file with name as your domain in `/etc/nginx/site-available`

```bash
server {
  listen 80;
  listen [::]:80;

  server_name demo-node-api.vietnguyenwp.com;

  root /var/www/demo-node-api.vietnguyenwp.com;
  index index.html;

  location / {
    proxy_pass http://localhost:3001; #whatever port your app runs on
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
  }
}
```

Then you need to enable that site:

`sudo ln -s /etc/nginx/sites-available/demo-node.vietnguyenwp.com /etc/nginx/sites-enabled/demo-node.vietnguyenwp.com`

Save it and exit.

Test nginx config and make sure there's no error

`sudo nginx -t`

then restart the service

`sudo systemctl reload nginx`

test nginx:

`curl localhost`

server block for react

```bash
server {
  listen 80;
  listen [::]:80;

  server_name demo-node.vietnguyenwp.com;

  root /var/www/demo-node.vietnguyenwp.com;
  index index.html;

  location / {
    try_files $uri $uri/ /index.html;
  }
  error_page 404 =200 /index.html;
}
```

## Setup Firewall

**NOTES: If you are using oracle, don't use ufw** <https://stackoverflow.com/questions/62326988/cant-access-oracle-cloud-always-free-compute-http-port>

Use this instead
```bash
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 443 -j ACCEPT
sudo netfilter-persistent save
```

```bash
sudo ufw enable
sudo ufw status
sudo ufw allow ssh (Port 22)
sudo ufw allow http (Port 80)
sudo ufw allow https (Port 443)
```

## Modify .env file

## Install PM2 process manager to keep the app running

PM 2 helps to create, terminate, and even monitor a Node.js application

`npm i -g pm2`

Create pm2 instance:

```bash
pm2 start --name api index.js
pm2 startup ubuntu 
```

## Install yarn

```bash
sudo apt remove cmdtest
sudo apt remove yarn # Only need if you already install yarn
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
sudo apt-get install yarn -y
yarn install
```

## Setup SSL using certbot

Go to https://certbot.eff.org/ and do as the instructions. When install Certbot, chose `sudo certbot --nginx`, it will automatically set up the SSL