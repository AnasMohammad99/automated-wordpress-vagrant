# WordPress Vagrant Environment

## Overview
This Vagrant environment is designed to automate the setup of a WordPress development environment using Ubuntu as the base operating system.

## Prerequisites
- Vagrant
- VirtualBox (or another Vagrant-compatible provider)

## Vagrantfile
The `Vagrantfile` provided in this project sets up an Ubuntu virtual machine and provisions it with all the necessary software to run WordPress. It includes the installation of Apache, MySQL, PHP, and other required PHP extensions.

## Shell Script Explanation with Bash Commands
The shell script within the `Vagrantfile` performs the following tasks:

1. **System Update**
   ```bash
   sudo apt update  # Updates the package lists for upgrades and new package installations.
  ```
2. **Package Installation**
  ```
    sudo apt install apache2 ghostscript libapache2-mod-php mysql-server php php-bcmath php-curl php-imagick php-intl php-json php-mbstring php-mysql php-xml php-zip -y  # Installs Apache, MySQL, PHP, and necessary PHP extensions.
  ```
3. **Create Web Root Directory**
  ```
  sudo mkdir -p /srv/www  # Creates a directory to serve web files.
  sudo chown www-data: /srv/www  # Changes the ownership of the directory to the web server user.

  ```
  4. **Download and Extract WordPress**
  ```
  curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www  # Downloads and extracts WordPress to the web root directory.

  ```
  5. **Apache Virtual Host Configuration**
  ```
  cat > /etc/apache2/sites-available/wordpress.conf <<EOF  # Creates a new Apache virtual host configuration file for WordPress.
    <VirtualHost *:80>
        DocumentRoot /srv/www/wordpress
        <Directory /srv/www/wordpress>
            Options FollowSymLinks
            AllowOverride Limit Options FileInfo
            DirectoryIndex index.php
            Require all granted
        </Directory>
        <Directory /srv/www/wordpress/wp-content>
            Options FollowSymLinks
            Require all granted
        </Directory>
    </VirtualHost>
    EOF

  ```
  6. **Enable Site and Modules**
  ```
  sudo a2ensite wordpress  # Enables the new site configuration.
  sudo a2enmod rewrite  # Enables the Apache rewrite module.
  sudo a2dissite 000-default  # Disables the default site configuration.

  ```
  7. **MySQL Database and User Configuration**
  ```
  mysql -u root -e 'CREATE DATABASE wordpress;'  # Creates a new MySQL database for WordPress.
  mysql -u root -e 'CREATE USER wordpress@localhost IDENTIFIED BY "admin123";'  # Creates a new MySQL user with a password.
  mysql -u root -e 'GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER ON wordpress.* TO wordpress@localhost;'  # Grants the new user permissions on the WordPress database.
  mysql -u root -e 'FLUSH PRIVILEGES;'  # Applies the new permissions.

  ```
  8. **WordPress Configuration File Setup**
  ```
  sudo -u www-data cp /srv/www/wordpress/wp-config-sample.php /srv/www/wordpress/wp-config.php  # Copies the WordPress sample configuration file.
  sudo -u www-data sed -i 's/database_name_here/wordpress/' /srv/www/wordpress/wp-config.php  # Sets the database name in the configuration file.
  sudo -u www-data sed -i 's/username_here/wordpress/' /srv/www/wordpress/wp-config.php  # Sets the database user in the configuration file.
  sudo -u www-data sed -i 's/password_here/admin123/' /srv/www/wordpress/wp-config.php  # Sets the database password in the configuration file.

  ```
  9. **Restart Services**
  ```
  systemctl restart mysql  # Restarts the MySQL service.
  systemctl restart apache2  # Restarts the Apache service.
  ```