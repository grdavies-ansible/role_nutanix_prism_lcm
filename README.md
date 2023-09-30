# Nutanix Role for Prism LCM Software & Firmware Updates

This Ansible role invokes LCM to perform an inventory operation followed by software and firmware updates if necessary.

## Role Variables

| Variable                                    | Required | Default | Choices                                                                                | Comments                                                                                                                                                                                                     |
|---------------------------------------------|----------|---------|----------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| role_nutanix_prism_lcm_host                 | yes      |         |                                                                                        | The IP address or FQDN for the Prism (Element or Central) to which you want to connect.                                                                                                                      |
| role_nutanix_prism_lcm_host_username        | yes      |         |                                                                                        | A valid username with appropriate rights to access the Nutanix API.                                                                                                                                          |
| role_nutanix_prism_lcm_host_password        | yes      |         |                                                                                        | A valid password for the supplied username.                                                                                                                                                                  |
| role_nutanix_prism_lcm_host_port            | no       | 9440    |                                                                                        | The Prism TCP port.                                                                                                                                                                                          |
| role_nutanix_prism_lcm_host_validate_certs  | no       | no      | yes / no                                                                               | Whether to check if Prism UI certificates are valid.                                                                                                                                                         |
| role_nutanix_prism_lcm_run_inventory        | no       | true    | true or false                                                                          | Whether to run an inventory prior to installing updates.                                                                                                                                                     |
| role_nutanix_prism_lcm_run_software_updates | no       | true    | true or false                                                                          | Whether to install software updates.                                                                                                                                                                         |
| role_nutanix_prism_lcm_sw_to_update         | no       | []      | ["NCC", "Cluster Maintenance Utilities", "Foundation", "AHVhypervisor", "AOS", "FSM"]  | If not defined then all available software updates will be installed. If one or more software choices are provided then only they will be updated, if not choices are provided all software will be updated. |
| role_nutanix_prism_lcm_run_firmware_updates | no       | true    | true or false                                                                          | Whether to install firmware updates.                                                                                                                                                                         |
| role_nutanix_prism_lcm_run_precheck         | no       | true    | true or false                                                                          | Whether to run a LCM precheck prior to installing updates.                                                                                                                                                   |
| role_nutanix_prism_lcm_task_retries         | no       | 5       |                                                                                        | Number of progress checks                                                                                                                                                                                    |
| role_nutanix_prism_lcm_delay                | no       | 300     |                                                                                        | Progress check interval                                                                                                                                                                                      |

## Dependencies

- grdavies.nutanix_role_prism_init_api
- grdavies.role_nutanix_prism_monitor_task

## Example Playbooks

This example playbook will invoke LCM on a specific cluster running only a software upgrade for "NCC"

```
- hosts: localhost
 roles:
   - role: grdavies.nutanix-role-prism-lcm
 vars:
   role_nutanix_prism_lcm_host: 10.38.185.37
   role_nutanix_prism_lcm_host_username: admin
   role_nutanix_prism_lcm_host_password: nx2Tech165!
   role_nutanix_prism_lcm_run_firmware_updates: false
   role_nutanix_prism_lcm_sw_to_update:
     - NCC
```

## License


See LICENSE.md

## Author Information

Ross Davies
