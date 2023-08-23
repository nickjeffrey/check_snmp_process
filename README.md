# check_snmp_process
nagios check for running processes via SNMP

# Requirements

You will need to enable SNMP on the monitored device. Windows / Linux / Cisco / etc are supported. Other devices using same SNMP OID values work as well.

# Usage

Add the following section to services.cfg on the nagios server.  Adjust process names as appropriate for your environment.
```

# Define a service to check for the vmtoolsd.exe process on a Windows box 
# We feed this check 4 parameters: SNMP_community_name,min_processes,max_processes,process_name
define service{
     use                             generic-service
     hostgroup_name                  all_windows_vmware_guests
     service_description             vmtoolsd.exe
     check_command                   check_snmp_process!public!1!2!vmtoolsd.exe
     }

# Define a service to check for the /usr/sbin/dhcpd process on a Linux box
# We feed this check 4 parameters: SNMP_community_name,min_processes,max_processes,process_name
define service{
     use                             generic-24x7-service
     host_name                       dhcp01.example.com
     service_description             dhcpd proces
     check_command                   check_snmp_process!public!1!2!/usr/sbin/dhcpd
     }
```

Add the following section to commands.cfg on the nagios server
```
# 'check_snmp_process' command definition
define command{
        command_name    check_snmp_process
        command_line    $USER1$/check_snmp_process -H $HOSTADDRESS$ -C $ARG1$ -m $ARG2$ -x $ARG3$ -p $ARG4$
        }
```

Copy the check_snmp_process file to /usr/local/nagios/libexec/ (or wherever your distro keeps nagios checks)
