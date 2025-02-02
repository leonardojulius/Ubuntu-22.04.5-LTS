## Step 1: Update Your System

Before installing any software, update your package lists:

```bash
sudo apt update && sudo apt upgrade -y
```

## Step 2: Install LAMP Stack (Apache, MySQL, PHP)

Moodle requires a web server, a database, and PHP.

2.1 Install Apache Web Server

```bash
sudo apt install apache2 -y
```

Enable and start Apache:


```bash
sudo systemctl enable apache2
```

```bash
sudo systemctl start apache2
```

2.2 Install MySQL Database Server

```bash
sudo apt install mysql-server -y
```

Secure MySQL installation:

```bash
sudo mysql_secure_installation
```

Follow the prompts:

- `Set up a root password.`
- `Remove anonymous users.`
- `Disallow root login remotely.`
- `Remove test database.`
- `Reload privilege tables.`

2.3 Install PHP and Required Extensions

Moodle requires PHP 8.0+ and some additional extensions

```bash
sudo apt install php php-cli php-common php-mysql php-gd php-xml php-xmlrpc php-curl php-zip php-intl php-mbstring php-soap -y
```

Check the PHP version:

```bash
php -v
```
## Step 3: Create a MySQL Database for Moodle


Log in to MySQL:

```bash
sudo mysql -u root -p
```


Create a database and user:

```sql 
CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'moodleuser'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON moodle.* TO 'moodleuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

Replace your_password with a strong password.

## Step 4: Download and Install Moodle

Navigate to the web root directory:

```bash
cd /var/www/html
```

Download Moodle (latest stable version):


```bash
sudo apt install git -y
```

```bash
wget -O moodle-latest.tgz https://download.moodle.org/download.php/direct/stable403/moodle-latest-403.tgz
```

Then extract:

```bash
tar -xvzf moodle-latest.tgz
```



Move the extracted Moodle folder to the Apache web root:

```bash
sudo mv moodle /var/www/html/
```

Set the correct permissions:

```bash
sudo chown -R www-data:www-data /var/www/html/moodle
```

```bash
sudo chmod -R 755 /var/www/html/moodle
```

## Step 5: Create a Moodle Data Directory

Moodle requires a separate data directory:

```bash
sudo mkdir /var/www/moodledata
```

```bash
sudo chown -R www-data:www-data /var/www/moodledata
```

```bash
sudo chmod -R 770 /var/www/moodledata
```

## Step 6: Configure Apache for Moodle

Edit the default Apache configuration file:

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

Modify the DocumentRoot and add the <Directory> block:

```apache
<VirtualHost *:80>
    ServerAdmin admin@localhost
    DocumentRoot /var/www/html/moodle

    <Directory /var/www/html/moodle>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```

Save and exit (CTRL + X, then Y, then Enter).

Enable mod_rewrite and restart Apache:

```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
```

## Step 7: Install Moodle

1. Open a browser and go to

```url
http://localhost/moodle
```


2. Follow the installation steps:
- Choose MySQL as the database type.
- Enter the database name, user, and password.
- Complete the installation.

## Step 8: Set Up Moodle Cron Job (Optional)

To ensure Moodle runs background tasks properly, set up a cron job:

```bash
sudo crontab -u www-data -e
```

Add this line:

```bash
*/1 * * * * /usr/bin/php /var/www/html/moodle/admin/cli/cron.php >/dev/null 2>&1

```

## Step 9: Access Moodle

```url
http://localhost/moodle
```
