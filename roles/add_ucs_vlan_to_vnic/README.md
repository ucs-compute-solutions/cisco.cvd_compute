# Cisco UCS adding vlan to vnic role

## Features

A role that creates **Cisco UCS Ethernet Network Group Policy** in Intersight.
This is used to add a VLAN to a vNIC in Intersight.

## Requirements

* Ansible v2.15.0 or newer
* Python 3.7 or newer (Older Python versions are no longer supported with this collection)
* The [Cisco Intersight](https://docs.ansible.com/ansible/latest/collections/cisco/intersight/index.html) Ansible collection is installed.
* Intersight user account with valid credentials and Cisco Intersight API private key and key ID.

## Role Variables

| Variable                                                                     | Description                                                                             | Default                         |
|------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|---------------------------------|
| `add_ucs_vlan_to_vnic_api_private_key`                                       | Path to Cisco Intersight API private key file on the host.                              |                                 |
| `add_ucs_vlan_to_vnic_api_key_id`                                            | Cisco Intersight API key ID.                                                            |                                 |
| `add_ucs_vlan_to_vnic_api_uri`                                               | Cisco Intersight API URI.                                                               | `https://intersight.com/api/v1` |
| `add_ucs_vlan_to_vnic_validate_certs`                                        | Validate SSL certificates (set to `false` if using self-signed certs).                  | 'true'                          |
| `add_ucs_vlan_to_vnic_name_of_fi_vlan_policy`                                | Name of the VLAN policy on which the new vlan will be created.                          | **Required** (no default)       |
| `add_ucs_vlan_to_vnic_vlan_multicast_policy`                                 | Name of the VLAN multicast policy which will be attached when the vlan will be created. | **Required** (no default)       |
| `add_ucs_vlan_to_vnic_vlan_id`                                               | ID of the created vlan.                                                                 | **Required** (no default)       |
| `add_ucs_vlan_to_vnic_vlan_name`                                             | Name of the created vlan.                                                               | **Required** (no default)       |
| `add_ucs_vlan_to_vnic_organization_name`                                     | Organization name on which the vlan is created.                                         | **Required** (no default)       |
| `add_ucs_vlan_to_vnic_ethernet_network_group_name`                           | Ethernet network group on which the newly created vlan wil lbe attached to.             | **Required** (no default)       |
| `add_ucs_vlan_to_vnic_name_of_domain_profile_list`                           | List of UCS domain profiles names.                                                      | **Required** (no default)       |

* Note: add_ucs_vlan_to_vnic_api_private_key variable definition can be declared optionally file, vault or .env based.

## Example Playbook
```yaml
    ---
    - name: Test Adding Vlan to a vNic in Intersight
      hosts: localhost
      vars:
        add_ucs_vlan_to_vnic_api_private_key: "{{ lookup('env', 'INTERSIGHT_API_PRIVATE_KEY') }}"
        add_ucs_vlan_to_vnic_api_key_id: "{{ lookup('env', 'INTERSIGHT_API_KEY_ID') }}"
        add_ucs_vlan_to_vnic_name_of_fi_vlan_policy: "AC08-6454-VLAN-Policy"
        add_ucs_vlan_to_vnic_vlan_multicast_policy: "AC08-MCAST"
        add_ucs_vlan_to_vnic_vlan_id: 3002
        add_ucs_vlan_to_vnic_vlan_name: "TEST-VLAN"
        add_ucs_vlan_to_vnic_organization_name: "AC08-OCP"
        add_ucs_vlan_to_vnic_ethernet_network_group_name: "AC08-OCP-iSCSI-NVMe-TCP-A-NetGrp-Policy"
        add_ucs_vlan_to_vnic_name_of_domain_profile_list: [ "AC08-6454-Domain-Profile-A", "AC08-6454-Domain-Profile-B" ]
      roles:
        - add_ucs_vlan_to_vnic
```
        