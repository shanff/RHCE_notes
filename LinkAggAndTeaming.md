<!-- TOC -->

- [1. Configuring Ling Aggregation and Bridging](#1-configuring-ling-aggregation-and-bridging)
    - [1.1. Network Teaming](#11-network-teaming)
        - [1.1.1. Types](#111-types)
        - [1.1.2. Creating Teams](#112-creating-teams)
        - [1.1.3. Review team states](#113-review-team-states)
    - [1.2. Configure software bridges](#12-configure-software-bridges)
        - [1.2.1. Creating Bridges](#121-creating-bridges)

<!-- /TOC -->

# 1. Configuring Ling Aggregation and Bridging

## 1.1. Network Teaming

### 1.1.1. Types

> | Name | Description|
> |---|---|
> | broadcast| simple runner which transmits each packet from all ports
> | roundrobin| simple runner which transmits packets in RR fashion from each port
> | activebackup| failover runner, watches for link changes and selects active port for use
> | loadbalance| monitors traffic, hash function to reach perfect balance
> | lacp| implements 802.3ad LACP. Same port selection as loadbalance

### 1.1.2. Creating Teams

1. Create the team interface

``` shell
[root@server ~]# nmcli con add type team con-name team0 ifname team0 config '{"runner": {"name": "loadbalance"}}'
```

2. Determine the IPv4/6 attributes of the team interface

``` shell
[root@server ~]# nmcli con mod team0 ipv4.addresses 1.2.3.4/24
[root@server ~]# nmcli con mod team0 ipv4.method manual
```

3. Assign the port interfaces

``` shell
[root@server ~]# nmcli con add type team-slave ifname eth1 master team0
or
[root@server ~]# nmcli con add type team-slave ifname eth2 master team0 con-name team0-eth2
```

4. Bring the team and port interfaces up/down.

``` shell
[root@server ~]# nmcli con up team0
```

### 1.1.3. Review team states

``` shell
[root@server ~]# teamdctl team0 state
```

## 1.2. Configure software bridges

### 1.2.1. Creating Bridges

1. Create the software bridge

``` shell
[root@server ~]# nmcli con add type bridge con-name br1 ifname br1
```

2. Configure the IPv4/6 attributes of the bridge interface

``` shell
[root@server ~]# nmcli con mod br1 ipv4.addresses 1.2.3.4/24
[root@server ~]# nmcli con mod br1 ipv4.method manual
```

3. Assign the port interface

``` shell
[root@server ~]# nmcli con add type brudge-slave con-name br1-port0 ifname eno1 master br1
```