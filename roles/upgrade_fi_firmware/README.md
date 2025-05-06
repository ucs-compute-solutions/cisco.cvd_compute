upgrade_fi_firmware
=========

A role that runs **Cisco UCS Fabric Interconnects (FIs)** firmware upgrade.

Requirements
------------

* Ansible v2.15.0 or newer
* Python 3.7 or newer (Older Python versions are no longer supported with this collection)
* `cisco.intersight` collection installed (`ansible-galaxy collection install cisco.intersight`).
* Intersight user account with valid permissions

Role Variables
--------------

| Variable                                    | Description                                                                                                                                                                                                                                                                                                                                                                                                             | Default                         |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|
| `upgrade_fi_firmware_api_private_key`       | Path to Cisco Intersight API private key file on the host.                                                                                                                                                                                                                                                                                                                                                              | **Required** (no default)       |
| `upgrade_fi_firmware_api_key_id`            | Cisco Intersight API key ID.                                                                                                                                                                                                                                                                                                                                                                                            | **Required** (no default)       |
| `upgrade_fi_firmware_api_uri`               | Cisco Intersight API URI.                                                                                                                                                                                                                                                                                                                                                                                               | `https://intersight.com/api/v1` |
| `upgrade_fi_firmware_validate_certs`        | Validate SSL certificates (set to `false` if using self-signed certs).                                                                                                                                                                                                                                                                                                                                                  | 'true'                          |
| `upgrade_fi_firmware_distributable_type`    | Firmware distributable type that will be used for upgrading the FI firmware.<br/>Chosen from:<br/>1) latest: Use the most recent firmware version available.<br/>2) recommended: Use the firmware version that is recommended by the HCL.<br/>3) specific_version: Use a specific firmware version as defined by the user. In this case please set the version number in upgrade_fi_firmware_distributable_version var. | recommended                     |
| `upgrade_fi_firmware_distributable_version` | If upgrade_fi_firmware_distributable_type selected as specific_version then please fill this variable with the desired firmware version to be upgraded on the FIs.                                                                                                                                                                                                                                                      | N/A                             |
| `upgrade_fi_firmware_fi_list`               | List of Fabric Interconnects to be firmware upgraded.                                                                                                                                                                                                                                                                                                                                                                   | **Required** (no default)       |

* Note: upgrade_fi_firmware_api_private_key is stored on a file on the host, we expect a path to this file.

Example Playbook
----------------

    ---
    - name: FI firmware upgrade
      hosts: localhost
      gather_facts: no
      vars: 
        upgrade_fi_firmware_api_private_key: "{{ lookup('env', 'INTERSIGHT_API_PRIVATE_KEY') }}"
        upgrade_fi_firmware_api_key_id: "{{ lookup('env', 'INTERSIGHT_API_KEY_ID') }}"
        upgrade_fi_firmware_distributable_type: "latest"
        upgrade_fi_firmware_fi_list: ["Domain FI-A", "Domain FI-B"]
      tasks:
        - name: Include the upgrade_fi_firmware role
          ansible.builtin.include_role:
            name: upgrade_fi_firmware
        