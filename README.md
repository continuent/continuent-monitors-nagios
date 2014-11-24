continuent-monitors-nagios
==========================

A Ruby Gem containing Nagios checks for Continuent Tungsten and Tungsten Replicator

## Installation 

```gem install continuent-monitors-nagios```

## Example Nagios Service Check

```
check_by_ssh -H $HOSTADDRESS$ -t 30 -o="StrictHostKeyChecking=no" -C "sudo -u tungsten /usr/bin/tungsten_nagios_latency -w 60 -c 120 --directory=/opt/continuent/"
```

## Other useful projects

* https://github.com/Ericbla/check_jstat

## Compatibility
These checks only work on the continuent-tungsten-2.x series they are not compatible with continuent-tungsten-1.x.