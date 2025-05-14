# Add vNIC From Template role

Add a vNIC from a vNIC Template to a LAN Connectivity Policy. 

## Overiew

UCS LAN Connectivity Policy is a configuration template used to define the networking settings for UCS servers.
A vNIC inside this policy represents a virtual network interface assigned to a server, providing the server with network connectivity.
This LAN Connectivity Policy, including the vNIC configuration, is applied to a Server Profile Template, And then Deployed to the servers Attched to that template ensuring consistent network configurations across multiple servers within the UCS environment.

The **add_ucs_vnic** role is part of the **Cisco UCS Validated Content Collection**. It uses the **Cisco Intersight REST API** to add a vNIC to a LAN Connectivity Policy and deploy a Server Profile Template to Cisco UCS Servers.

## Features

- Automates Add vNIC From Template.
- Supports Adding a vNIC with an existing vNIC Template to an existing Server Profile Template.

## Requirements

* Server Profile Template with Lan Connectivity Policy is configured in Intersight
* vNIC Template exist in Intersight
* Ansible v2.15.0 or newer
* Python 3.7 or newer (Older Python versions are no longer supported with this collection)
* The [Cisco Intersight](https://docs.ansible.com/ansible/latest/collections/cisco/intersight/index.html) Ansible collection is installed.
* Intersight user account with valid credentials and Cisco Intersight API private key and key ID.

## Role Variables

| Variable                                                 | Description                                                            | Default                         |
|----------------------------------------------------------|------------------------------------------------------------------------|---------------------------------|
| `add_ucs_vnic_api_private_key`                           | Path to Cisco Intersight API private key file on the host.             | **Required** (no default)       |
| `add_ucs_vnic_api_key_id`                                | Cisco Intersight API key ID.                                           | **Required** (no default)       |
| `add_ucs_vnic_api_uri`                                   | Cisco Intersight API URI.                                              | `https://intersight.com/api/v1` |
| `add_ucs_vnic_validate_certs`                            | Validate SSL certificates (set to `false` if using self-signed certs). | 'true'                          |
| `add_ucs_vnic_server_profile_template_name`              | Server Profile Template name to apply changes to                       | **Required** (no default)       |
| `add_ucs_vnic_template_name`                             | vNIC Template name to use for adding new vNIC                          | **Required** (no default)       |
| `add_ucs_vnic_name`                                      | New vNIC name                                                          | **Required** (no default)       |

* Note: if add_ucs_vnic_api_private_key is stored in a file on the host, we expect a path to this file. It is best practice to use environment variables to store the private key and key ID.

## Example Playbook

```yaml
    ---
    - name: Add vNIC from Template to an existing server
      hosts: localhost
      vars:
        add_ucs_vnic_api_private_key: "{{ INTERSIGHT_API_PRIVATE_KEY }}"
        add_ucs_vnic_api_key_id: "{{ INTERSIGHT_API_KEY_ID }}"
        add_ucs_vnic_server_profile_template_name: "Server-Profile-Template-Name"
        add_ucs_vnic_template_name: "vNIC-Template-Name"
        add_ucs_vnic_name: "eno6"
      roles:
        - add_ucs_vnic
```