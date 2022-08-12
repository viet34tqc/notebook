# Ubuntu command

<https://github.com/jlevy/the-art-of-command-line#everyday-use>

## Commands

- `man [command]` or `[command] --help`: command --help. Find instruction for command. Ex: `cat --help`
- `cat`: 
  - read the file, combines with `grep`
  - concat files
- `tail`: reads the last ten (default) rows in the file. Usage: read newest bugs in the log file.
  - `tail -n 5 file_name`: read 5 rows
  - `tail -f file_name`: watch the file
- `grep`: search the file
  - `cat file_name | grep key_word`
- `kill`: shutdown a process
  - `kill [PID]`
  - `kill -9 [PID]`: force to kill
- `top`, `htop`: like task manager on window
- `df -h`: display free space
- `free -h`: display free ram

- `sudo apt get [package_name] -y`
- `sudo apt remove [package_name] -y`: Then run `sudo apt autoremove` to remove other dependencies of the packages
- `sudo chown -R ubuntu: .`

## Install nginx, php7.4, mariadb on ubuntu

### Nginx

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

### Firewall setup

```shell
sudo ufw allow ssh
sudo ufw allow http
sudo ufw enable
```

### PHP (7.4)

<https://gist.github.com/nd3w/8017f2e0b8afb44188e733d2ec487deb>

```shell
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php && sudo apt update
sudo apt install -y php7.4 php7.4-fpm php7.4-curl php7.4-gd php7.4-json php7.4-mbstring php7.4-mysql php7.4-opcache php7.4-xml php7.4-xmlrpc php7.4-fileinfo php7.4-imagick php-pear
sudo service php7.4-fpm start
```

Upgrade to php 8: change the 7.4 to 8.0

### MariaDB

`sudo apt install mariadb-server -y`
`sudo mysql_secure_installation`: create new password for root
`sudo apt install -y php7.4 php7.4-fpm php7.4-curl php7.4-gd php7.4-json php7.4-mbstring php7.4-mysql php7.4-opcache php7.4-zip php7.4-xml php7.4-xmlrpc php7.4-fileinfo php7.4-imagick php-pear`

### Create nginx block

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


## Key

- `ctrl + a` & `ctrl + e`: go to start and end of line