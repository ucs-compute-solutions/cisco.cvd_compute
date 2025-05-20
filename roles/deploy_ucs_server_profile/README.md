# Cisco UCS deploying a server profile

## Overview

A role that deploys **Cisco UCS Server profile** in Intersight.

## Features

* Deploys a server profile in Intersight.
* User can choose to deploy server with or without reboot.
* User can deploy multiple server profiles.

## Requirements

* Ansible v2.15.0 or newer
* Python 3.7 or newer (Older Python versions are no longer supported with this collection)
* The [Cisco Intersight](https://docs.ansible.com/ansible/latest/collections/cisco/intersight/index.html) Ansible collection is installed.
* Intersight user account with valid credentials and Cisco Intersight API private key and key ID.

## Role Variables

| Variable                                             | Description                                                            | Default                         |
|------------------------------------------------------|------------------------------------------------------------------------|---------------------------------|
| `deploy_ucs_server_profile_api_private_key`          | Path to Cisco Intersight API private key file on the host.             | **Required** (no default)       |
| `deploy_ucs_server_profile_api_key_id`               | Cisco Intersight API key ID.                                           | **Required** (no default)       |
| `deploy_ucs_server_profile_api_uri`                  | Cisco Intersight API URI.                                              | `https://intersight.com/api/v1` |
| `deploy_ucs_server_profile_validate_certs`           | Validate SSL certificates (set to `false` if using self-signed certs). | 'true'                          |
| `deploy_ucs_server_profile_server_profiles_list`     | List of server profiles for deployment.                                | **Required** (no default)       |

* Note: deploy_ucs_server_profile_api_private_key is stored on a file on the host, we expect a path to this file.

## Example Playbook
```yaml
    ---
    - name: Test Cisco UCS deploy server profile in Intersight
      hosts: localhost
      vars:
        deploy_ucs_server_profile_api_private_key: "{{ lookup('env', 'INTERSIGHT_API_PRIVATE_KEY') }}"
        deploy_ucs_server_profile_api_key_id: "{{ lookup('env', 'INTERSIGHT_API_KEY_ID') }}"
        deploy_ucs_server_profile_server_profiles_list: ['worker0', 'worker1', 'worker2', 'control0', 'control1', 'control2']
      roles:
        - deploy_ucs_server_profile
```