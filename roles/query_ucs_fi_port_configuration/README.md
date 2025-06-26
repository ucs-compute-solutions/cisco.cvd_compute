# Query Port Configuration of UCS Fabric Interconnect Port(s)

## Overview

The `query_ucs_fi_port_configuration` role is part of the **Cisco UCS Validated Content Collection**. It uses the **Cisco Intersight API** to query and return detailed information about physical ports on UCS Fabric Interconnects (FIs). This helps administrators assess port availability, usage, and breakout configurations prior to provisioning or troubleshooting.

## Features

- Queries UCS Fabric Interconnects for the configuration on specified ports, range of ports or all ports.
- Supports querying ports using:
  - A **range of ports** and a specified `slot_id`.
  - An **explicit list of ports**, including breakout ports like `52/1`, `52/2`, etc.
- Returns a **consolidated report** of filtered port information.
- Output includes administrative and operational state, speed, role, and group data.

## Requirements

* Ansible v2.15.0 or newer
* Python 3.7 or newer (Older Python versions are no longer supported with this collection)
* The [Cisco Intersight](https://docs.ansible.com/ansible/latest/collections/cisco/intersight/index.html) Ansible collection is installed.
* Intersight user account with valid credentials and Cisco Intersight API private key and key ID.

## Role Variables

| Variable                                          | Description                                                            | Default                         |
|---------------------------------------------------|------------------------------------------------------------------------|---------------------------------|
| `query_ucs_fi_port_configuration_api_private_key` | Path to Cisco Intersight API private key.                              | N/A                             |
| `query_ucs_fi_port_configuration_api_key_id`      | Cisco Intersight API key ID.                                           | N/A                             |
| `query_ucs_fi_port_configuration_api_uri`         | Cisco Intersight API URI.                                              | 'https://intersight.com/api/v1' |
| `query_ucs_fi_port_configuration_validate_certs`  | Whether to validate SSL certificates.                                  | 'true'                          |
| `query_ucs_fi_port_configuration_fi_list`         | List of Fabric Interconnect names to query.                            | N/A                             |
| `port_range`                                      | Optional. Range of ports to query (e.g., `41-48`). Requires `slot_id`. | N/A                             |
| `slot_id`                                         | Slot number (e.g., `1`). Required if using `port_range`.               | N/A                             |
| `ports`                                           | Optional. List of port dictionaries (`port_id`, `slot_id`).            | N/A                             |

* query_ucs_fi_port_configuration_api_private_key variable definition can be declared optionally file, vault or .env based.
* If both port_range and ports are not specified, all ports will be queried.

---
## Example Playbooks

### Example 1: Using Explicit List of Ports (Including Breakout Ports)
```yaml
---
- name: Query UCS Fabric Port Query
  hosts: localhost
  gather_facts: no
  vars:
    query_ucs_fi_port_configuration_api_private_key: "{{ 'INTERSIGHT_API_PRIVATE_KEY' }}"
    query_ucs_fi_port_configuration_api_key_id: "{{ lookup('env', 'INTERSIGHT_API_KEY_ID') }}"
    query_ucs_fi_port_configuration_api_uri: "https://intersight.com/api/v1"
    query_ucs_fi_port_configuration_validate_certs: true
    query_ucs_fi_port_configuration_fi_list: ["AC08-6454 FI-A", "AC08-6454 FI-B"]
    ports:
      - port_id: 49
        slot_id: 1
      - port_id: 51
        slot_id: 1
      - port_id: 52/1
        slot_id: 1
      - port_id: 52/2
        slot_id: 1
      - port_id: 52/3
        slot_id: 1
      - port_id: 52/4
        slot_id: 1
  tasks:
    - name: Run FI port query
      ansible.builtin.include_role:
        name: query_ucs_fi_port_configuration

### Output

```json
{
            "fi_name": "AC08-6454 FI-A",
            "filtered_ports": [
                {
                    "admin_state": "Enabled",
                    "aggregate_port_Id": 0,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "100G",
                    "oper_state": "up",
                    "parent_port": "Physical Port",
                    "port_Id": "49",
                    "port_group": {
                        "ClassId": "mo.MoRef",
                        "Moid": "6758c0dc61767530017381fd",
                        "ObjectType": "port.Group",
                        "link": "https://intersight.com/api/v1/port/Groups/6758c0dc61767530017381fd"
                    },
                    "role": "Appliance",
                    "slot_Id": 1,
                    "switch_id": "A"
                },
                {
                    "admin_state": "Disabled",
                    "aggregate_port_Id": 0,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "auto",
                    "oper_state": "down",
                    "parent_port": "Physical Port",
                    "port_Id": "51",
                    "port_group": {
                        "ClassId": "mo.MoRef",
                        "Moid": "6758c0dc61767530017381fd",
                        "ObjectType": "port.Group",
                        "link": "https://intersight.com/api/v1/port/Groups/6758c0dc61767530017381fd"
                    },
                    "role": "unknown",
                    "slot_Id": 1,
                    "switch_id": "A"
                },
                {
                    "admin_state": "Enabled",
                    "aggregate_port_Id": 52,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "25G",
                    "oper_state": "up",
                    "parent_port": 52,
                    "port_Id": "52/1",
                    "port_group": null,
                    "role": "Server",
                    "slot_Id": 1,
                    "switch_id": "A"
                },
                {
                    "admin_state": "Enabled",
                    "aggregate_port_Id": 52,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "25G",
                    "oper_state": "up",
                    "parent_port": 52,
                    "port_Id": "52/2",
                    "port_group": null,
                    "role": "Server",
                    "slot_Id": 1,
                    "switch_id": "A"
                },
                {
                    "admin_state": "Disabled",
                    "aggregate_port_Id": 52,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "auto",
                    "oper_state": "down",
                    "parent_port": 52,
                    "port_Id": "52/3",
                    "port_group": null,
                    "role": "unknown",
                    "slot_Id": 1,
                    "switch_id": "A"
                },
                {
                    "admin_state": "Disabled",
                    "aggregate_port_Id": 52,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "auto",
                    "oper_state": "down",
                    "parent_port": 52,
                    "port_Id": "52/4",
                    "port_group": null,
                    "role": "unknown",
                    "slot_Id": 1,
                    "switch_id": "A"
                }
            ]
        },
        {
            "fi_name": "AC08-6454 FI-B",
            "filtered_ports": [
                {
                    "admin_state": "Enabled",
                    "aggregate_port_Id": 0,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "100G",
                    "oper_state": "up",
                    "parent_port": "Physical Port",
                    "port_Id": "49",
                    "port_group": {
                        "ClassId": "mo.MoRef",
                        "Moid": "6758c0d061767530017375b1",
                        "ObjectType": "port.Group",
                        "link": "https://intersight.com/api/v1/port/Groups/6758c0d061767530017375b1"
                    },
                    "role": "Appliance",
                    "slot_Id": 1,
                    "switch_id": "B"
                },
                {
                    "admin_state": "Disabled",
                    "aggregate_port_Id": 0,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "auto",
                    "oper_state": "down",
                    "parent_port": "Physical Port",
                    "port_Id": "51",
                    "port_group": {
                        "ClassId": "mo.MoRef",
                        "Moid": "6758c0d061767530017375b1",
                        "ObjectType": "port.Group",
                        "link": "https://intersight.com/api/v1/port/Groups/6758c0d061767530017375b1"
                    },
                    "role": "unknown",
                    "slot_Id": 1,
                    "switch_id": "B"
                },
                {
                    "admin_state": "Enabled",
                    "aggregate_port_Id": 52,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "25G",
                    "oper_state": "up",
                    "parent_port": 52,
                    "port_Id": "52/1",
                    "port_group": null,
                    "role": "Server",
                    "slot_Id": 1,
                    "switch_id": "B"
                },
                {
                    "admin_state": "Enabled",
                    "aggregate_port_Id": 52,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "25G",
                    "oper_state": "up",
                    "parent_port": 52,
                    "port_Id": "52/2",
                    "port_group": null,
                    "role": "Server",
                    "slot_Id": 1,
                    "switch_id": "B"
                },
                {
                    "admin_state": "Disabled",
                    "aggregate_port_Id": 52,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "auto",
                    "oper_state": "down",
                    "parent_port": 52,
                    "port_Id": "52/3",
                    "port_group": null,
                    "role": "unknown",
                    "slot_Id": 1,
                    "switch_id": "B"
                },
                {
                    "admin_state": "Disabled",
                    "aggregate_port_Id": 52,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "auto",
                    "oper_state": "down",
                    "parent_port": 52,
                    "port_Id": "52/4",
                    "port_group": null,
                    "role": "unknown",
                    "slot_Id": 1,
                    "switch_id": "B"
                }
            ]
        }

```

### Example 2: Using Range of Ports (Without Breakout Ports)

```yaml
---
- name: Query UCS Fabric Port Query
  hosts: localhost
  gather_facts: no
  vars:
    query_ucs_fi_port_configuration_api_private_key: "{{ lookup('env', 'INTERSIGHT_API_PRIVATE_KEY') }}"
    query_ucs_fi_port_configuration_api_key_id: "{{ lookup('env', 'INTERSIGHT_API_KEY_ID') }}"
    query_ucs_fi_port_configuration_api_uri: "https://intersight.com/api/v1"
    query_ucs_fi_port_configuration_validate_certs: true
    query_ucs_fi_port_configuration_fi_list: ["AC08-6454 FI-A", "AC08-6454 FI-B"]
    port_range: 41-42
    slot_id: 1 
  tasks:
    - name: Run FI port query
      ansible.builtin.include_role:
        name: query_ucs_fi_port_configuration

### Output

```json
[
        {
            "fi_name": "AC08-6454 FI-A",
            "filtered_ports": [
                {
                    "admin_state": "Disabled",
                    "aggregate_port_Id": 0,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "auto",
                    "oper_state": "down",
                    "parent_port": "Physical Port",
                    "port_Id": "41",
                    "port_group": {
                        "ClassId": "mo.MoRef",
                        "Moid": "6758c0dc61767530017381fd",
                        "ObjectType": "port.Group",
                        "link": "https://intersight.com/api/v1/port/Groups/6758c0dc61767530017381fd"
                    },
                    "role": "unknown",
                    "slot_Id": 1,
                    "switch_id": "A"
                },
                {
                    "admin_state": "Disabled",
                    "aggregate_port_Id": 0,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "auto",
                    "oper_state": "down",
                    "parent_port": "Physical Port",
                    "port_Id": "42",
                    "port_group": {
                        "ClassId": "mo.MoRef",
                        "Moid": "6758c0dc61767530017381fd",
                        "ObjectType": "port.Group",
                        "link": "https://intersight.com/api/v1/port/Groups/6758c0dc61767530017381fd"
                    },
                    "role": "unknown",
                    "slot_Id": 1,
                    "switch_id": "A"
                }
            ]
        },
        {
            "fi_name": "AC08-6454 FI-B",
            "filtered_ports": [
                {
                    "admin_state": "Disabled",
                    "aggregate_port_Id": 0,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "auto",
                    "oper_state": "down",
                    "parent_port": "Physical Port",
                    "port_Id": "41",
                    "port_group": {
                        "ClassId": "mo.MoRef",
                        "Moid": "6758c0d061767530017375b1",
                        "ObjectType": "port.Group",
                        "link": "https://intersight.com/api/v1/port/Groups/6758c0d061767530017375b1"
                    },
                    "role": "unknown",
                    "slot_Id": 1,
                    "switch_id": "B"
                },
                {
                    "admin_state": "Disabled",
                    "aggregate_port_Id": 0,
                    "class_id": "ether.PhysicalPort",
                    "oper_speed": "auto",
                    "oper_state": "down",
                    "parent_port": "Physical Port",
                    "port_Id": "42",
                    "port_group": {
                        "ClassId": "mo.MoRef",
                        "Moid": "6758c0d061767530017375b1",
                        "ObjectType": "port.Group",
                        "link": "https://intersight.com/api/v1/port/Groups/6758c0d061767530017375b1"
                    },
                    "role": "unknown",
                    "slot_Id": 1,
                    "switch_id": "B"
                }
            ]
        }
    ]

```