<!-- TOC -->

- [1. NTP](#1-ntp)
    - [1.1. Synchronize Time Using Other NTP Peers - Set Up Local Time Server](#11-synchronize-time-using-other-ntp-peers---set-up-local-time-server)
        - [1.1.1. Install NTP](#111-install-ntp)
        - [1.1.2. Enable and Start Services](#112-enable-and-start-services)
        - [1.1.3. Check Services](#113-check-services)
        - [1.1.4. Local Time Service Config](#114-local-time-service-config)
        - [1.1.5. Firewall Config for Local NTP Server](#115-firewall-config-for-local-ntp-server)
    - [1.2. Peer with New Time Server](#12-peer-with-new-time-server)
        - [1.2.1. Install and configure NTP](#121-install-and-configure-ntp)
        - [1.2.2. Edit the NTP Config File to Use New NTP server](#122-edit-the-ntp-config-file-to-use-new-ntp-server)
        - [1.2.3. Firewall Config for Local NTP Server](#123-firewall-config-for-local-ntp-server)

<!-- /TOC -->

# 1. NTP

## 1.1. Synchronize Time Using Other NTP Peers - Set Up Local Time Server

### 1.1.1. Install NTP

``` shell
# yum -y install NTP
```

>*Show servers*

``` shell
cat /etc/ntp.conf |grep server
```

### 1.1.2. Enable and Start Services

``` shell
# systemctl enable ntpd
# systemctl start ntpd
```

### 1.1.3. Check Services

``` shell
ntpq -p
ntpstat
```

### 1.1.4. Local Time Service Config

>*edit /etc/ntp.conf - Make note that 127.127.1.0 is special NTP IP address for local servers*

``` shell
# sed -i.bak 's/^server/#server/g' /etc/ntp.conf  #echo out old server entries
# echo "server 127.127.1.0" >> /etc/ntp.conf      #add local server entry
# systemctl restart ntpd
```

### 1.1.5. Firewall Config for Local NTP Server

``` shell
# firewall-cmd --permanent --add-service ntp
# firewall-cmd --reload
```

## 1.2. Peer with New Time Server

### 1.2.1. Install and configure NTP

``` shell
# yum -y install NTP
```

### 1.2.2. Edit the NTP Config File to Use New NTP server

>*Instead of using local NTP services, specify IP of your new NTP server created in previous step. Enable and Start NTP service persistently*

``` shell
# sed -i.bak 's/^server/#server/g' /etc/ntp.conf  #echo out old server entries
# echo "server 192.168.33.100" >> /etc/ntp.conf   #add NTP server entry
# systemctl enable ntpd
# systemctl restart ntpd
```

### 1.2.3. Firewall Config for Local NTP Server

``` shell
# firewall-cmd --permanent --add-service ntp
# firewall-cmd --reload
```