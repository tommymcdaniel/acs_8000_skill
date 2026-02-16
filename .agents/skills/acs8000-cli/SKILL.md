---
name: acs8000-cli
description: Use this skill when a user needs help operating a Vertiv/Avocent ACS800 or ACS8000 Advanced Console System through its CLI, including login/navigation, path discovery, configuration changes with set/add/delete and commit/revert, serial console access, multi-session commands, power control, and translating Web Manager actions to CLI paths.
---

# ACS800/8000 CLI operator guide
Use this workflow to answer requests about the ACS800/8000 CLI command interface described in the Vertiv command reference guide.

## Apply this operating pattern
1. Determine user context first.
   - Confirm whether the user is an administrator or regular user (available top-level paths differ).
   - Confirm connection method: local console, SSH, Telnet, or Web Manager Appliance Viewer.
2. Discover before changing.
   - Use `pwd`, `ls`, and `show` to inspect current location and values.
   - Use `cd <space><tab><tab>` to discover valid path elements at any level.
3. Make configuration changes explicitly.
   - Use `set <parameter>=<value>` for parameter updates.
   - Use `add <path>` to add a node.
   - Use `delete <path> <parameter>` to remove a node.
4. Finalize every edit session.
   - Use `commit` to save changes.
   - Use `revert` to discard pending changes.
5. Verify after each change.
   - Re-run `show` and/or `list_configuration`.
   - Confirm prompt state is clean (no pending changes indicator).

## Explain prompt and navigation semantics
- Treat CLI as a tree of paths.
- Use `/` separators, and translate Web Manager spaces to underscores.
  - Example: `Users - Local Accounts - User Names` becomes `cd /users/local_accounts/user_names`.
- Use `cd ..` to move up one level, `cd ../..` for multiple levels, `cd /` for root.
- Explain prompt markers when relevant:
  - `--:- ... cli->` indicates normal state.
  - `**:- ... cli->` indicates unsaved changes pending commit/revert.
  - Wizard prompts may appear as `--:#- [path] cli->`.
- Treat commands as case-sensitive.

## Provide command help in this order
1. Start with a minimal command sequence for the task.
2. Show placeholders explicitly (for example `<serial_port_id>` or `<PDU_ID>`).
3. Add verification commands (`show`, `ls`, `list_configuration`).
4. Add rollback/fallback (`revert` or `Ctrl+z` for session suspend) when applicable.

## Use these high-value command patterns
### Session basics
- Launch CLI from Linux shell: `cli`
- Show help: `help`
- Show current path: `pwd`
- List child nodes: `ls`
- Show current node values: `show`
- Exit CLI: `exit` or `quit`

### Configuration lifecycle
- Set value: `set <path_optional> <parameter>=<value>`
- Add node: `add <path>`
- Delete node: `delete <path> <parameter>`
- Save: `commit`
- Undo pending edits: `revert`
- Export replayable config for current node: `list_configuration`

### Serial console access
- Enter access level: `cd /access`
- Connect to port: `connect <port_name>`
- Suspend target session back to CLI: `Ctrl+z`

### Multi-session access (at `cd /access/<serial_port_ID>`)
- View-only attach: `sniff`
- Read/write attach: `share`
- List shared users: `list_shared_session`
- Terminate shared user: `kill_shared_session <username>`
- Send message: `sendmsg <username> <message>`
- Show/clear port data buffer: `show_databuf`, `clean_databuf`
- Show/clear appliance data log: `show_appliance_databuf`, `clean_appliance_databuf`

### Power control
- From `cd /access`, control all outlets by target/PDU:
  - `on <PDU_ID_or_target>`
  - `off <PDU_ID_or_target>`
  - `cycle <PDU_ID_or_target>`
- From `cd /access/<PDU_ID>`, control specific outlets:
  - Single: `on <outlet>`
  - Range: `off <start>-<end>`
  - List: `cycle <a>,<b>,<c>`
- From `cd /power_management/pdus/<PDU_ID>/outlets`, also use:
  - `lock <outlet(s)>`
  - `unlock <outlet(s)>`

## Use task recipes
### Inspect appliance and reachable ports
1. `cd /access`
2. `show`
3. `ls`

### Connect to a serial console
1. `cd /access`
2. `connect <serial_port_name>`
3. Authenticate if prompted.
4. Use `Ctrl+z` to suspend and return to CLI.

### Toggle IPv6 (checkbox-equivalent flow)
1. `cd /network/settings`
2. `show`
3. `set enable_ipv6=yes` or `set enable_ipv6=no`
4. Optionally set:
   - `set get_dns_from_dhcpv6=yes`
   - `set get_domain_from_dhcpv6=yes`
5. `show`
6. `commit`

### Configure a serial port for CAS
1. `set_cas ports/serial_ports/ <port_number>`
2. `show`
3. `set status=enabled`
4. `show`
5. `save` (wizard flow) or `commit` (standard flow) depending on prompt mode.

### Configure a serial port for Power profile
1. `set_power ports/serial_ports/ <port_number>`
2. `show`
3. `set status=enabled`
4. `save`/`commit`
5. `show` to verify profile and status.

## Escalate to bundled reference when needed
- Start with `references/core-commands.md` for detailed command semantics, prerequisites, and admin path mapping.
- Use `references/section-index.md` to jump to exact manual sections.
- Use `references/acs8000-command-reference-transcript.md` for near-complete guide text when handling uncommon, advanced, or edge-case operations.
- For uncommon operations not covered in recipes, derive navigation path via `cd <tab><tab>`, then use `show` and `help` before proposing destructive actions.

### Transcript search patterns
- Search by command section: `2.1.<n>`, `2.2.<n>`, `2.3`
- Search by operation name: `set_power`, `set_dial-in`, `clone_ports`, `kill_shared_session`, `show_databuf`
- Search by admin path: `/system/`, `/network/`, `/ports/`, `/users/`, `/events_and_logs/`, `/power_management/`
- Search edge cases: `Appendix A`, `Appendix B`, `Appendix C`, `Migration CLI`, `recover-flash.sh`, `sudo`

