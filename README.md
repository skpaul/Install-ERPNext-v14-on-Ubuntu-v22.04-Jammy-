# 

# Install ERPNext v14 on Ubuntu v22.04 (Jammy)

A guide for beginners.

# Update Ubuntu

```shell
sudo apt-get update -y
```

# Upgrade Ubuntu

```shell
sudo apt-get upgrade -y
```

# Install necessary library

We're going to install in a single call-

```shell
sudo apt-get install git python3-dev python3.10-dev python3-setuptools python3-pip python3-distutils python3.10-venv software-properties-common mariadb-server mariadb-client redis-server xvfb libfontconfig wkhtmltopdf libmysqlclient-dev curl -y
```

# Install MariaDB

```shell
sudo mysql_secure_installation
```

- Enter current password for root: (Enter your SSH root user password)
- Switch to unix_socket authentication [Y/n]:  `Y`
- Change the root password? [Y/n]:   `Y`
- New password:         //This can be different from the SSH root user password.
- Re-enter new password:
- Remove anonymous users? [Y/n]   `Y`
- Disallow root login remotely? [Y/n]:  `N`  //we might want to access the database from a remote server for using business analytics software.
- Remove test database and access to it? [Y/n]:  `Y`
- Reload privilege tables now? [Y/n]:    `Y`

## Configure my.cnf 

```shell
sudo nano /etc/mysql/my.cnf
```

Add the following lines as-it-is at the end of the file-

```
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
[mysql]
default-character-set = utf8mb4
```

## Restart MariaDB

```shell
sudo service mysql restart
```

# Install Node

```shell
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install 16.15.0
```

# Install Node Package Manager

```shell
sudo apt-get install npm -y
sudo npm install -g yarn
```

# Install bench

bench is the command line tool of frappe framework.

```shell
sudo pip3 install frappe-bench
bench init --frappe-branch version-14 frappe-bench
cd frappe-bench

```

# Create your first site

Must use `.localhost` if you want to visit locally.

```shell
bench new-site mysite.localhost   
```

# Download frappe apps

The first app we will download is the `payments` app. This app is required when setting up `ERPNext`.

```shell
bench get-app payments
bench get-app --branch version-14 --resolve-deps erpnext
bench get-app hrms   //optional
```

# Install the apps

```shell
bench --site mysite.localhost install-app payments
bench --site mysite.localhost install-app erpnext
```

# Start bench

```shell
bench start
```

# Visit your site

- **Login-** Administrator.
- **Pass-**  You set during installation.

```https
http://mysite.localhost:8000
```

# Create Another Site

If you not inside the `frappe-bench` directory, go to there-

```shell
cd frappe-bench
bench new-site yousite.localhost   
bench --site yoursite.localhost install-app payments
bench --site yoursite.localhost install-app erpnext
```

Then visit `http://yoursite.localhost:8000`

# Restart bench

If you shutdown/restart your testing server, then you need to start the bench again. Go to frappe-bench directory -

```shell
cd frappe-bench
bench start
```

