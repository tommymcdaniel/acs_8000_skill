# Avocent ACS800/8000 CLI Examples

Natural language requests mapped to CLI commands.

## Device Access

### "Connect to serial port 2"
```
cd /access
connect 21-67-72-p-2
```

### "Show all connected devices"
```
cd /access
show
```
Output shows: name, port, type (serial/PDU), status (idle/in-use)

### "Monitor port 5 without being able to type"
```
cd /access/21-67-72-p-5
sniff
```
View-only connection to see port activity.

### "Share a session on port 3 with read/write"
```
cd /access/21-67-72-p-3
share
```

### "See who's connected to port 1"
```
cd /access/21-67-72-p-1
list_shared_session
```

### "Kick user admin@139 off port 1"
```
cd /access/21-67-72-p-1
kill_shared_session admin@139
```

### "Send message to user on port"
```
cd /access/<port_name>
sendmsg admin@139 Disconnecting in 5 minutes for maintenance
```

---

## Port Configuration

### "Enable serial port 1"
```
cd /ports/serial_ports/1
set status=enabled
commit
```

### "Configure port 2 for console access"
```
cd /
set_cas ports/serial_ports/ 2
set status=enabled
save
```

### "Set port 3 speed to 115200"
```
cd /ports/serial_ports/3
cd physical
set speed=115200
save
```

### "Copy configuration from port 5 to ports 10 and 15"
```
cd /ports/serial_ports
clone_ports 5
set copy_configuration_to=10,15
save
```

### "Configure port 4 for PDU connection"
```
cd /
set_power ports/serial_ports/ 4
set status=enabled
save
```

### "Disable ports 8 through 12"
```
cd /
disable_ports 8,9,10,11,12
```
Or:
```
cd /
disable_ports 8-12
```

### "Reset port 6 to factory defaults"
```
cd /
reset_port_to_factory 6
```

---

## Power Management (PDU)

### "Power cycle outlet 3 on PDU myPDU"
```
cd /access/myPDU
cycle 3
```

### "Turn off outlets 1 through 5"
```
cd /access/<pdu_name>
off 1-5
```

### "Turn on outlets 2, 4, and 6"
```
cd /access/<pdu_name>
on 2,4,6
```

### "Power cycle all outlets on PDU"
```
cd /access
cycle <pdu_name>
```

### "Lock outlet 4 (prevent power control)"
```
cd /power_management/pdus/<pdu_name>/outlets
lock 4
```

### "Rename PDU to 'ServerRack1'"
```
cd /power_management/pdus
rename <old_pdu_name>
set new_pdu_id=ServerRack1
save
```

---

## Network Configuration

### "Set static IP 192.168.1.100 on eth0"
```
cd /network/devices/eth0
set ipv4_method=static
set ipv4_address=192.168.1.100 ipv4_mask=255.255.255.0 ipv4_gateway=192.168.1.1
commit
```

### "Enable DHCP on eth0"
```
cd /network/devices/eth0
set ipv4_method=dhcp
commit
```

### "Change hostname to 'DataCenter-ACS'"
```
cd /network/settings
set hostname=DataCenter-ACS
commit
```

### "Set primary DNS to 8.8.8.8"
```
cd /network/settings
set primary_dns=8.8.8.8
commit
```

### "Enable IPv6"
```
cd /network/settings
set enable_ipv6=yes
commit
```
Note: Requires reboot to take effect.

### "Run network configuration wizard"
```
wiz
```
Interactive wizard for eth0 IP, DNS, hostname configuration.

### "Add host entry for server1 at 192.168.1.50"
```
cd /network/hosts
add
set hostname=server1 ip=192.168.1.50
save
```

### "Delete host entry 192.168.1.50"
```
cd /network/hosts
delete 192.168.1.50
commit

### "Show current network settings"
```
cd /network/settings
show
```

---

## User Management

### "Add user 'jsmith' with password 'SecurePass123'"
```
cd /users/local_accounts/user_names
add
set user_name=jsmith password=SecurePass123 confirm_password=SecurePass123
save
```

### "Delete user 'testuser'"
```
delete users/local_accounts/user_names/testuser
commit
```

### "Change my password"
```
cd /change_password
set old_password=oldpass new_password=newpass confirm_password=newpass
```

### "Set password to expire in 90 days"
```
cd /users/local_accounts/user_names/<username>/settings
set password_maximum_days=90
commit
```

### "Show all local users"
```
cd /users/local_accounts/user_names
show
```

### "Enable password complexity requirements"
```
cd /users/local_accounts/password_rules
set check_password_complexity=yes
set min_digits=1
set min_upper_case_characters=1
set min_special_characters=1
set minimum_size=8
commit
```

---

## Authentication

### "Configure RADIUS authentication"
```
cd /authentication/authentication_servers/radius
set first_authentication_server=192.168.1.10
set secret=radiussecret
set timeout=5
set retries=3
commit
```

### "Enable single sign-on"
```
cd /authentication/appliance_authentication
set enable_single_sign-on=yes
commit
```

### "Set authentication type to RADIUS with local fallback"
```
cd /authentication/appliance_authentication
set authentication_type=radius_down_local
commit
```

### "Configure TACACS+ server"
```
cd /authentication/authentication_servers/tacacs+
set first_authentication_server=192.168.1.20
set secret=tacacssecret
set timeout=10
commit
```

---

## System Administration

### "Show system information"
```
cd /system/information
show
```

### "Set timezone to EST"
```
cd /system/date_and_time
set time_zone=EST
commit
```

### "Enable login banner"
```
cd /system/general
set enable_login_banner=yes
set login_banner=Authorized access only. All activity is monitored.
commit
```

### "Enable SSH and disable Telnet"
```
cd /system/security
set enable_telnet_service=no
commit
```

### "View memory and flash usage"
```
cd /system/usage
show
```

### "View CPU temperature"
```
cd /sensors/appliance/internal
show
```

### "Reboot the console system"
```
cd /system_tools
reboot
```

### "Set console speed to 115200"
```
cd /system/boot_configuration
set console_speed=115200
commit
```

---

## Data Buffering

### "View data buffer for port 2"
```
cd /access/21-67-72-p-2
show_databuf
```

### "Clear data buffer for port 2"
```
cd /access/21-67-72-p-2
clean_databuf
```

### "Enable data buffering on port 3"
```
cd /ports/serial_ports/3
cd data_buffering
set status=enabled
save
```

### "View appliance session log"
```
show_appliance_databuf
```

---

## Session Management

### "Show all active sessions"
```
cd /active_sessions
show
```

### "Kill session 37"
```
cd /active_sessions
kill_session 37
```

---

## Backup and Restore

### "Export configuration"
```
list_configuration
```
Output can be pasted back to restore configuration.

### "Copy file via SCP"
```
scp admin@192.168.1.100:/backup/config.tar /tmp/config.tar
```

### "Connect to FTP server"
```
ftp 192.168.1.200
```

---

## Quick Diagnostic Commands

### "Where am I in the CLI?"
```
pwd
```

### "What can I do here?"
```
ls
```
Or press Tab twice.

### "What are the current settings?"
```
show
```

### "Undo my changes"
```
revert
```

### "Save my changes"
```
commit
```

---

## Troubleshooting

### "Check if changes are pending"
Look at prompt:
- `--:-` = No pending changes
- `**:-` = Unsaved changes exist

### "Exit without saving"
```
revert
exit
```

### "Recovery mode (won't boot from flash)"
1. Connect to console port at 9600 8N1
2. Power cycle and press any key at "Hit any key to stop autoboot"
3. Insert USB with firmware-ngacs.fl
4. At U-Boot: `usb_boot single`
5. At # prompt: `recover-flash.sh`
6. Type `yes` at warnings
