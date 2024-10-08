# Ansible Role for linux setup

[![CI](https://github.com/unleftie/ansible-role-linux/actions/workflows/ci.yml/badge.svg)](https://github.com/unleftie/ansible-role-linux/actions/workflows/ci.yml)
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/unleftie/ansible-role-zsh/badge)](https://securityscorecards.dev/viewer/?uri=github.com/unleftie/ansible-role-linux)

## Compatibility

| Platform | Version |
| -------- | ------- |
| debian   | 12      |

## Dependencies

- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) (v2.14+)
- [Molecule](https://molecule.readthedocs.io/en/latest/installation.html) + (v4.0.4+) + [docker plugin](https://github.com/ansible-community/molecule-plugins) (for local testing)
- [Docker](https://docs.docker.com/get-docker/) (for local testing)

## Local Testing

```sh
git clone https://github.com/unleftie/ansible-role-linux.git
cd ansible-role-linux/
molecule test
```

## Installation

> Upgradability notice: When upgrading from old version of this role, be aware that some files may be lost.

```yml
- name: Sample 1
  hosts: all
  become: true
  tasks:
    - include_role:
        name: "ansible-role-linux"
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
