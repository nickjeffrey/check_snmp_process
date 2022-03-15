# check_snmp_process
nagios check for running processes via SNMP

# Requirements

You will need to enable SNMP on the monitored device. Windows / Linux / Cisco / etc are supported. Other devices using same SNMP OID values work as well.

# Usage

Add the following section to services.cfg on the nagios server
```
# Check uptime via SNMP
# syntax is check_snmp_uptime!optional_snmp_community!optional_minutes_uptime_warn_threshold
define service{
        use                             generic-24x7-service
        hostgroup_name                  all_windows,all_linux,all_cisco
        service_description             uptime
        check_command                   check_snmp_uptime!optional_snmp_community!optional_minutes_uptime_warn_threshold
        }
```
Add the following section to commands.cfg on the nagios server
```
# ---------------------------------------------------------------------------
# 'check_snmp_uptime' command definition
define command{
        command_name    check_snmp_uptime
        command_line    $USER1$/check_snmp_uptime -H $HOSTADDRESS$ -C $ARG1$ -w $ARG2$
        }
```
Copy the check_snmp_process file to /usr/local/nagios/libexec/ (or wherever your distro keeps nagios checks)
