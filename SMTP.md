<!-- TOC -->

- [1. SMTP](#1-smtp)
    - [1.1. Configure System to Forward All Email to Central Email Server - Setup](#11-configure-system-to-forward-all-email-to-central-email-server---setup)
        - [1.1.1. Install Postfix](#111-install-postfix)
        - [1.1.2. Configure Postfix](#112-configure-postfix)
        - [1.1.3. Create sasl_passwd](#113-create-saslpasswd)
        - [1.1.4. Enable and Start Postfix](#114-enable-and-start-postfix)
        - [Open Firewall ports for smtp](#open-firewall-ports-for-smtp)
    - [1.2. Configure System to Forward All Email to Central Email Server - Client Testing](#12-configure-system-to-forward-all-email-to-central-email-server---client-testing)
        - [Install mail client - mailx](#install-mail-client---mailx)

<!-- /TOC -->

# 1. SMTP

## 1.1. Configure System to Forward All Email to Central Email Server - Setup

### 1.1.1. Install Postfix

``` shell
# yum -y install postfix
```

### 1.1.2. Configure Postfix

>*/etc/postfix/main.cf - Configuration file for Postfix*
>*add the following lines for relayhost - Gmail specific settings included*

``` shell
relayhost = [smtp.gmail.com]:587

## Gmail Specific Settings
smtp_use_tls = yes
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_password
smtp_tls_CAfile = /etc/ssl/certs/ca-bundle.crt
smtp_sasl_security_options = noanonymous
smtp_sasl_tls_security_options = noanonymous
## End Gmail Specific Settings

inet_interfaces = loopback-only

mynetworks = 127.0.0.0/8 [::1]/128

myorigin = $myhostname

mydestination =

local_transport = error: local delivery disabled
```

### 1.1.3. Create sasl_passwd

>*create /etc/postfix/sasl_passwd*

``` shell
[smtp.gmail.com]:587 user@gmail.com:app_password
```

>*make sasl_password ready*

``` shell
# postmap /etc/postfix/sasl_passwd
# chown root:postfix /etc/postfix/sasl_passwd*
# chmod 640 /etc/postfix/sasl_passwd*
```

### 1.1.4. Enable and Start Postfix

``` shell
# systemctl enable postfix
# systemctl start postfix
```

### Open Firewall ports for smtp

``` shell
# firewall-cmd --permanent --add-service smtp
# firewall-cmd --reload
```

## 1.2. Configure System to Forward All Email to Central Email Server - Client Testing

### Install mail client - mailx

``` shell
# yum -y install mailx
```