# Avocent ACS800/8000 CLI Navigation Tree

Complete navigation structure for administrators.

## Root Level (/)

```
access/
active_sessions/
authentication/
change_password/
events_and_logs/
monitoring/
network/
pluggable_devices/
ports/
power_management/
sensors/
system/
system_tools/
users/
```

Regular users see only:
```
access/
change_password/
power_management/
```

## System (/system)

```
system/
├── security/
│   ├── security_profile
│   │   ├── idle_timeout=
│   │   ├── rpc=
│   │   ├── enable_pluggable_device_detection=
│   │   ├── enable_pluggable_storage_devices=
│   │   ├── port_access_kill_multi_session=
│   │   ├── port_access_send_message_multi_session=
│   │   ├── port_access_power_control=
│   │   ├── port_access_data_buffer_management=
│   │   ├── bootp_enabled=
│   │   ├── bootp_interface=
│   │   ├── ssh_allows_authentication_via_username|password=
│   │   ├── enable_telnet_service=
│   │   ├── enable_ftp_service=
│   │   ├── enable_snmp_service=
│   │   ├── enable_ipsec=
│   │   ├── answer_icmp_message=
│   │   ├── ssh_version=
│   │   ├── ssh_tcp_port=
│   │   ├── ssh_allow_root_access=
│   │   ├── ssh_minimum_cipher_and_mac_suite_level=
│   │   ├── enable_http_session=
│   │   ├── http_port=
│   │   ├── enable_https_session=
│   │   ├── https_tls_version=
│   │   ├── https_port=
│   │   ├── https_minimum_cipher_suite_level=
│   │   └── redirect_http|https=
│   ├── dsview/
│   │   └── all_appliance_to_be_managed_by_dsview=
│   └── fips_140/
│       └── enable_fips_140-2_module=
├── date_and_time/
│   ├── settings=
│   ├── day=
│   ├── hour=
│   ├── minute=
│   ├── month=
│   ├── second=
│   ├── year=
│   └── time_zone=
├── help_and_language/
│   ├── appliance_language=
│   └── url=
├── general/
│   ├── enable_login_banner=
│   ├── login_banner=
│   └── viewer_type=
├── boot_configuration/
│   ├── boot_mode=
│   ├── boot_image=
│   ├── watchdog_timer=
│   └── console_speed=
├── information/
└── usage/
    ├── memory
    └── flash_usage
```

## Network (/network)

```
network/
├── settings/
│   ├── hostname=
│   ├── primary_dns=
│   ├── secondary_dns=
│   ├── domain=
│   ├── search=
│   ├── enable_lldp=
│   ├── enable_ipv6=
│   ├── get_dns_from_dhcpv6=
│   ├── get_domain_from_dhcpv6=
│   ├── multiple_routing=
│   └── enable_bonding=
├── devices/
│   ├── eth0/
│   │   ├── set_as_primary_interface=
│   │   ├── status=
│   │   ├── ipv4_method=          # dhcp, static, ipv4_address_unconfigured
│   │   ├── ipv4_address=
│   │   ├── ipv4_mask=
│   │   ├── ipv6_method=          # stateless, dhcpv6, static, ipv6_address_unconfigured
│   │   ├── ipv6_address=
│   │   ├── ipv6_prefix_length=
│   │   └── mode=
│   └── eth1/
│       └── (same as eth0)
├── ipv4_static_routes/
│   └── default_gateway/
│       ├── gateway=
│       ├── interface=
│       └── metric=
├── ipv6_static_routes/
├── hosts/
│   └── <ip_address>/
│       ├── ip=
│       ├── hostname=
│       └── alias=
├── firewall/
│   ├── ipv4_filter_table/
│   │   ├── FORWARD/
│   │   ├── INPUT/
│   │   └── OUTPUT/
│   └── ipv6_filter_table/
│       ├── FORWARD/
│       ├── INPUT/
│       └── OUTPUT/
├── ipsec(vpn)/
└── snmp/
```

## Ports (/ports)

```
ports/
├── serial_ports/
│   └── <port_number>/
│       ├── physical/
│       │   ├── status=
│       │   ├── rj45_pin-out=
│       │   ├── speed=
│       │   ├── parity=
│       │   ├── data_bits=
│       │   ├── stop_bits=
│       │   └── flow_control=
│       ├── cas/
│       │   ├── port_name=
│       │   ├── enable_auto_discovery=
│       │   ├── enable_speed_auto_detection=
│       │   ├── protocol=
│       │   ├── authentication_type=
│       │   ├── text_session_hot_key=
│       │   ├── power_session_hot_key=
│       │   ├── restful_hot_key=
│       │   ├── telnet_port_alias=
│       │   ├── ssh_port_alias=
│       │   ├── raw_mode_port_alias=
│       │   ├── port_ipv4_alias=
│       │   ├── port_ipv6_alias=
│       │   ├── dcd_sensitivity=
│       │   ├── enable_auto_answer=
│       │   ├── dtr_mode=
│       │   ├── line_feed_suppression=
│       │   ├── transmission_interval=
│       │   ├── break_sequence=
│       │   ├── break_interval=
│       │   ├── show_multi-session_menu=
│       │   └── log_in|out_multi_session_notification=
│       ├── data_buffering/
│       │   ├── status=
│       │   ├── type=
│       │   ├── local_type=
│       │   ├── time_stamp=
│       │   ├── login|logout_message=
│       │   └── serial_session_logging=
│       ├── alerts/
│       └── power/
├── auxiliary_ports/
│   └── ttyM1/                    # Internal modem (if present)
│       ├── status=
│       ├── speed=
│       ├── init_chat=
│       ├── ppp_address=
│       ├── ppp_authentication=
│       ├── chap-interval=
│       ├── chap-max-challenge=
│       ├── chap-restart=
│       ├── ppp_idle_timeout=
│       └── cas_profile/
├── cas_profile/
│   ├── auto_discovery/
│   │   └── settings/
│   ├── auto_answer/
│   ├── pool_of_ports/
│   └── restful_settings/
└── dial-in_profile/
    ├── secure_dial-in/
    │   ├── callback_users/
    │   └── ppp_otp_users/
    └── settings/
```

## Authentication (/authentication)

```
authentication/
├── appliance_authentication/
│   ├── authentication_type=
│   ├── enable_fallback_to_local_type_for_root_user_in_appliance_console_port=
│   └── enable_single_sign-on=
└── authentication_servers/
    ├── radius/
    │   ├── first_authentication_server=
    │   ├── first_accounting_server=
    │   ├── second_authentication_server=
    │   ├── second_accounting_server=
    │   ├── secret=
    │   ├── timeout=
    │   ├── retries=
    │   └── enable_servicetype=
    ├── tacacs+/
    │   ├── first_authentication_server=
    │   ├── first_accounting_server=
    │   ├── second_authentication_server=
    │   ├── second_accounting_server=
    │   ├── service=
    │   ├── secret=
    │   ├── timeout=
    │   ├── retries=
    │   ├── tacacs+_version=
    │   └── enable_user-level=
    ├── ldap(s)|ad/
    │   ├── server=
    │   ├── base=
    │   ├── secure=
    │   ├── database_user_name=
    │   ├── database_password=
    │   └── login_attributes=
    ├── kerberos/
    │   ├── server=
    │   ├── realm_domain_name=
    │   └── domain_name=
    └── dsview/
        ├── ip_address_1=
        ├── ip_address_2=
        ├── ip_address_3=
        └── ip_address_4=
```

## Users (/users)

```
users/
├── authorization/
│   └── groups/
│       ├── admin/
│       │   ├── members/
│       │   ├── login_profile/
│       │   │   ├── session_timeout=
│       │   │   └── enable_log-in_profile=
│       │   └── access_rights/
│       │       ├── serial/
│       │       ├── power/
│       │       └── appliance/
│       ├── appliance-admin/
│       ├── shell-login-profile/
│       ├── user/
│       └── dsview_access_rights/
└── local_accounts/
    ├── user_names/
    │   └── <username>/
    │       ├── settings/
    │       │   ├── user_name=
    │       │   ├── password=
    │       │   ├── confirm_password=
    │       │   ├── password_change_at_next_login=
    │       │   ├── user_group=
    │       │   ├── password_minimum_days=
    │       │   ├── password_maximum_days=
    │       │   ├── password_inactive_days=
    │       │   ├── password_warning_days=
    │       │   └── account_expiration_date=
    │       └── access_rights/
    └── password_rules/
        ├── check_password_complexity=
        ├── min_digits=
        ├── min_upper_case_characters=
        ├── min_special_characters=
        ├── minimum_size=
        ├── def_expiration_min_days=
        ├── def_expiration_max_days=
        ├── def_expiration_warning_days=
        ├── number_of_permitted_failed_attempts_{0|disabled}=
        ├── account_lockout_duration_after_each_failed_login_{min}=
        └── unlock_account_after_{min}_{0|manual_unlock}=
```

## Power Management (/power_management)

```
power_management/
├── pdus/
│   └── <pdu_name>/
│       ├── outlets/
│       │   └── <outlet_number>/
│       │       ├── name=
│       │       ├── status=         # ON/OFF
│       │       └── action=
│       ├── sensors/
│       └── settings/
├── login/                          # PDU credentials
├── outlet_groups/
├── network_pdus/
├── ups/
└── network_ups/
```

## Events and Logs (/events_and_logs)

```
events_and_logs/
├── event_list/
├── event_destinations/
│   ├── syslog/
│   ├── snmp_trap/
│   ├── sms/
│   ├── email/
│   ├── dsview/
│   └── trap_forward/
├── data_buffering/
│   ├── local_data_buffering_settings/
│   │   ├── segment_size_(kbytes)=
│   │   └── spare_segments=
│   ├── nfs_data_buffering_settings/
│   │   ├── nfs_server=
│   │   ├── nfs_path=
│   │   ├── segment_size_(kbytes)=
│   │   └── spare_segments=
│   └── syslog_data_buffering_settings/
│       └── syslog_facility=
├── appliance_logging/
│   └── enable_session_logging=
└── sensors/
```

## Sensors (/sensors)

```
sensors/
├── appliance/
│   └── internal/
│       ├── current_cpu_temperature_(deg_c)
│       ├── maximum_cpu_temperature_(deg_c)=
│       ├── maximum_cpu_temperature_threshold_(deg_c)=
│       ├── minimum_cpu_temperature_(deg_c)=
│       ├── minimum_cpu_temperature_threshold_(deg_c)=
│       ├── current_board_temperature_(deg_c)
│       ├── maximum_board_temperature_(deg_c)=
│       ├── maximum_board_temperature_threshold_(deg_c)=
│       ├── minimum_board_temperature_(deg_c)=
│       ├── minimum_board_temperature_threshold_(deg_c)=
│       └── (voltage sensors...)
├── 1-wire/
│   └── <sensor>/
├── digital_in/
│   └── <sensor>/
└── pdu/
    └── <sensor>/
```

## Access (/access)

```
access/
├── <appliance_name>              # Console system info
└── <port_name>/                  # Configured ports
    ├── connect                   # Connect to port referenced by port_name
    ├── sniff                     # View-only connection
    ├── share                     # Shared read/write
    ├── list_shared_session
    ├── kill_shared_session
    ├── sendmsg
    ├── show_databuf
    └── clean_databuf
```

## Active Sessions (/active_sessions)

```
active_sessions/
└── <id>
    └── show
        ├── user
        ├── client_ip
        ├── creation_time
        ├── session_type
        ├── connection_type
        ├── target_name
        ├── id
        └── parent_id
```
Command: `kill_session <session_id>`
