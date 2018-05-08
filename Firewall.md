<!-- TOC -->

- [1. Network Port Security](#1-network-port-security)
    - [1.1. Firewalld overview](#11-firewalld-overview)
    - [1.2. Managing firewalld](#12-managing-firewalld)
        - [1.2.1. References](#121-references)
    - [1.3. Managing Rich Rules](#13-managing-rich-rules)
        - [1.3.1. Rich Rules Concepts](#131-rich-rules-concepts)

<!-- /TOC -->

# 1. Network Port Security

## 1.1. Firewalld overview

> firewalld is the default management tool for host-level firewalls in RHEL7.  iptables, ip6tables and ebtables conflict. Prior
> to using/enabling firewalld, mask those services with the following.

``` shell
user@server ~]# systemctl mask $service
```

**firewalld** separates all incoming traffic into zones.  Each zone has it's own set of rules.  **firewalld** uses logic rules to determine how rules are applied.

1. If the *source address* of an incoming packet matches a source rule setup for a zone, the packet will be routed through that zone.
2. if the *incoming interface* for a packet matches a filter setup for a zone, that zone will be used
3. Otherwise, the *default zone* will be used.  The default zone is not a separate zone; instead, it is a pointer to a defined zone.

> The standard default zone is the **public** zone

|Zone Name| Default Configuration|
|---|---|
|trusted| Allow all incoming traffic|
|home| Reject incoming traffic except for related, ssh, mdns, ipp-, samba-, dhcpv6|
|internal| *Same as **home** by default.
|work| Reject incoming unless related or matching ssh, ipp-clicnt, dhcpv6-client
|public| Reject unless related or matching ssh or dhcpv6-client
|external| Reject unless related, matching ssh.  Ougtoing is masqueraded against ipv4 address of outgoing network interface
|dmz| Reject incoming unless related or matches ssh
|block| Reject all incoming unless related
|drop| Drop all incoming except for related

## 1.2. Managing firewalld

1. using command line tool firewall-cmd
2. graphical tool firewall-config
3. configuration files /etc/firewalld/
    - /usr/lib/firewalld (original files - standard)
    - /etc/firewalld (override config if 2 files with same name exist)

### 1.2.1. References

> **firewall-cmd**(1), **firewall-config**(1), **firewalld**(1), **firewalld.zone**(5), **firewalld.zones**(5) man pages

## 1.3. Managing Rich Rules

### 1.3.1. Rich Rules Concepts

Direct Rules

- not covered in the course.

Rich Rules