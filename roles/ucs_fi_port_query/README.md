# UCS Fabric Interconnect Port Query Role

## Overview

The `ucs_fi_port_query` role is part of the **Cisco UCS Validated Content Collection**. It uses the **Cisco Intersight API** to query and return detailed information about physical ports on UCS Fabric Interconnects (FIs). This helps administrators assess port availability, usage, and breakout configurations prior to provisioning or troubleshooting.

## Features

- Queries specified UCS Fabric Interconnects via Intersight.
- Supports querying ports using:
  - A **range of ports** and a specified `slot_id`.
  - An **explicit list of ports**, including breakout ports like `52/1`, `52/2`, etc.
- Returns a **consolidated report** of filtered port information.
- Output includes administrative and operational state, speed, role, and group data.

## Requirements

- Cisco Intersight API access credentials.
- Python environment with required dependencies.
- Installed collection: `cisco.intersight`.

## Role Variables

| Variable                              | Description                                                              | Default                        |
|--------------------------------------|--------------------------------------------------------------------------|--------------------------------|
| `ucs_fi_port_query_api_private_key`  | Path to Cisco Intersight API private key.                                | N/A                            |
| `ucs_fi_port_query_api_key_id`       | Cisco Intersight API key ID.                                             | N/A                            |
| `ucs_fi_port_query_api_uri`          | Cisco Intersight API URI.                                                | N/A |
| `ucs_fi_port_query_validate_certs`   | Whether to validate SSL certificates.                                    | N/A                         |
| `ucs_fi_port_query_fi_list`          | List of Fabric Interconnect names to query.                              | N/A                            |
| `port_range`                         | Optional. Range of ports to query (e.g., `41-48`). Requires `slot_id`.   | N/A                            |
| `slot_id`                            | Slot number (e.g., `1`). Required if using `port_range`.                 | N/A                            |
| `ports`                              | Optional. List of port dictionaries (`port_id`, `slot_id`).              | N/A                            |

---

## Example Playbooks

### Example 1: Using Explicit List of Ports (Including Breakout Ports)
```yaml
---
- name: Query UCS Fabric Port Query
  hosts: localhost
  gather_facts: no
  vars:
    ucs_fi_port_query_api_private_key: "{{ lookup('env', 'INTERSIGHT_API_PRIVATE_KEY') }}"
    ucs_fi_port_query_api_key_id: "{{ lookup('env', 'INTERSIGHT_API_KEY_ID') }}"
    ucs_fi_port_query_api_uri: "https://intersight.com/api/v1"
    ucs_fi_port_query_validate_certs: true
    ucs_fi_port_query_fi_list: ["AC08-6454 FI-A", "AC08-6454 FI-B"]
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
        name: ucs_fi_port_query

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

### Example 2: Using Explicit List of Ports (Including Breakout Ports)

```yaml
---
- name: Query UCS Fabric Port Query
  hosts: localhost
  gather_facts: no
  vars:
    ucs_fi_port_query_api_private_key: "{{ lookup('env', 'INTERSIGHT_API_PRIVATE_KEY') }}"
    ucs_fi_port_query_api_key_id: "{{ lookup('env', 'INTERSIGHT_API_KEY_ID') }}"
    ucs_fi_port_query_api_uri: "https://intersight.com/api/v1"
    ucs_fi_port_query_validate_certs: true
    ucs_fi_port_query_fi_list: ["AC08-6454 FI-A", "AC08-6454 FI-B"]
    port_range: 41-42
    slot_id: 1 
  tasks:
    - name: Run FI port query
      ansible.builtin.include_role:
        name: ucs_fi_port_query

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