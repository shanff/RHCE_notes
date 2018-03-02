<!-- TOC -->

- [1. SSH](#1-ssh)
    - [1.1. Configure Key-Based Authentication](#11-configure-key-based-authentication)
        - [1.1.1. Create Key and Copy it to Remote Server](#111-create-key-and-copy-it-to-remote-server)
    - [1.2. Configure Additional Options](#12-configure-additional-options)
        - [1.2.1. Install SSH](#121-install-ssh)
        - [1.2.2. Config Files](#122-config-files)

<!-- /TOC -->

# 1. SSH

## 1.1. Configure Key-Based Authentication

### 1.1.1. Create Key and Copy it to Remote Server

>*Requires OpenSSL or openssl-server to be installed*

``` shell
ssh-keygen
#save default or create new file
#set passphrase if desired
```

``` shell
less ~/.ssh/id_rsa.pub #review public key
```

``` shell
ssh-copy-id user@<server ip>
#if password auth enabled, it will prompt for password for initial connect
```

``` shell
less ~/.ssh/authorized_keys
#will list keys of clients that you allow connecting to this account
```

## 1.2. Configure Additional Options

### 1.2.1. Install SSH

``` shell
# yum -y install openssh-server openssl
# systemctl enable sshd
# systemctl start sshd
# firewall-cmd --permanent --add-service ssh
# firewall-cmd --reload
```

### 1.2.2. Config Files

``` shell
less /etc/ssh/sshd_config'
man sshd
```

