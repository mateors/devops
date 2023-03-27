### Update Upgrade 
Login To your Ubuntu Server using ssh and install software-properties-common, curl, zip, unzip, git and vnstat using follwoing command
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install software-properties-common
sudo apt-get install curl zip unzip
sudo apt-get install git
sudo apt-get install vnstat
```

#### Create New user and disallow root login
Create your own sudo user. For example I'm creating a username called istiayk you can replace your username instead of istiyak
```
adduser istiayk
usermod -aG sudo istiyak
su - istiyak
sudo whoami
```

#### Locking root for ssh login
For a secuirty reson you can disallow root login
```
sudo nano /etc/ssh/sshd_config
```
Find the following line and change it to no
```
PermitRootLogin no
```
Save and Exit
```
sudo service ssh restart
```

#### Changing SSH port and account lockout policy
Using the default SSH port 22 can make you an easy target for hackers – they often look for open ports through which to intercept and extract sensitive data. Therefore, I recommend changing the SSH port to avoid potential cyber attacks and add extra protection to your Linux server. In my case I'll use 2022 port as SSH port
```
sudo ufw status
# If ufw active then deactive ufw
sudo ufw allow ssh
sudo ufw allow 2022
sudo nano /etc/ssh/sshd_config
```
Port value will change port from 22 to 2022 and MaxAuthTries will lock out IP address if it enters wrong password in more than 5 attempts
```
Port 2022
MaxAuthTries 5
```
Save and Exit
```
sudo service ssh restart
ssh istiyak@xxx.xxx.xxx.xxx -p 2022
```

#### Check Failed ssh login attempt
```
grep "Failed password" /var/log/auth.log
```

#### Firewall setup
A firewall is a way to protect machines from any unwanted traffic from outside. It enables users to control incoming network traffic on host machines by defining a set of firewall rules. These rules are used to sort the incoming traffic and either block it or allow through.
```
sudo ufw status
sudo ufw default allow outgoing
sudo ufw default deny incoming
ufw allow 80,443/tcp
sudo ufw allow from 103.111.224.74
sudo ufw enable
sudo ufw status
```

#### Linux, Apache, MySQL, PHP (LAMP)
```
sudo apt-get install apache2
sudo a2enmod ssl rewrite
sudo apt-get install mysql-server php-mysql
sudo mysql_secure_installtions
sudo apt install php
sudo apt install php-pear php-fpm php-dev php-xdebug php-zip php-curl php-xmlrpc php-gd php-mysql php-mbstring php-xml libapache2-mod-php libapache2-mod-php php-cli
systemctl enable apache2
systemctl start apache2

```

#### Install and setup composer
```
sudo apt update
sudo apt install wget php-cli php-zip unzip
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
HASH="$(wget -q -O - https://composer.github.io/installer.sig)"
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```
#### phpMyAdmin Setup
copy download link from https://www.phpmyadmin.net/downloads/
```
cd /var/www/html
sudo wget https://files.phpmyadmin.net/phpMyAdmin/5.1.1/phpMyAdmin-5.1.1-all-languages.zip
unip phpMyAdmin-5.1.1-all-languages.zip
sudo rm phpMyAdmin-5.1.1-all-languages.zip
mv phpMyAdmin-5.1.1-all-languages pma
```

#### Install Lets Encrypt for SSL certificate 
```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get install letsencrypt
certbot certonly --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory --manual-public-ip-logging-ok --email admin@oservo.com -d '*.oservo.com' -d oservo.com
```

#### Golang Setup

```
wget https://go.dev/dl/go1.20.2.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.2.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
source $HOME/.profile
go version
```

#### Listen Port check 
```
sudo ss -ltn
```

#### Backup System
If you want to create backup your source code and database here is bash script may code hand to you.
Create a mysql_backup.sh file and give it to proper permission and run 

```
sudo nano /home/user/script/mysql_backup.sh
```
And paste the following code
```
# Backup storage directory 
backupfolder=/var/backups
# Notification email address 
recipient_email=<username@mail.com>
# MySQL user
user=<user_name>
# MySQL password
password=<password>
# Number of days to store the backup 
keep_day=30 
sqlfile=$backupfolder/all-database-$(date +%d-%m-%Y_%H-%M-%S).sql
zipfile=$backupfolder/all-database-$(date +%d-%m-%Y_%H-%M-%S).zip 
# Create a backup 
sudo mysqldump -u $user -p$password --all-databases > $sqlfile 
if [ $? == 0 ]; then
  echo 'Sql dump created' 
else
  echo 'mysqldump return non-zero code' | mailx -s 'No backup was created!' $recipient_email  
  exit 
fi 
# Compress backup 
zip $zipfile $sqlfile 
if [ $? == 0 ]; then
  echo 'The backup was successfully compressed' 
else
  echo 'Error compressing backup' | mailx -s 'Backup was not created!' $recipient_email 
  exit 
fi 
rm $sqlfile 
echo $zipfile | mailx -s 'Backup was successfully created' $recipient_email 
# Delete old backups 
find $backupfolder -mtime +$keep_day -delete
```
Cron allows you to schedule this script to run regularly. In order to facilitate this, do as follows:

```
sudo crontab -e
```
Then, add the script path to the end of the string

```
30 22 * * * /home/user/script/mysql_backup.sh
```
Thereafter, your script will be executed every day at 10:30 PM.
