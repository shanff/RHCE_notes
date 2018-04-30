<!-- TOC -->

- [1. Systemctl](#1-systemctl)
    - [1.1. Common Unit Types](#11-common-unit-types)
        - [1.1.1. service units (.service)](#111-service-units-service)
        - [1.1.2. socket units (.socket)](#112-socket-units-socket)
        - [1.1.3. path units (.path)](#113-path-units-path)
    - [1.2. Service States](#12-service-states)
    - [1.3. Listing unit files](#13-listing-unit-files)
        - [1.3.1. Query the state of all units to verify a system startup](#131-query-the-state-of-all-units-to-verify-a-system-startup)
        - [1.3.2. Query the state of only service units](#132-query-the-state-of-only-service-units)
        - [1.3.3. Investigate any units which are in a failed or maintenance state.  Optionallly, add the -l option to show the full output](#133-investigate-any-units-which-are-in-a-failed-or-maintenance-state-optionallly--add-the--l-option-to-show-the-full-output)
        - [1.3.4. Easily show the active and enable states](#134-easily-show-the-active-and-enable-states)
        - [1.3.5. List the active state of loaded units, limit by type.  The --all option will add inactive units](#135-list-the-active-state-of-loaded-units--limit-by-type-the---all-option-will-add-inactive-units)
        - [1.3.6. View the enabled and disabled settings for all units.  Optionally, limit the type of unit](#136-view-the-enabled-and-disabled-settings-for-all-units-optionally--limit-the-type-of-unit)
        - [1.3.7. View only failed services](#137-view-only-failed-services)

<!-- /TOC -->

# 1. Systemctl

## 1.1. Common Unit Types

### 1.1.1. service units (.service)

> Represent system services.  Used to start frequently accessed services, such as web or sql services.

### 1.1.2. socket units (.socket)

> Represent interprocess communication sockets. Allows connection to be made, then passes control off to service started as socket is connected.  Typical for less frequently used services for on-demand use.

### 1.1.3. path units (.path)

> Used to delay activation of service until a specified file system change occurs, such as printing spool/system.

## 1.2. Service States

``` shell
root@foundation0 ~]# systemctl status sshd.service
sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled)
   Active: active (running) since Sun 2018-04-29 21:03:09 EDT; 43min ago
  Process: 734 ExecStartPre=/usr/sbin/sshd-keygen (code=exited, status=0/SUCCESS)
 Main PID: 748 (sshd)
   CGroup: /system.slice/sshd.service
           └─748 /usr/sbin/sshd -D
```

> | Keyword:   | Description   |
> |---|---|
> | loaded    | Unit Configuration file has been processed |
> | active (running)| Running with one or more continuing processes|
> | active (exited)| Successfully completed a one-time configuration|
> | active (waiting)| Running but waiting for an event|
> | inactive | Not running
> | enabled | Will be started at boot time
> | disabled | Will not be started at boot time
> | static | Cannot be enabled, but may be started by an enabled unit automatically

## 1.3. Listing unit files

### 1.3.1. Query the state of all units to verify a system startup

``` shell
root@foundation0 ~]# systemctl
```

### 1.3.2. Query the state of only service units

``` shell
root@foundation0 ~]# systemctl --type=service
```

### 1.3.3. Investigate any units which are in a failed or maintenance state.  Optionallly, add the -l option to show the full output

``` shell
root@foundation0 ~]# systemctl status rngd.service -l
```

### 1.3.4. Easily show the active and enable states

``` shell
root@foundation0 ~]# systemctl is-enabled sshd
root@foundation0 ~]# systemctl is-active sshd
```

### 1.3.5. List the active state of loaded units, limit by type.  The --all option will add inactive units

``` shell
root@foundation0 ~]# systemctl list-units --type=service
root@foundation0 ~]# systemctl list-units --type=service --all
```

### 1.3.6. View the enabled and disabled settings for all units.  Optionally, limit the type of unit

``` shell
root@foundation0 ~]# systemctl list-unit-files --type=service
```

### 1.3.7. View only failed services

``` shell
root@foundation0 ~]# systemctl --failed --type=service
```