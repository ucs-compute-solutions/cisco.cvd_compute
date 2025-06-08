# Cisco UCS chassis rediscover role

## Overview

Rediscover rebuilds the chassis inventory, including the connections between the Fabric Interconnect and the IOMs/IFMs of the chassis, without impacting the service of the blade servers. The operation also performs a quick discovery for inventory update on the blade servers.

The **rediscover_ucs_chassis** role is part of the **Cisco UCS Validated Content Collection**. It uses the **Cisco Intersight REST API** to query and rediscover
any given Cisco UCS chassis available within the Intersight account.

## Features

- Queries and rediscover a specified UCS chassis via Intersight.

## Requirements

* Ansible v2.15.0 or newer
* Python 3.7 or newer (Older Python versions are no longer supported with this collection)
* The [Cisco Intersight](https://docs.ansible.com/ansible/latest/collections/cisco/intersight/index.html) Ansible collection is installed.
* Intersight user account with valid credentials and Cisco Intersight API private key and key ID.

## Role Variables

| Variable                                    | Description                                                            | Default                         |
|---------------------------------------------|------------------------------------------------------------------------|---------------------------------|
| `rediscover_ucs_chassis_api_private_key`    | Path to Cisco Intersight API private key file or environment variable  |                                 |
| `rediscover_ucs_chassis_api_key_id`         | Cisco Intersight API key ID                                            |                                 |
| `rediscover_ucs_chassis_api_uri`            | Cisco Intersight API URI                                               | `https://intersight.com/api/v1` |
| `rediscover_ucs_chassis_validate_certs`     | Validate SSL certificates (set to `false` if using self-signed certs)  | 'true'                          |
| `rediscover_ucs_chassis_chassis_name_list`  | Comma separated list of chassis name(s) to rediscover                  | **Required** (no default)       |

Notes:
* rediscover_ucs_chassis_api_private_key variable definition can be declared optionally file, vault or .env based.

## Example Playbook

```yaml
    ---
    - name: UCS Chassis Rediscovery in Intersight
      hosts: localhost
      vars:
        rediscover_ucs_chassis_api_private_key: "{{ 'INTERSIGHT_API_PRIVATE_KEY' }}"
        rediscover_ucs_chassis_api_key_id: "{{ 'INTERSIGHT_API_KEY_ID' }}"
        rediscover_ucs_chassis_chassis_name_list: ['AC08-6454-1']
      roles:
        - rediscover_ucs_chassis
```
