# WordPress Vagrant Environment

## Overview
This Vagrant environment is designed to automate the setup of a WordPress development environment using Ubuntu as the base operating system.

## Prerequisites
- Vagrant
- VirtualBox (or another Vagrant-compatible provider)

## Vagrantfile
The `Vagrantfile` provided in this project sets up an Ubuntu virtual machine and provisions it with all the necessary software to run WordPress. It includes the installation of Apache, MySQL, PHP, and other required PHP extensions.

## Shell Script Explanation
The shell script within the `Vagrantfile` performs the following tasks:

1. **System Update and Package Installation**
   Updates the package lists and installs necessary packages including Apache, MySQL, PHP, and required PHP extensions.

2. **WordPress Download and Setup**
   Downloads the latest WordPress package and extracts it to `/srv/www`, setting the appropriate permissions.

3. **Apache Configuration**
   Configures a new Apache virtual host for WordPress and enables the necessary modules and sites.

4. **MySQL Database and User Setup**
   Creates a new MySQL database and user for WordPress and grants the necessary permissions.

5. **WordPress Configuration**
   Copies the WordPress sample configuration file and updates it with the database details.

6. **Service Restart**
   Restarts the MySQL and Apache services to apply the changes.

## Usage
To use this environment, simply run the following command in the directory containing the `Vagrantfile`:

```bash
vagrant up
