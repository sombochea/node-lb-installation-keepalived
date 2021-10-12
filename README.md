# Node LB Installation

```shell
sudo apt-get install keepalived
```

- Node1: Create or edit file: `/etc/keepalived/keepalived.conf`

```conf
global_defs {
   notification_email {
     sysadmin@cubetiqhost.net
     support@cubetiqhost.net
   }
   notification_email_from lb-node1@cubetiqhost.net
   smtp_server localhost
   smtp_connect_timeout 30
}

vrrp_instance VI_1 {
    state MASTER
    interface ens18
    virtual_router_id 101
    priority 101
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.0.60
    }
}
```

- Node1: Create or edit file: `/etc/keepalived/keepalived.conf`

```confg
global_defs {
   notification_email {
     sysadmin@cubetiqhost.net
     support@cubetiqhost.net
   }
   notification_email_from lb-node2@cubetiqhost.net
   smtp_server localhost
   smtp_connect_timeout 30
}

vrrp_instance VI_1 {
    state BACKUP
    interface ens18
    virtual_router_id 101
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass cubetiq007
    }
    virtual_ipaddress {
        192.168.0.60
    }
}
```

- Restart keepalived service

```shell
sudo service keepalived start
```

- Show virtual ip address on lb-node2
```shell
ip -a
```

- Notes
1. Priority value will be higher on Master server, It doesn’t matter what you used in state. If your state is MASTER but your priority is lower than the router with BACKUP, you will lose the MASTER state.
2. virtual_router_id should be same on both LB1 and LB2 servers.
3. By default single vrrp_instance support up to 20 virtual_ipaddress. In order to add more addresses you need to add more vrrp_instance

### Setup 3 masters keepalived
- master-1 `/etc/keepalived/keepalived.conf`
```config
global_defs {
   notification_email {
     sysadmin@cubetiqhost.net
     support@cubetiqhost.net
   }
   notification_email_from master-1@cubetiqhost.net
   smtp_server localhost
   smtp_connect_timeout 30
}

vrrp_instance VI_1 {
    state MASTER
    interface ens18
    virtual_router_id 101
    priority 101
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.0.60
    }
}
```

- master-2 (BACKUP) `/etc/keepalived/keepalived.conf`
```config
global_defs {
   notification_email {
     sysadmin@cubetiqhost.net
     support@cubetiqhost.net
   }
   notification_email_from master-2@cubetiqhost.net
   smtp_server localhost
   smtp_connect_timeout 30
}

vrrp_instance VI_1 {
    state BACKUP
    interface ens18
    virtual_router_id 101
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.0.60
    }
}
```

- master-3 (BACKUP) `/etc/keepalived/keepalived.conf`
```config
global_defs {
   notification_email {
     sysadmin@cubetiqhost.net
     support@cubetiqhost.net
   }
   notification_email_from master-3@cubetiqhost.net
   smtp_server localhost
   smtp_connect_timeout 30
}

vrrp_instance VI_1 {
    state BACKUP
    interface ens18
    virtual_router_id 101
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.0.60
    }
}
```