# ACS800/8000 detailed command and administration reference
Use this file for high-precision answers that should closely mirror the ACS800/8000 Command Reference Guide.
For full transcript coverage, open `acs8000-command-reference-transcript.md`.

## 1) Access, login, and navigation model
### Access methods
- Local console: terminal emulator on console port, default serial settings `9600 8N1`, no flow control.
- Remote: SSH/Telnet (if enabled in Security Profile), Web Manager Appliance Viewer, or DSView.
- Root users may land in Linux shell and then run `cli`.

### First-login default
- Default username is `admin`.
- First login as `admin` uses blank password, then prompts for new password.

### Prompt states
- Normal: `--:- ... cli->`
- Pending unsaved changes: `**:- ... cli->`
- Wizard mode prompt examples: `--:#- [path] cli->`

### Navigation conventions
- Tree paths with `/`.
- Web Manager labels map to CLI by replacing spaces with underscores.
- `cd /users/local_accounts/user_names` is equivalent to `Users - Local Accounts - User Names`.
- Use `cd ..`, `cd ../..`, and `cd /` to move up or root.
- Use `cd <space><tab><tab>` to discover available child nodes at current level.
- Commands are case-sensitive.

## 2) CLI command set (section 2.1)
### `add`
- Purpose: add a node.
- Syntax: `add <Path>`
- Example: `add network/hosts`
- After `add`, prompt enters wizard mode with `--:#-`.

### `cd`
- Purpose: change level/path.
- Syntax: `cd <Path>`
- Examples:
  - `cd access`
  - `cd ..`
  - `cd /`
  - `cd /information`

### `commit`
- Purpose: save settings.
- Usage: run after `set`/`add`/`delete` style edits when prompt shows pending changes.

### `delete`
- Purpose: delete a node.
- Syntax: `delete <Path> <parameter>`

### `exit` / `quit`
- Purpose: exit CLI to login prompt.
- Syntax: `exit` or `quit`

### `ftp`
- Purpose: open FTP client session to remote server.
- Syntax: `ftp [<server_IP_address>|<hostname>]`
- Note: full local directory control requires root login context.

### `help`
- Purpose: print CLI help and key hints.
- Includes:
  - tab completion guidance
  - command-history arrows
  - reminders about `ls`, `show`
  - control-char escaping hints with backslash

### `list_configuration`
- Purpose: print replayable config commands under current node.
- Behavior:
  - lists configurable params under current node.
  - unconfigured params are prefixed with `#`.
- Useful for migration, backup snapshots, and reapply workflows.

### `ls`
- Purpose: list available directories/subnodes.
- Syntax: `ls` or `ls <path>`

### `opiepasswd`
- Purpose: set one-time password (OTP) for local user and restart sequence number.
- Syntax: `opiepasswd -f -c <username>`
- Operational note from guide: strongly prefer local console use for secure setup.

### `pwd`
- Purpose: print current path.
- Syntax: `pwd`

### `passwd`
- Purpose: change current user password.
- Syntax: `passwd`

### `revert`
- Purpose: undo pending parameter changes.
- Syntax: `revert`

### `save`
- Purpose: Use in wizard mode (--:#- prompt) to save changes
- Syntax: `save` 

### `scp`
- Purpose: secure copy between local/remote endpoints.
- Syntax: `scp [[user@]host1:]file1 [...] [[user@]host2:]file2`

### `set`
- Purpose: set parameter value(s).
- Syntax: `set <Path> <Parameter>=<Value> [<Parameter>=<Value> ...]` (path may be omitted when already in correct node, multiple parameters can be set on one line)
- After `set`, prompt usually switches to pending (`**:-`) until `commit` or `revert`.

### `show`
- Purpose: display current node content, parameters, and current values.
- Syntax: `show`

### `wiz`
- Purpose: interactive eth0 quick setup.
- Covers interface status, IPv4/IPv6 methods, addressing, DNS, hostname, and IPv6 enablement.
- Note: guide states IPv6 enable/disable requires reboot to take effect.

### `connect`
- Purpose: connect to serial port.
- Syntax: `connect <port_name>`
- Common flow:
  - run from `cd /access`
  - authenticate if required (depends on SSO and per-port auth settings)
  - use suspend hotkey (`Ctrl+z`) to return to CLI

### `disconnect`
- Purpose: suspend active target session and return to CLI.
- Hotkey: `Ctrl+z`

### Power control commands: `cycle`, `on`, `off`, `lock`, `unlock`
- Scope: control PDU outlets when PDU is connected and configured with proper profile.
- `lock`/`unlock` are supported on Cyclades and Avocent PDUs.

Power-control operating modes:
1. At `cd /access`:
   - control all outlets by PDU name:
   - `[cycle|on|off] <pdu_name>`
2. At `cd /access/<pdu_name>`:
   - control specific outlet(s):
   - single outlet: `[cycle|on|off] <outlet_number>`
   - range: `[cycle|on|off] <start_outlet_number>-<end_outlet_number>`
   - list: `[cycle|on|off] <outlet_number>,<outlet_number>,...`
3. At `cd /power_management/pdus/<pdu_name>/outlets`:
   - same outlet targeting model, plus lock/unlock where supported.

## 3) Special multi-session commands (section 2.2)
Prerequisite: navigate to enabled configured serial port node:
- `cd /access/<port_name>`

### `sniff`
- Attach as additional view-only user.
- Syntax: `sniff`

### `share`
- Attach as additional read/write user.
- Syntax: `share`

### `list_shared_session`
- List users connected to shared serial port.
- Syntax: `list_shared_session`

### `kill_shared_session`
- Terminate one user’s shared session.
- Syntax: `kill_shared_session <username>`
- Availability requirement: Kill Multi Session option must be enabled in Port Access Rights.

### `sendmsg`
- Send message to a connected shared-session user.
- Syntax: `sendmsg <username> <message>`
- Availability requirement: Send Message Multi Session option must be enabled in Port Access Rights.

### `show_databuf` and `show_appliance_databuf`
- `show_databuf`: view data buffer for the current serial port.
- `show_appliance_databuf`: view appliance session data logs.
- Requirements:
  - data buffering/logging must be enabled.
  - user must be authorized for data buffer management where applicable.

### `clean_databuf` and `clean_appliance_databuf`
- Clear port data buffer or appliance logging buffer.

## 4) Checkbox-equivalent CLI flow (section 2.3)
Guide example: toggling IPv6 with equivalent CLI actions:
1. `cd /network/settings`
2. `show`
3. `set enable_ipv6=yes` or `set enable_ipv6=no`
4. Optional dependent parameters:
   - `set get_dns_from_dhcpv6=yes`
   - `set get_domain_from_dhcpv6=yes`
5. `show` to verify
6. `commit`

## 5) Port access/configuration examples (section 3)
### View appliance and connected entities
1. `cd /access`
2. `show`
3. `ls`

### Connect to serial console
1. `cd /access`
2. `connect <port_name>`
3. enter password if prompted
4. use `Ctrl+z` to suspend session back to CLI

### `ts_menu` notes (section 3.3)
- Supports direct port access flows and listing.
- Key forms shown in guide include:
  - `-u <user> [-l] [-ro] <console port>`
  - `-p` (display TCP port)
  - `-i` (display local IP assigned to serial port)
  - `-e <[^]char>` (set escape character; guide notes default `Ctrl-X` for ts_menu context)

### Configure serial profile examples
- CAS profile:
  1. `set_cas ports/serial_ports/ <port_number>`
  2. `show`
  3. `set status=enabled`
  4. `show`
  5. `save`
- Power profile:
  1. `set_power ports/serial_ports/ <port_number>`
  2. `show`
  3. `set status=enabled`
  4. `save`
  5. `show`
- set_dial-in / set_dial-out
  1. set_dial-in ports/serial_ports/ <port_number>
  or set_dial-out ports/serial_ports/ <port_number>
- set_socket-client
  1. set_socket-client ports/serial_ports/ <port_number>
- clone_ports - Copy Configuration
  1. clone_ports <source_port_number>
  2. set copy_configuration_to=<target_port_numbers>
  3. save
  Example:
     clone_ports 5
     set copy_configuration_to=10,15
     save
- enable_ports / disable_ports
  1. enable_ports <port_numbers>
  or disable_ports <port_numbers>
  Example:
     enable_ports 1,2,3
- reset_port_to_factory
  1. reset_port_to_factory <port_numbers>

## 6) Administrator CLI map (section 4)
Use this section as a quick map; for full parameter trees use transcript pages in section 4.

### `/system`
- Subtrees: `security`, `date_and_time`, `help_and_language`, `general`, `boot_configuration`, `information`, `usage`
- Includes settings for security profile, protocol services, session behavior, and system metadata.

### `/network`
- Subtrees: `settings`, `devices`, `ipv4_static_routes`, `ipv6_static_routes`, `hosts`, `firewall`, `ipsec(vpn)`, `snmp`
- Common flows:
  - static addressing under `/network/devices/<eth0|eth1>/settings`
  - host table add/edit under `/network/hosts`
  - parameter update using `commit`

### `/sensors`
- Appliance internal temperature/voltage and other sensor families.
- Includes `wiz` quick-network configuration behavior in the guide context.

### `/ports`
- Deep tree for serial `cas`, physical settings, data buffering, alerts, power, pools, auto-discovery, RESTful settings, and dial profiles.
- Operational port commands listed by guide:
  - `set_cas`
  - `set_dial-in`
  - `set_dial-out`
  - `set_power`
  - `set_socket-client`
  - `clone_ports`
  - `reset_port_to_factory`
  - `enable_ports`
  - `disable_ports`
- parameter update using `save`

### `/pluggable_devices`
- Discover/configure detected pluggable devices (for supported and enabled scenarios).

### `/authentication`
- `appliance_authentication` and `authentication_servers` families.
- Server types shown include radius, tacacs+, ldap(s)/ad, kerberos, dsview.

### `/users/local_accounts`
- Local accounts, password rules
- Canonical add-user flow from guide:
  - `cd users/local_accounts/user_names`
  - `add`
  - `set user_name=<...> password=<...> confirm_password=<...>`
  - `save`
  - `show`

### `/users/authorization`
- Groups, login profiles, per-user access rights.
- Canonical add-user flow from guide:
  - `cd users/authorization/groups/<group_name>/members`
  - `add`
  - `set local_users=<...>`
  - `save`
  - `show`

### `/events_and_logs`
- Event destinations, data buffering settings, appliance logging, and sensor logging controls.

### `/power_management`
- PDU/UPS/network PDU/network UPS inventory and control flows.
- Includes rename example under `/power_management/pdus`.

### `/active_sessions`
- `show` active session metadata.
- `kill_session <id>` if authorized.

### `/change_password`
- self-service password update node.
- Guide syntax style:
  - `set old_password=<...> new_password=<...> confirm_password=<...>`

## System Tools Commands (from /system_tools)

| Command | Description |
|---------|-------------|
| `reboot` | Reboot the appliance |
| `shutdown` | Shutdown the appliance |
| `factory_defaults` | Reset to factory defaults |
| `save_configuration` | Backup configuration |
| `restore_configuration` | Restore configuration |
| `upgrade_firmware` | Upgrade firmware |
| `generate_|_download_certificate` | SSL certificate management |

## 7) Appendix-level operational edge cases
### Appendix A: recovery when flash does not boot
- Physical access + U-Boot workflow.
- Uses Vertiv firmware file and `recover-flash.sh`.
- Destructive: reinitializes flash and erases configuration/data.

### Appendix B: Migration CLI
- Migration compatibility guide for ACS/ACS5000 script workflows.
- Lists command/value combinations not supported in Migration CLI mode.
- Contains access-rights group naming conventions generated during migration.

### Appendix C: `su` and `sudo`
- `su` syntax and wheel-group guidance.
- `sudo` behavior and `/etc/sudoers` ordering model.
- Includes sudoers alias examples.

## 8) Answering policy for hard tasks
When the user asks for advanced/unusual behavior:
1. Locate exact command family in this file.
2. Validate syntax/constraints against the transcript file.
3. Include prerequisites (permissions, profile enablement, buffering/logging settings, SSO/auth behavior).
4. Provide verify/rollback path (`show`, `list_configuration`, `revert`, session suspend key).
