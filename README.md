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

# END
