# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "512"
    vb.cpus = 2 # not less than the required 2
  end

  config.vm.define "cacti-master-ubuntu" do |cacticluster|
      cacticluster.vm.box = "bento/ubuntu-18.04"
      cacticluster.vm.hostname = "cacti-master-ubuntu"
      cacticluster.vm.network "private_network", ip: "192.168.50.1"
      cacticluster.vm.network "forwarded_port", guest: 80, host: 8081
      cacticluster.vm.provider "virtualbox" do |vb|
          vb.name = "cacti-master-ubuntu"
          vb.memory = "1024"
      end
      cacticluster.vm.provision "shell", inline: <<-SHELL
            hostnamectl status
            apt-get update -qq
            # apt-get -yqq install php php-mysql php-curl php-net-socket \
            # php-gd php-intl php-pear php-imap php-memcache libapache2-mod-php \
            # php-pspell php-recode php-tidy php-xmlrpc php-snmp \
            # php-mbstring php-gettext php-gmp php-json php-xml php-common
            # php -v
            # sudo apt-get -yqq install apache2
            #https://www.itzgeek.com/how-tos/linux/ubuntu-how-tos/how-to-install-cacti-on-ubuntu-18-04-lts-bionic-beaver.html
            #Install Apache & MariaDB
            sudo apt-get -yqq install apache2 mariadb-server mariadb-client php-mysql libapache2-mod-php
            #Install PHP Extensions
            sudo apt-get -yqq install php-xml php-ldap php-mbstring php-gd php-gmp
            #Install SNMP
            sudo apt-get -yqq install snmp php-snmp rrdtool librrds-perl
            wget --no-check-certificate https://www.cacti.net/downloads/cacti-latest.tar.gz
            tar -zxvf cacti-latest.tar.gz
            mv cacti-1* /opt/cacti
            # https://www.itzgeek.com/how-tos/linux/how-to-monitor-remote-linux-servers-with-cacti.html
            # Configure the firewall to allow Cacti to fetch the data from client machines
            ufw enable
            ufw allow 161/udp
            ufw reload
            ufw status
            # https://www.itzgeek.com/how-tos/linux/how-to-monitor-remote-linux-servers-with-cacti.html
      SHELL
    end

    # config.vm.define "node01" do |cacticluster|
    #     cacticluster.vm.box = "bento/ubuntu-20.04"
    #     cacticluster.vm.hostname = "node01"
    #     cacticluster.vm.network "private_network", ip: "192.168.50.12"
    #     cacticluster.vm.network "forwarded_port", guest: 80, host: 80
    #     cacticluster.vm.network "forwarded_port", guest: 443, host: 443
    #     cacticluster.vm.network "forwarded_port", guest: 5666, host: 5666
    #     cacticluster.vm.provider "virtualbox" do |vb|
    #         vb.name = "node01"
    #         vb.memory = "1024"
    #     end
    #     cacticluster.vm.provision :shell, path: "provisioning/bootstrap.sh"
    #     cacticluster.vm.provision :shell, path: "scripts/deploy-nagios.sh"
    #     # cacticluster.vm.provision "shell", inline: <<-SHELL
    #     #       hostnamectl status
    #     #       apt-get update -qq
    #     #       # https://www.itzgeek.com/how-tos/linux/how-to-monitor-remote-linux-servers-with-cacti.html
    #     #       apt-get -yqq install snmpd
    #     # SHELL
    #   end


      # config.vm.define "node02" do |cacticluster|
      #     cacticluster.vm.box = "bento/centos-8.3"
      #     cacticluster.vm.hostname = "node02"
      #     cacticluster.vm.network "private_network", ip: "192.168.50.13"
      #     cacticluster.vm.provider "virtualbox" do |vb|
      #         vb.name = "node02"
      #         vb.memory = "1024"
      #     end
      #     # cacticluster.vm.provision "shell", inline: <<-SHELL
      #     #       hostnamectl status
      #     #       dnf update -y #update your system packages to the latest version
      #     #       # https://www.itzgeek.com/how-tos/linux/how-to-monitor-remote-linux-servers-with-cacti.html
      #     #       yum -y install net-snmp net-snmp-utils
      #     # SHELL
      #     cacticluster.vm.provision "shell", inline: <<-SHELL
      #     hostnamectl status
      #     dnf update -y #update your system packages to the latest version
      #     # https://www.server-world.info/en/note?os=CentOS_8&p=cacti&f=1
      #     # Install Apache httpd to configure Web Server
      #     # dnf -y install httpd
      #     # mv /etc/httpd/conf.d/welcome.conf.org /etc/httpd/conf.d/welcome.conf 
      #     # systemctl enable firewalld
      #     # systemctl start firewalld
      #     # systemctl status firewalld
      #     # firewall-cmd --add-service=http --permanent 
      #     # firewall-cmd --reload
      #     # https://www.server-world.info/en/note?os=CentOS_8&p=cacti&f=1
      #     # https://www.centlinux.com/2020/06/install-cacti-server-on-centos-8.html#point9
      #     dnf install -y httpd httpd-devel
      #     systemctl enable --now httpd.service
      #     systemctl enable firewalld
      #     systemctl start firewalld
      #     systemctl status firewalld
      #     firewall-cmd --permanent --add-service=http
      #     firewall-cmd --reload
      #     dnf install -y mariadb-server
      #     systemctl enable --now mariadb.service
      #     dnf install -y php-fpm php-common php-mysqlnd php-xml php-ldap php-json php-cli php-gd php-gmp php-mbstring php-process
      #     systemctl enable --now php-fpm.service
      #     dnf install -y net-snmp php-snmp net-snmp-utils
      #     dnf install -y rrdtool
      #     wget --no-check-certificate https://www.cacti.net/downloads/cacti-latest.tar.gz
      #     # tar -C cacti-latest.tar.gz
      #     # mv /var/www/html/cacti-1* /var/www/html/cacti
      #     # https://www.centlinux.com/2020/06/install-cacti-server-on-centos-8.html#point9
      #     # dnf install net-snmp net-snmp-utils net-snmp-libs rrdtool -y #dependencies required for Cacti
      #     # systemctl status snmpd
      #     # systemctl start snmpd #start the SNMP service and enable it to start at boot
      #     # systemctl enable snmpd
      #     # systemctl status snmpd
      #     # #################Install LAMP Server#################
      #     # #Install the Apache web server, MariaDB database server, PHP and other necessary PHP extensions
      #     # dnf install httpd mariadb-server php php-xml php-session php-sockets php-ldap php-gd php-json php-mysqlnd php-gmp php-mbstring php-posix php-snmp php-intl -y
      #     # #start the HTTP and MariaDB service and enable them to start at boot
      #     # systemctl status httpd
      #     # systemctl start httpd
      #     # systemctl enable httpd
      #     # systemctl status httpd
      #     # systemctl status mariadb
      #     # systemctl start mariadb
      #     # systemctl enable mariadb
      #     # systemctl status mariadb
      #     # dnf install epel-release -y
      #     # dnf install cacti -y
      #     # systemctl enable firewalld
      #     # systemctl start firewalld
      #     # systemctl status firewalld            
      #     SHELL          
      #   end


end