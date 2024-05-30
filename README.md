### Nagios Installation Guide for EC2 Linux Instances

 step-by-step instructions for installing Nagios on EC2 Linux instances. Nagios is a powerful monitoring system that helps in keeping track of the status of your network, servers, and services.

#### Prerequisites
1. Launch EC2 instances using a Linux AMI.
2. Create a unique key pair (.ppk).
3. Configure security groups to allow SSH and HTTP access from anywhere.

#### Step 1: System Update and Package Installation
```bash
sudo yum update -y
sudo yum groupinstall "Development Tools"
sudo yum install httpd php gcc glibc glibc-common gd gd-devel wget unzip perl make net-snmp net-snmp-utils net-snmp-libs net-snmp-devel libjpeg-devel libpng-devel
```

#### Step 2: User and Group Setup for Nagios
```bash
sudo adduser -m nagios
sudo passwd nagios
sudo groupadd nagioscmd
sudo usermod -a -G nagioscmd nagios
sudo usermod -a -G nagioscmd apache
```

#### Step 3: Directory Creation and Download Nagios Software
```bash
mkdir ~/downloads
cd ~/downloads
```

#### Step 4: Download the Latest Nagios Core and Plugins
```bash
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.6.tar.gz
wget https://nagios-plugins.org/download/nagios-plugins-2.3.3.tar.gz
```

#### Step 5: Extract and Install Nagios Core
```bash
tar zxvf nagios-4.4.6.tar.gz
cd nagios-4.4.6
```

#### Step 6: Configure and Compile Nagios Core
```bash
./configure --with-command-group=nagioscmd
make all
sudo make install
sudo make install-init
sudo make install-config
sudo make install-commandmode
sudo make install-webconf
```

#### Step 7: Set Up Nagios Web Interface
```bash
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
sudo systemctl restart httpd
```

#### Step 8: Extract and Install Nagios Plugins
```bash
cd ~/downloads
tar zxvf nagios-plugins-2.3.3.tar.gz
cd nagios-plugins-2.3.3
./configure --with-nagios-user=nagios --with-nagios-group=nagios
make
sudo make install
```

#### Step 9: Enable and Start Nagios Service
```bash
sudo chkconfig --add nagios
sudo chkconfig nagios on
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
sudo systemctl start nagios
sudo systemctl restart httpd
```
#### Accessing Nagios Web Interface
Retrieve the public IP address of the instance and enter it into any web browser followed by "/nagios". Provide the username and password to access the Nagios web page.





