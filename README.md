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

## Disable Turbo in Software

Ive encountered an odd behavior when disabling the turbo on intel CPUs via
<pre>
    echo "1" | sudo tee /sys/devices/system/cpu/intel_pstate/no_turbo
</pre>

When you try to re-activate the turbo within like 5 minutes, it works as expected. When you keep it running for longer, for some reason, the maximum frequency will be stuck to non-turbo mode

To fix it, apply the maximum turbo boost via 

<pre>
    echo "0" | sudo tee /sys/devices/system/cpu/intel_pstate/no_turbo
    echo 4700000 | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_max_freq
</pre>

the 4700000 in this case, is the maximum boost frequency of my i3-14100, so you need to check what yours is.
