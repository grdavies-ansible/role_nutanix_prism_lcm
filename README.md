# Nutanix Role for Prism LCM Software & Firmware Updates

This Ansible role invokes LCM to perform an inventory operation followed by software and firmware updates if necessary.

## Role Variables

| Variable                                    | Required | Default | Choices                                                                                | Comments                                                                                                                                                                                                     |
|---------------------------------------------|----------|---------|----------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| role_nutanix_prism_lcm_host                 | yes      |         |                                                                                        | The IP address or FQDN for the Prism (Element or Central) to which you want to connect.                                                                                                                      |
| role_nutanix_prism_lcm_host_username        | yes      |         |                                                                                        | A valid username with appropriate rights to access the Nutanix API.                                                                                                                                          |
| role_nutanix_prism_lcm_host_password        | yes      |         |                                                                                        | A valid password for the supplied username.                                                                                                                                                                  |
| role_nutanix_prism_lcm_host_port            | no       | 9440    |                                                                                        | The Prism TCP port.                                                                                                                                                                                          |
| role_nutanix_prism_lcm_host_validate_certs  | no       | false   | true / false                                                                           | Whether to check if Prism UI certificates are valid.                                                                                                                                                         |
| role_nutanix_prism_lcm_debug                | no       | false   | true / false                                                                           | Enable debug logging.                                                                                                                                                                                        |
| role_nutanix_prism_lcm_run_inventory        | no       | true    | true / false                                                                           | Whether to run an inventory prior to installing updates.                                                                                                                                                     |
| role_nutanix_prism_lcm_run_software_updates | no       | true    | true / false                                                                           | Whether to install software updates.                                                                                                                                                                         |
| role_nutanix_prism_lcm_run_firmware_updates | no       | true    | true / false                                                                           | Whether to install firmware updates.                                                                                                                                                                         |
| role_nutanix_prism_lcm_run_precheck         | no       | true    | true / false                                                                           | Whether to run a LCM precheck prior to installing updates.                                                                                                                                                   |
| role_nutanix_prism_lcm_task_retries         | no       | 5       |                                                                                        | Number of progress checks                                                                                                                                                                                    |
| role_nutanix_prism_lcm_delay                | no       | 300     |                                                                                        | Progress check interval                                                                                                                                                                                      |
| role_nutanix_prism_lcm_sw_to_update         | no       | []      | See dictionary format int able below.                                                  | If not defined then all available software updates will be installed. If one or more software choices are provided then only they will be updated, if not choices are provided all software will be updated. |

### role_nutanix_prism_lcm_sw_to_update dictionary format

| Variable                                    | Required | Default | Choices                                                                                | Comments                                                                                                                                                                                                     |
|---------------------------------------------|----------|---------|----------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| entity                                      | yes      |         | See 'Prism Element Entity Names' and 'Prism Central/NCM Entity Names' below            |                                                                                                                                                                                                              |
| version                                     | no       | latest  |                                                                                        | Is blank version will default to the latest version available to LCM                                                                                                                                         |
| service_name                                | no       | None    |                                                                                        | This is required to apply updates to cluster managed entities such as Object Clusters or Nutanix Kubernetes Engine Clusters                                                                                  |

#### Prism Element Entity Names

| Entity Name                   | Notes                                                             |
|-------------------------------|-------------------------------------------------------------------|
| AOS                           |                                                                   |
| AHV hypervisor                |                                                                   |
| NCC                           |                                                                   |
| FSM                           |                                                                   |
| Cluster Maintenance Utilities |                                                                   |
| Foundation                    |                                                                   |

#### Prism Central/NCM Entity Names

| Entity Name                   | Service Name Required | Notes                                                                           |
|-------------------------------|-----------------------|---------------------------------------------------------------------------------|
| PC                            |                       |                                                                                 |
| AOS                           |                       |                                                                                 |
| Cluster Maintenance Utilities |                       |                                                                                 |
| Epsilon                       |                       |                                                                                 |
| Calm                          |                       |                                                                                 |
| Files Manager                 |                       |                                                                                 |
| Flow Network Security PC      |                       |                                                                                 |
| Foundation Central            |                       |                                                                                 |
| Licensing                     |                       |                                                                                 |
| MSP                           |                       |                                                                                 |
| NCC                           |                       |                                                                                 |
| Nutanix Kubernetes Engine     |                       |                                                                                 |
| Objects Manager               |                       |                                                                                 |
| Objects Service               | Yes                   | Service name should be equal to each Objects Cluster that you wish to upgrade.  |
| PC Core Services              |                       |                                                                                 |
| PC                            |                       |                                                                                 |

## Dependencies

- grdavies.nutanix_role_prism_init_api
- grdavies.role_nutanix_prism_monitor_task

## Example Playbooks

This example playbook will invoke LCM on a cluster running at 10.38.185.37 and update all software and firmware

```YAML
- hosts: localhost
 roles:
    - role: grdavies.nutanix-role-prism-lcm
 vars:
    role_nutanix_prism_lcm_host: 10.38.185.37
    role_nutanix_prism_lcm_host_username: admin
    role_nutanix_prism_lcm_host_password: nx2Tech165!
    role_nutanix_prism_lcm_run_firmware_updates: true
    role_nutanix_prism_lcm_run_software_updates: true
```

This example playbook will invoke LCM on a NCI cluster running at 10.38.185.37 to update "NCC" only

```YAML
- hosts: localhost
 roles:
    - role: grdavies.nutanix-role-prism-lcm
 vars:
    role_nutanix_prism_lcm_host: 10.38.185.37
    role_nutanix_prism_lcm_host_username: admin
    role_nutanix_prism_lcm_host_password: nx2Tech165!
    role_nutanix_prism_lcm_run_firmware_updates: false
    role_nutanix_prism_lcm_sw_to_update:
      - entity: "NCC"
```

This example playbook will invoke LCM on Prism Central running only a software upgrade to version 3.5.0.1 for the "Objects Manager" service and a single Objects cluster named "ntnx-objects"

```YAML
- hosts: localhost
 roles:
    - role: grdavies.nutanix-role-prism-lcm
 vars:
    role_nutanix_prism_lcm_host: 10.38.185.39
    role_nutanix_prism_lcm_host_username: admin
    role_nutanix_prism_lcm_host_password: nx2Tech165!
    role_nutanix_prism_lcm_run_firmware_updates: false
    role_nutanix_prism_lcm_sw_to_update:
    role_nutanix_prism_lcm_sw_to_update:
      - entity: "Objects Manager"
        version: "3.5.0.1"
      - entity: "Objects Service"
        version: "3.5.0.1"
        service_name: "ntnx-objects"
```

## License

See LICENSE.md

## Author Information

Ross Davies
