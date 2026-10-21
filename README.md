# verify system
1. hostname
2. cat /etc/os-release
3. uptime
4. ip addr
# Update and upgrade the system
1. sudo apt update
2. sudo apt upgrade -y
3. sudo apt autoremove -y
# Verify the OS and package status:
1. lsb_release -a
# Install the LAMP stack
LAMP stands for Linux, Apache, MySQL, and PHP.
1. sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql -y
# Start and enable the services:
1. sudo systemctl enable --now apache2
2. sudo systemctl enable --now mysql
# Verify that Apache and MySQL are running:
1. sudo systemctl status apache2 --no-pager
2. sudo systemctl status mysql --no-pager
# Check the installed versions:
1. apache2 -v
2. mysql --version
3. php -v
# Test Apache locally:
curl -I http://localhost
Note: If you want to access Apache from your browser, allow inbound HTTP traffic on port 80 in your cloud firewall/security group.
# Create a database and dedicated user
Open the MySQL shell:
1. sudo mysql
NOTE: Run the following SQL commands, replacing the example database and username if needed.

sql
CREATE DATABASE appdb;

CREATE USER 'appuser'@'localhost'
IDENTIFIED BY 'REPLACE_WITH_A_STRONG_PASSWORD';

GRANT ALL PRIVILEGES ON appdb.*
TO 'appuser'@'localhost';

FLUSH PRIVILEGES;

SHOW GRANTS FOR 'appuser'@'localhost';

EXIT;
# Security guidelines:
1. Use a unique, strong password and do not share it in team notes or screenshots.
2. Avoid putting the real password directly in shell commands or scripts.
3. Keep the database user restricted to localhost unless remote access is explicitly required.
4. FLUSH PRIVILEGES is included as requested, although MySQL normally applies CREATE USER and GRANT changes immediately.
# Verify database access
1. Log in as the dedicated user. The -p option prompts for the password securely.

mysql -u appuser -p appdb
Inside the MySQL shell, run:

sql
SELECT DATABASE();
SHOW TABLES;
Expected result:

SELECT DATABASE() returns appdb.

SHOW TABLES runs successfully, even if the database is empty.

Exit MySQL:

sql
EXIT;
# Verify all components
1. Run these commands on the VM:
2. sudo systemctl is-active apache2
3. sudo systemctl is-active mysql
4. 5. php -m | grep -i mysqli
6. curl -I http://localhost
# Expected outcomes:
Component
Expected result
VM
Running and accessible over SSH
Apache
active
MySQL
active
PHP
Version displayed and MySQL extension available
HTTP
Successful response, usually 200 OK
Database login
Successful authentication as appuser
Database access
appdb selected successfully
# Install the PHP extensions WordPress needs
sudo apt update
sudo apt install php-mysql php-curl php-gd php-imagick php-mbstring php-xml php-zip php-intl
sudo systemctl restart apache2
# Verify PHP and the MySQL extension:
php -v
php -m | grep -Ei 'mysqli|curl|gd|imagick|mbstring|xml|zip|intl'
# Create the WordPress database
sudo mysql
CREATE DATABASE wordpress
  DEFAULT CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;

CREATE USER 'wpuser'@'localhost'
  IDENTIFIED BY 'REPLACE_WITH_A_STRONG_PASSWORD';

GRANT ALL PRIVILEGES ON wordpress.*
  TO 'wpuser'@'localhost';

EXIT;
# Download the latest stable WordPress release
1. First, identify the latest stable release on the official WordPress GitHub tags page . Avoid alpha, beta, and release-candidate tags.
2. Then download the corresponding stable release archive from the official WordPress release archive .
3. Deploy the extracted files into /var/www/html only after checking whether that directory contains anything you need to preserve.
# Configure WordPress
1. WordPress needs database connection settings in wp-config.php:
2. Database name: wordpress
3. Database user: wpuser
4. Database password: the private password you created
5. Database host: localhost
Use the official WordPress configuration documentation  to configure the file safely.
Then verify that Apache has permission to read the WordPress files and write only where needed. Avoid making the entire installation world-writable.
# Complete the installation
Open this address in your browser, substituting your VM's IP address:
http://YOUR_VM_IP/
Follow the WordPress setup wizard to create the administrator account with a separate strong password.
# Verify the result
Check the service and configuration:
sudo systemctl status apache2 --no-pager
sudo systemctl status mysql --no-pager
sudo apache2ctl configtest
You should see Apache report Syntax OK, and both services should be running.
# END








# END
