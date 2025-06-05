# Upgrade UCS Fabric Interconnect Firmware

## Overview
A role that runs **Cisco UCS Fabric Interconnects (FIs)** firmware upgrade.

⚠️ **Warning:** : By running this role you are going to run a full Firmware upgrade on both your FIs. ⚠️

## Features

- Automates Firmware upgrades.
- Supports upgrading different distributables - latest, recommended or specific version.

## Requirements

* Ansible v2.15.0 or newer
* Python 3.7 or newer (Older Python versions are no longer supported with this collection)
* `cisco.intersight` collection installed (`ansible-galaxy collection install cisco.intersight`).
* Intersight user account with valid permissions

## Role Variables


| Variable                                        | Description                                                                                                                                                                                                                                                                                                                                                                                                               | Default                         |
|-------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|
| `upgrade_ucs_fi_firmware_api_private_key`       | Path to Cisco Intersight API private key file on the host.                                                                                                                                                                                                                                                                                                                                                                |                                 |
| `upgrade_ucs_fi_firmware_api_key_id`            | Cisco Intersight API key ID.                                                                                                                                                                                                                                                                                                                                                                                              |                                 |
| `upgrade_ucs_fi_firmware_api_uri`               | Cisco Intersight API URI.                                                                                                                                                                                                                                                                                                                                                                                                 | `https://intersight.com/api/v1` |
| `upgrade_ucs_fi_firmware_validate_certs`        | Validate SSL certificates (set to `false` if using self-signed certs).                                                                                                                                                                                                                                                                                                                                                    | 'true'                          |
| `upgrade_ucs_fi_firmware_distributable_type`    | Firmware distributable type that will be used for upgrading the FI firmware.<br/>Chosen from:<br/>1) latest: Use the most recent firmware version available. <br/> 2) recommended: Use the firmware version that is recommended by the HCL.<br/>3) specific_version: Use a specific firmware version as defined by the user. In this case please set the version number in upgrade_fi_firmware_distributable_version var. | recommended                     |
| `upgrade_ucs_fi_firmware_distributable_version` | If upgrade_ucs_fi_firmware_distributable_type selected as specific_version then please fill this variable with the desired firmware version to be upgraded on the FIs.                                                                                                                                                                                                                                                    |                                 |
| `upgrade_ucs_fi_firmware_fi_list`               | List of Fabric Interconnects to be firmware upgraded.                                                                                                                                                                                                                                                                                                                                                                     | **Required** (no default)       |

* Note: upgrade_ucs_fi_firmware_api_private_key variable definition can be declared optionally file, vault or .env based.

## Example Playbook

```yaml
    ---
    - name: FI firmware upgrade
      hosts: localhost
      gather_facts: no
      vars: 
        upgrade_ucs_fi_firmware_api_private_key:  "{{ INTERSIGHT_API_PRIVATE_KEY }}"
        upgrade_ucs_fi_firmware_api_key_id: "{{ INTERSIGHT_API_KEY_ID }}"
        upgrade_ucs_fi_firmware_distributable_type: "latest"
        upgrade_ucs_fi_firmware_fi_list: ["AC08-6454 FI-A", "AC08-6454 FI-B"]
      tasks:
        - name: Include the upgrade_ucs_fi_firmware role
          ansible.builtin.include_role:
            name: upgrade_ucs_fi_firmware

```