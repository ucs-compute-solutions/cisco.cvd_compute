# Update Fabric Interconnect Port Policy

## Overview

Each fabric interconnect has a set of ports in a fixed port module that you can configure as either server ports or uplink Ethernet ports. These ports are not reserved and they cannot be used by a Cisco UCS instance until you configure them. 

To extend an existing Cisco UCS domain with additional Cisco UCS C-Series servers or Cisco UCS X-Series compute nodes new ports need to be configured as server ports. They will handle data traffic between the fabric interconnect and the adapter cards.

## Features

- Configure ports as server ports on port policy via Intersight.
- Supports breakout ports.
- Supports multiple port policies and domain profiles.

## Requirements

* Ansible v2.15.0 or newer
* Python 3.7 or newer (Older Python versions are no longer supported with this collection)
* The [Cisco Intersight](https://docs.ansible.com/ansible/latest/collections/cisco/intersight/index.html) Ansible collection is installed.
* Intersight user account with valid credentials and Cisco Intersight API private key and key ID.

## Role Variables

| Variable                                                | Description                                                                             | Default                         |
|---------------------------------------------------------|-----------------------------------------------------------------------------------------|---------------------------------|
| `update_ucs_fi_port_policy_api_private_key`             | Path to Cisco Intersight API private key.                                               |                                 |
| `update_ucs_fi_port_policy_api_key_id`                  | Cisco Intersight API key ID.                                                            |                                 |
| `update_ucs_fi_port_policy_api_uri`                     | Cisco Intersight API URI.                                                               | <https://intersight.com/api/v1> |
| `update_ucs_fi_port_policy_validate_certs`              | Validate SSL certificates (*false* for self-signed certificates)                        | true                            |
| `update_ucs_fi_port_policy_port_id_list`                | Ports that will be configured as server in the selected policies.                       | **Required** (no default)       |
| `update_ucs_fi_port_policy_slot_id_list`                | Slot ids that will be configured in the selected policies.                              | [1]                             |
| `update_ucs_fi_port_policy_name_of_domain_profile_list` | List of UCS domain profiles names.                                                      | **Required** (no default)       |
| `update_ucs_fi_port_policy_port_policy_list`            | List of port policy names.                                                              | **Required** (no default)       |
| `update_ucs_fi_port_policy_breakout_option`             | Breakout option string. Chosen from: 'BreakoutEthernet10G' Or 'BreakoutEthernet25G'     |                                 |

Notes:
* If update_ucs_fi_port_policy_port_id_list is set, you must set update_ucs_fi_port_policy_breakout_option and vice versa.
* The format of variable update_ucs_fi_port_policy_port_id_list can be breakout port id (e.g. 52/1) or regular port id (e.g. 5). But it can't be both in the same list.
* Intersight handles the Domain profile internally as A and B, therefore a list is required even with a single domain profile visible in Intersight.
* update_ucs_fi_port_policy_api_private_key variable definition can be declared optionally file, vault or .env based.

## Example 1 (without breakout):

```yaml
    ---
    - name: Update UCS Fabric Interconnect port role as server port
      hosts: localhost
      vars:
        update_ucs_fi_port_policy_api_private_key: "{{ 'INTERSIGHT_API_PRIVATE_KEY'}}"
        update_ucs_fi_port_policy_api_key_id: "{{ 'INTERSIGHT_API_KEY_ID' }}"
        update_ucs_fi_port_policy_port_id_list: [5, 6, 7, 8]
        update_ucs_fi_port_policy_port_policy_list: ["AC08-6454-A-Port-Policy", "AC08-6454-B-Port-Policy"]
        update_ucs_fi_port_policy_name_of_domain_profile_list: ["AC08-6454-Domain-Profile-A", "AC08-6454-Domain-Profile-B"]
      roles:
        - update_ucs_fi_port_policy
```

## Example 2 (with breakout): 

```yaml
    - name: Update UCS Fabric Interconnect port role as server port - Breakout option
      hosts: localhost
      vars:
        update_ucs_fi_port_policy_api_private_key: "{{ 'INTERSIGHT_API_PRIVATE_KEY' }}"
        update_ucs_fi_port_policy_api_key_id: "{{ 'INTERSIGHT_API_KEY_ID' }}"
        update_ucs_fi_port_policy_port_policy_list: ["AC08-6454-A-Port-Policy", "AC08-6454-B-Port-Policy"]
        update_ucs_fi_port_policy_name_of_domain_profile_list: ["AC08-6454-Domain-Profile-A", "AC08-6454-Domain-Profile-B"]
        update_ucs_fi_port_policy_aggregate_port_id_list: [52/1, 52/2]
        update_ucs_fi_port_policy_breakout_option: "BreakoutEthernet25G"
      roles:
        - update_ucs_fi_port_policy    
```

The role test folder contains a validate_ports.yml file you can use to verify the ports are configured correctly. 
In this case copy the yml file to the same folder as the example playbook and extend the playbook with the following lines:

      tasks:
        - name: Verify the ports got correctly configured as per policy
          ansible.builtin.include_tasks: validate_ports.yml
          loop: "{{ update_ucs_fi_port_policy_port_policy_list }}"
          loop_control:
            loop_var: policy_name
