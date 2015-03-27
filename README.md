continuent-monitors-nagios
==========================

A Ruby Gem containing Nagios checks for Continuent Tungsten and Tungsten Replicator

# Installation 

* Install the Ruby gem
 
 ```sudo gem install continuent-monitors-nagios```
* Install the Nagios NRPE service
* Add the IP of your Nagios server to the ```/etc/nagios/nrpe.cfg``` configuration file. For example:

 ```allowed_hosts=127.0.0.1,192.168.2.20```
* Add the Tungsten check commands that you want to execute to the ```/etc/nagios/nrpe.cfg``` configuration file. For example:

 ```command[tungsten_nagios_online]=/usr/bin/tungsten_nagios_online```

 If the commands need to be executed with superuser privileges, the ```/etc/sudo``` or ```/etc/sudoers``` file must be updated to enable the commands to be executed as root through sudo as the nagios user. This can be achieved by updating the configuration file, usually performed by using the visudo command:
 
 ```nagios          ALL=(tungsten)  NOPASSWD: /usr/bin/tungsten_nagios_*```
 
 In addition, the sudo command should be added to the Tungsten check commands within the Nagios ```/etc/nagios/nrpe.cfg```, for example:
 
 ```command[tungsten_nagios_online]=/usr/bin/sudo -u tungsten /usr/bin/tungsten_nagios_online```
* Start the NRPE service:

 ```shell> sudo /etc/init.d/nagios-nrpe-server start```
*  Add an entry to your Nagois ```services.cfg``` file for each service you want to monitor:
 
 ```define service {
         host_name database
         service_description     check_tungsten_online
         check_command           check_nrpe! -H $HOSTADDRESS$  -t 30 -c check_tungsten_online
         retry_check_interval    1
         check_period            24x7
         max_check_attempts      3
         flap_detection_enabled  1
         notifications_enabled   1
         notification_period     24x7
         notification_interval   60
         notification_options    c,f,r,u,w
         normal_check_interval   5
 }```

# Global Options

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

# Global Outputs

Each of these will usually be appended with a text message depending on the individual script being run.

* OK:  
* WARNING:
* CRITICAL:   
* UNKNOWN:

# Available Checks

Use the ```--help``` option for each command to see the full list of available options.

## tungsten_nagios_backups
Compare the age of the last backup with that given in the option max-backup-age.  

Returns OK with message 'Not running check because this node is not the coordinator' if the host on which this is being run is not the current cluster coordinator.  
Returns OK if a backup is found to have the same or lesser age and CRITICAL otherwise.
Returns an error message under the following circumstances:  
Script is run on a host that does not have a running Manager or a running Replicator.  
A backup is not found in the configured loacation.  

Available options:

--service String  
Where String is the replication service or cluster to check

--max-backup-age String  
Where String is the maximum allowed age in seconds of a backup on any machine. The default value is 86400.

## tungsten_nagios_connector

Check the availability of the Connector.

Returns one of:  
OK: The connection was successfully created  
CRITICAL: The server is not a Continuent Tungsten Connector  
CRITICAL: The Continuent Tungsten Connector is not running  
CRITICAL: A connection to the Tungsten Connector could not be created  

Available options:  
--defaults-file String  
The defaults file to use when connecting to MySQL

--statement String  
The command to run against the Tungsten Connector. The default command is "tungsten connection status".

## tungsten_nagios_latency

Check that all applied latency values are under a given number of seconds.

## tungsten_nagios_monitor_threads

Check that the number of Java threads for one of the VMware Continuent services is under a given value.

## tungsten_nagios_online

Check that all replication services and datasources are ONLINE.

## tungsten_nagios_policy

Check that the cluster policy is set to AUTOMATIC.

## tungsten_nagios_progress

Check that the VMware Continuent Replicator process is successfully applies a heartbeat event created by the script.

## tungsten_nagios_relative_latency

Check that all relative latency values are under a given number of seconds.

## tungsten_nagios_services

Check that all configured VMware Continuent services are running.

## Other useful projects

* https://github.com/Ericbla/check_jstat

## Compatibility
These checks only work on VMware Continuent 2.0 and later.
