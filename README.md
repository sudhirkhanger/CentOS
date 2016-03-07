# CentOS Server Guide

* [Initial Server Setup](#initial-server-setup)
* [Additional Recommended Steps](#additional-recommended-steps)
* [EPEL and IUS Repository Setup](#epel-and-ius-repository-setup)
* [LAMP](#lamp)
* VirtualHost
* Let's Encrypt
* WordPress

## Initial Server Setup

### Update system

    yum update
    
### Setup root password

    passwd
    
### Add a new user

    adduser admin
    passwd admin
    gpasswd -a admin wheel

### Copy ssh key to the new users account

    ssh-copy-id -i ~/.ssh/ssh-public-key.pub admin@x.x.x.x
    
### SSH config - disable root login and setup port

    cp /etc/ssh/sshd_config /etc/ssh/sshd_config.orig
    PermitRootLogin no
    Port xxxxx #(0 and 65535)
    systemctl reload sshd

[Source](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos-7)

## Additional Recommended Steps           

### Setup NTP

    sudo yum install ntp
    sudo systemctl start ntpd
    sudo systemctl enable ntpd
    
### Setup Swap

    sudo cp /etc/fstab /etc/fstab.orig
    sudo fallocate -l 1G /swapfile
    sudo chmod 600 /swapfile
    sudo mkswap /swapfile
    sudo swapon /swapfile
    sudo sh -c 'echo "/swapfile none swap sw 0 0" >> /etc/fstab'

[Source](https://www.digitalocean.com/community/tutorials/additional-recommended-steps-for-new-centos-7-servers)

# EPEL and IUS Repository Setup
    
    sudo yum install epel-release
    sudo yum install https://dl.iuscommunity.org/pub/ius/stable/CentOS/7/x86_64/ius-release-1.0-14.ius.centos7.noarch.rpm

[EPEL](https://fedoraproject.org/wiki/EPEL)
[IUS Repo](https://ius.io/GettingStarted/)
    
## LAMP

### Apache

	sudo yum install httpd
	sudo systemctl start httpd.service
	sudo systemctl enable httpd.service    
    
### Mariadb

	sudo yum shell
	> remove mariadb-libs
	> install mariadb100u-libs
	> run
	> quit

	sudo yum install mariadb100u-server mariadb100u
	sudo systemctl start mariadb
	sudo mysql_secure_installation
	sudo systemctl enable mariadb

### PHP 5.6

	sudo yum install php56u php56u-mysqlnd php56u-gd
	sudo systemctl restart httpd.service

[Source](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-centos-7)
