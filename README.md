continuent-monitors-nagios
==========================

A Ruby Gem containing Nagios checks for Continuent Tungsten and Tungsten Replicator

## Installation 

```gem install continuent-monitors-nagios```

## Example Nagios Service Check

```
check_by_ssh -H $HOSTADDRESS$ -t 30 -o="StrictHostKeyChecking=no" -C "sudo -u tungsten /usr/bin/tungsten_nagios_latency -w 60 -c 120 --directory=/opt/continuent/"
```

## Global Options

--directory  
Use this installed Tungsten directory as the base for all operations

--quiet, -q

--info, -i

--notice, -n

--verbose, -v

--help, -h  
Display this message

--json                      
Provide return code and logging messages as a JSON object after the script finishes

--net-ssh-option=key=value  
Set the Net::SSH option for remote system calls. Valid options can be found at http://net-ssh.github.com/ssh/v2/api/classes/Net/SSH.html#M000002

## Global Outputs

OK:  
CRITICAL:   
UNKNOWN:    
The above will usually be appended with a text message depending on the individual script being run.

## tungsten_nagios_backups
This script will compare the age of the last backup with that given in the option max-backup-age.  
Returns OK with message 'Not running check because this node is not the coordinator' if the host on which this is being run is not the current cluster coordinator.  
Returns OK if a backup is found to have the same or lesser age and CRITICAL otherwise.

The script returns an error message if run on a host that does not have a running Manager or a running Replicator.
If a backup is not found in the configured loacation an error will be returned.

Available options:

--service String  
Where String is the replication service or cluster to check

--max-backup-age String  
Where String is the maximum allowed age in seconds of a backup on any machine. The default value is 86400.


## Other useful projects

* https://github.com/Ericbla/check_jstat

## Compatibility
These checks only work on the continuent-tungsten-2.x series they are not compatible with continuent-tungsten-1.x.
