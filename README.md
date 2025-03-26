# QuickFixes
A Repo of nifty workarounds


## Keepalived

Ive an issue with purely running virtual ips provided by keepalived, so i needed to run the MASTER with the *real* ip while having the backups provide the mirrored ip as backup.

The quick and easy way to achive that is by doing the following

### MASTER Config

<pre>
    virtual_ipaddress {
       fd00::42:c1fe:fc28:a102 # Real ULA of MASTER
    }
    #virtual_ipaddress_excluded {
    #}
</pre>
### BACKUP Config
<pre>
    virtual_ipaddress {
    fd00::42:c1fe:fc28:a102
    }
    virtual_ipaddress_excluded {
    192.168.2.2 # Real IP of the MASTER
    }
</pre>

And thats it. When the Master is down, the Backup will deploy both ipv6 and ipv4 of the Master as virtual IP without complaining or throwing a fuss.
