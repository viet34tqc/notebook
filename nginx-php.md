# Setup nginx, php7.4, mariadb on ubuntu

## Nginx

```shell
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

`vi sites-available/default`

Add or change this line:

```shell
location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
}
```
Save it and exit.

Test nginx config and make sure there's no error

`sudo nginx -t`

then restart the service

`sudo systemctl reload nginx`

test nginx

`curl localhost`

## Firewall setup

**NOTES: If you are using oracle, don't use ufw** <https://stackoverflow.com/questions/62326988/cant-access-oracle-cloud-always-free-compute-http-port>

Use this instead
```bash
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 443 -j ACCEPT
sudo netfilter-persistent save
```

If you are using something else, here is how to turn on uwf
```shell
sudo ufw enable
sudo ufw status
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
```

## PHP (7.4)

<https://gist.github.com/nd3w/8017f2e0b8afb44188e733d2ec487deb>

```shell
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php && sudo apt update
sudo apt install -y php7.4 php7.4-fpm php7.4-curl php7.4-gd php7.4-json php7.4-mbstring php7.4-mysql php7.4-opcache php7.4-xml php7.4-xmlrpc php7.4-fileinfo php7.4-imagick php-pear
sudo service php7.4-fpm start
```

Upgrade to php 8: change the 7.4 to 8.0

## MariaDB

`sudo apt install mariadb-server -y`
`sudo mysql_secure_installation`: create new password for root
`sudo mariadb`

## Create sites-available and enable

Create a file with name as your domain in `/etc/nginx/site-available`, like test.com

```shell
server {
    listen 80;
    listen [::]:80;

    root /var/www/test.com;
    index index.php index.html index.htm;

    server_name test.com;

    location / {
      # If using wordpress: https://thachpham.com/wordpress/wordpress-tutorials/tong-hop-cac-cau-hinh-nginx-cho-wordpress.html
        # try_files $uri $uri/ /index.php?$args; 
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
    }
}
```

Then you need to enable that site:

`sudo ln -s /etc/nginx/sites-available/demo-node.vietnguyenwp.com /etc/nginx/sites-enabled/demo-node.vietnguyenwp.com`

Then restart nginx: `sudo systemctl reload nginx`