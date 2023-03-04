# Role for linux setup

[![Ansible CI](https://github.com/unleftie/ansible-role-linux/actions/workflows/ansible-ci.yml/badge.svg)](https://github.com/unleftie/ansible-role-linux/actions/workflows/ansible-ci.yml)
[![Checkmarx KICS](https://github.com/unleftie/ansible-role-linux/actions/workflows/checkmarx-kics.yml/badge.svg)](https://github.com/unleftie/ansible-role-linux/actions/workflows/checkmarx-kics.yml)

## Compatibility

| Platform   | Version |
| ---------- | ------- |
| debian     | 11      |
| el (rocky) | 9       |
| ubuntu     | 22.04   |

## Dependencies

- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) (v2.14+)
- [Molecule](https://molecule.readthedocs.io/en/latest/installation.html) (v4.0.4+) (for local testing)
- [Docker](https://docs.docker.com/get-docker/) (for local testing)

## Local Testing

```sh
molecule test
```

## Installation

> Upgradability notice: When upgrading from old version of this role, be aware that some files may be lost.

To deploy the Linux on hosts, add the Datadog role to your playbook. Below are some sample playbooks to assist you with using the Linux Ansible role.

```yml
- name: Sample 1
  hosts: all
  become: true
  environment:
    PATH: "{{ ansible_env.PATH }}:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin"
  vars:
    vault_root_password: "teSt_passw0rd"
    vault_user_initial_password: "teSt_passw0rd"
  pre_tasks:
    - name: Ensure apt cache are updated
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"
  tasks:
    - include_role:
        name: "linux"
```

## Variables

| Variable                       | Description                                                                          | Value      |
| ------------------------------ | ------------------------------------------------------------------------------------ | ---------- |
| `service_name`                 | Service/organization name tag                                                        |            |
| `packages_upgrade_all`         | Whether to upgrade all packages                                                      | true/false |
| `packages_extra`               | Whether to install packages from extra list                                          | true/false |
| `packages_install_jdk`         | Whether to install JDK packages from default list (see `packages_jdk_list`)          | true/false |
| `packages_install_jdk_extra`   | Whether to install JDK packages from extra list (see `packages_jdk_extra_list`)      | true/false |
| `packages_autoupdate_enabled`  | Whether to enable automatic updates for system packages                              | true/false |
| `system_hardening`             | Whether to provide numerous security-related configurations                          | true/false |
| `system_log_all_commands`      | Whether to log all program executions (see `tasks/snoopy.yml`)                       | true/false |
| `system_configure`             | Whether to provide numerous system-related configurations                            | true/false |
| `system_locale`                | System locale value                                                                  |
| `user_configure`               | Whether to provide numerous user-related configurations                              | true/false |
| `user_root_update_password`    | Whether to update root password on every run                                         | true/false |
| `user_root_password`           | (`sensitive`) Root password value                                                    |
| `user_names_list`              | List of system user accounts to be added                                             |
| `user_initial_password`        | (`sensitive`) Initial password value for users from `user_names_list`                |
| `user_group_name`              | Group name for user accounts from `user_names_list`                                  |
| `user_group_ssh_password_auth` | Whether to provide SSH password auth for users from `user_names_list`                | true/false |
| `ssh_configure`                | Whether to provide numerous ssh-related configurations                               | true/false |
| `ssh_permit_root_login`        | Specifies whether root user can log in using SSH                                     |
| `swap_configure`               | Whether to provide numerous swap-related configurations                              | true/false |
| `swap_size`                    | Swap file size                                                                       | 0-x (MiB)  |
| `swap_file_path`               | Swap file path                                                                       |
| `swap_swappiness_value`        | Rate at which kernel moves pages into and out of active RAM                          | 0-100 (%)  |
| `auditd_configure`             | Whether to provide numerous auditd-related configurations                            | true/false |
| `firewall_configure`           | Whether to provide numerous firewall-related configurations                          | true/false |
| `firewall_reset_rules`         | Whether to reset iptables rules list on every run                                    | true/false |
| `firewall_input_policy_drop`   | Whether to switch iptables INPUT policy to DROP                                      | true/false |
| `firewall_forward_policy_drop` | Whether to switch iptables FORWARD policy to DROP                                    | true/false |
| `packages_unnecessary_list`    | List of packages to be removed                                                       |
| `os_security_auto_logout`      | Timeout for logout users automatically after time. Set to `0` to disable the timeout | 0-x (sec)  |
| `os_unused_filesystems`        | List of filesystems to be disabled                                                   |
| `os_filesystem_whitelist`      | OS-related overwrite for `os_unused_filesystems`                                     |
| `sysctl_config`                | List of various sysctl-settings                                                      |
| `sysctl_custom_config`         | OS-related overwrite for `system_configure`                                          |
| `sysctl_overwrite`             | Global overwrite for `system_configure`                                              |
| `os_shadow_perms`              | OS-related permissions for /etc/shadow                                               |
| `os_passwd_perms`              | OS-related permissions for /etc/passwd                                               |

## Repositories

| #                   | Default repository                                          |
| ------------------- | ----------------------------------------------------------- |
| snoopy_rpm_repo_url | https://a2o.github.io/snoopy-packages/repo/centos/8/stable/ |
