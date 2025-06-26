# Query UCS Fabric Interconnect for Available Ports

## Overview

The `query_ucs_fi_available_ports` role is part of the **Cisco UCS Validated Content Collection**. It queries **Cisco UCS Fabric Interconnects (FIs)** using the **Cisco Intersight API** to determine port usage and availability before provisioning new ports.

## Features

- Queries UCS Fabric Interconnects for available ports.
- Supports port provisioning checks using range of ports, explicit port definitions or all ports if both range and explicit ports are not defined.
- Supports breakout port formats such as `52/1`, `52/2`, etc.
- Provides a summarized provisioning report.
- Fails gracefully with an error if the requested ports are unavailable.

## Requirements

- **Cisco Intersight API** access (API key and private key).
- Python dependencies and the `cisco.intersight` collection must be installed.

## Role Variables

| Variable                                        | Description                                                        | Default                         |
|-------------------------------------------------|--------------------------------------------------------------------|---------------------------------|
| `query_ucs_fi_available_ports_api_private_key`  | Path to Cisco Intersight API private key.                          | N/A                             |
| `query_ucs_fi_available_ports_api_key_id`       | Cisco Intersight API key ID.                                       | N/A                             |
| `query_ucs_fi_available_ports_api_uri`          | Cisco Intersight API URI.                                          | 'https://intersight.com/api/v1' |
| `query_ucs_fi_available_ports_validate_certs`   | Whether to validate SSL certificates.                              | 'true'                          |
| `query_ucs_fi_available_ports_fi_list`          | List of Fabric Interconnect names to be queried.                   | N/A                             |
| `port_range`                                    | Optional. Range of ports to check (e.g., `41-48`).                 | N/A                             |
| `slot_id`                                       | Required when using `port_range`. Denotes slot number (e.g., `1`). | N/A                             |
| `ports`                                         | Optional. List of ports with `port_id` and `slot_id`.              | N/A                             |

* query_ucs_fi_available_ports_api_private_key variable definition can be declared optionally file, vault or .env based.
* If both port_range and ports are not specified, all ports will be queried.

## Example Playbooks

### **Example 1: Using Explicit List of Ports (Breakout & Non-Breakout)**
```yaml
---
- name: Query UCS Fabric Interconnect Provisioning
  hosts: localhost
  gather_facts: no
  vars:
    query_ucs_fi_available_ports_api_private_key: "{{ lookup('env', 'INTERSIGHT_API_PRIVATE_KEY') }}"
    query_ucs_fi_available_ports_api_key_id: "{{ lookup('env', 'INTERSIGHT_API_KEY_ID') }}"
    query_ucs_fi_available_ports_api_uri: "https://intersight.com/api/v1"
    query_ucs_fi_available_ports_validate_certs: true
    query_ucs_fi_available_ports_fi_list: ["AC08-6454 FI-A", "AC08-6454 FI-B"]
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
    - name: Run Query UCS FI Available Ports
      ansible.builtin.include_role:
        name: query_ucs_fi_available_ports
### Output

```json
{
  "msg": [
    {
      "fi_name": "AC08-6454 FI-A",
      "available_ports": "2",
      "total_ports": "2",
      "status": "Provisioning can proceed"
    },
    {
      "fi_name": "AC08-6454 FI-B",
      "available_ports": "2",
      "total_ports": "2",
      "status": "Provisioning can proceed"
    }
  ]
}
```

### **Example 2: Using Port Range without Breakout Ports**
```yaml
---
- name: Query UCS Fabric Interconnect Provisioning
  hosts: localhost
  gather_facts: no
  vars:
    query_ucs_fi_available_ports_api_private_key: "{{ INTERSIGHT_API_PRIVATE_KEY }}"
    query_ucs_fi_available_ports_api_key_id: "{{ INTERSIGHT_API_KEY_ID }}"
    query_ucs_fi_available_ports_api_uri: "https://intersight.com/api/v1"
    query_ucs_fi_available_ports_validate_certs: true
    query_ucs_fi_available_ports_fi_list: ["AC08-6454 FI-A", "AC08-6454 FI-B"]
    port_range: 41-42
    slot_id: 1
  tasks:
    - name: Run Query UCS FI Available Ports
      ansible.builtin.include_role:
        name: query_ucs_fi_available_ports
### Output

```json
fatal: [localhost]: FAILED! => {
  "changed": false,
  "msg": "Provisioning cannot be proceeded for AC08-6454 FI-A.\nTotal queried ports: 6\nAvailable ports     : 3\n\nPlease check the Fabric Interconnect configuration and ensure all required ports are available."
}
```