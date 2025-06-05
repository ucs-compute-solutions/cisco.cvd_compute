# Derive chassis profile from chassis profile template role

Derive Cisco UCS chassis profile from Cisco UCS Chassis Profile Template. 

## Overiew

UCS Chassis profile templates enable you to define a template from which chassis profiles can be easily derived and deployed at scale. This method is particularly useful when you need a large number of chassis profiles that use the same set of policies. Any property modification made in the template is inherited by the derived profiles. You can deploy these modified profiles individually.

The **derive_ucs_chassis_profile_template** role is part of the **Cisco UCS Validated Content Collection**. It uses the **Cisco Intersight REST API** to derive and deploy a UCS Chassis Profile Template to a Cisco UCS chassis.

## Features

- Derives a UCS Chassis Profile from a UCS Chassis Profile Template.
- Attaches the derived UCS Chassis Profile to a Cisco UCS chassis.

## Requirements

* Update Fabric Interconnect port policy with a Server port configuration to discover the UCS chassis and compute nodes
* The Cisco UCS chassis is visible in Cisco Intersight Chassis Table View
* Ansible v2.15.0 or newer
* Python 3.7 or newer (Older Python versions are no longer supported with this collection)
* The [Cisco Intersight](https://docs.ansible.com/ansible/latest/collections/cisco/intersight/index.html) Ansible collection is installed.
* Intersight user account with valid credentials and Cisco Intersight API private key and key ID.

## Role Variables

| Variable                                                            | Description                                                            | Default                         |
|---------------------------------------------------------------------|------------------------------------------------------------------------|---------------------------------|
| `derive_ucs_chassis_profile_template_api_private_key`               | Path to Cisco Intersight API private key file on the host.             |                                 |
| `derive_ucs_chassis_profile_template_api_key_id`                    | Cisco Intersight API key ID.                                           |                                 |
| `derive_ucs_chassis_profile_template_api_uri`                       | Cisco Intersight API URI.                                              | `https://intersight.com/api/v1` |
| `derive_ucs_chassis_profile_template_validate_certs`                | Validate SSL certificates (set to `false` if using self-signed certs). | 'true'                          |
| `derive_ucs_chassis_profile_template_chassis_serial_number`         | Serial number of the UCS chassis.                                      | **Required** (no default)       |
| `derive_ucs_chassis_profile_template_description`                   | UCS Chassis Profile Description                                        |                                 |
| `derive_ucs_chassis_profile_template_chassis_profile_template_name` | UCS Chassis Profile Template Name                                      | **Required** (no default)       |
| `derive_ucs_chassis_profile_template_tags`                          | Dictionary of keys and values that specifies the tags of the profile.  |                                 |

* Note: derive_ucs_chassis_profile_template_api_private_key variable definition can be declared optionally file, vault or .env based.

## Example Playbook

```yaml
    ---
    - name: Derive UCS Chassis Profile from UCS Chassis Profile Template
      hosts: localhost
      vars:
        derive_ucs_chassis_profile_template_api_private_key: "{{ 'INTERSIGHT_API_PRIVATE_KEY' }}"
        derive_ucs_chassis_profile_template_api_key_id: "{{ 'INTERSIGHT_API_KEY_ID' }}"
        derive_ucs_chassis_profile_template_chassis_serial_number: "MOCK2630D74C"
        derive_ucs_chassis_profile_template_description: "Custom Profile"
        derive_ucs_chassis_profile_template_chassis_profile_template_name: "AC08-6454-X9508-Template"
        derive_ucs_chassis_profile_template_tags:
          - Key: "configmode"
            Value: "ansible"
      roles:
        - derive_ucs_chassis_profile_template
```
        