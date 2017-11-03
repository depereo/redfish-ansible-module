# Playbooks

| Playbook                   | Description                                   |
|----------------------------|-----------------------------------------------|
| get_firmware_inventory.yml | Get device firmware information               |
| get_system_inventory.yml   | Get device information                        |
| get_system_logs.yml        | Manage SE & LC logs                           |
| manage_bios.yml            | Manage BIOS settings                          |
| manage_cooling.yml         | Get cooling information (i.e. fans)           |
| manage_idrac_power.yml     | Restart iDRAC                                 |
| manage_idrac_settings.yml  | Manage iDRAC settings                         |
| manage_scp.yml             | Manage SCP files                              |
| manage_storage.yml         | Get storage information (controller & disks)  |
| manage_system_power.yml    | Restart, shutdown, power-cycle server, etc.   |
| manage_users.yml           | Add, edit & remove users from iDRAC           |
| setupoutputfile.yml        | Set up file with results in JSON format       |

# Examples

The best way to learn how to use these playbooks is by looking at each one. Most playbooks can be run as-is, whereas others require that you run specific sections using tags or by commenting out what you don't want.

## Get system inventory

```bash
$ ansible-playbook get_system_inventory.yml
```

Sample:

```bash
  - name: Getting system inventory
    local_action: >
       idrac category=Inventory command=GetInventory idracip={{idracip}}
       idracuser={{idracuser}} idracpswd={{idracpswd}}
    register: result

  - name: Copying results to file
    local_action: copy content={{ result | to_nice_json }}
       dest={{template}}_inventory.json
```

## Get firmware inventory

```bash
$ ansible-playbook get_firmware_inventory.yml
```

```bash
  - name: Get Firmware Inventory
    local_action: >
       idrac category=Firmware command=GetInventory idracip={{idracip}}
       idracuser={{idracuser}} idracpswd={{idracpswd}}
    register: result
    ignore_errors: yes

  - name: Copy inventory to file
    local_action: copy content={{ result | to_nice_json }}
       dest={{template}}_firmware_inventory.json

```

## Get system logs

```bash
$ ansible-playbook get_system_logs.yml
```

```bash
  # ------------------------------------------------------------------------
  - name: Get SE Logs
    local_action: >
       idrac category=Logs command=GetSeLogs idracip={{ idracip }}
            idracuser={{ idracuser }} idracpswd={{ idracpswd }}
    register: result
    tags: selog

  - name: Place SE Logs in file
    local_action: copy content={{ result | to_nice_json }}
             dest={{template}}_SELog.json
    tags: selog

  # ------------------------------------------------------------------------
  - name: Get LC Logs
    local_action: >
       idrac category=Logs command=GetLcLogs idracip={{ idracip }}
            idracuser={{ idracuser }} idracpswd={{ idracpswd }}
    register: result
    tags: lclog

  - name: Place LC Logs in file
    local_action: copy content={{ result | to_nice_json }}
             dest={{template}}_LCLog.json
    tags: lclog
```

More coming soon