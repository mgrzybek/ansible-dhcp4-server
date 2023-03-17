# ansible-dhcp4-server

Ansible role to provide DHCP service with UEFI HTTP Client support.

## Requirements

Any pre-requisites that may not be covered by Ansible itself or the role
should be mentioned here. For instance, if the role uses the EC2 module,
it may be a good idea to mention in this section that the boto package
is required.

## Role Variables

```yaml
# Configuration of the DHCP service
dhcp4_server_netboot_gateways: []
dhcp4_server_netboot_interfaces: []
dhcp4_server_netboot_ntp_servers: []
dhcp4_server_netboot_dns_servers: []

# First and last IP to provide
dhcp4_server_netboot_first_ip:
dhcp4_server_netboot_last_ip:

# UEFI HTTP client URLs
dhcp4_server_bootfile_ipxe_url:
dhcp4_server_bootfile_ipxe_efi_url:

# Use a notification to restart the service
dhcp4_server_post_config_restart_on_change: true

# Corosync / Pacemaker usage
dhcp4_server_cluster_aware: false
dhcp4_server_cluster_resource_name: dhcp4-server

# Package management
dhcp4_server_package_state: present
cache_timeout: 600
```

## Dependencies

N.A.

## Example Playbook

``` yaml
- hosts: servers
  roles:
  - role: ansible-dhcp4-server
  vars:
    # Configuration of the DHCP service
    dhcp4_server_netboot_network: 10.0.0.0
    dhcp4_server_netboot_netmask: 255.255.255.0
    dhcp4_server_netboot_first_ip: 10.0.0.100
    dhcp4_server_netboot_last_ip: 10.0.0.200
    dhcp4_server_netboot_interfaces:
    - net0

    # Where to find the boot artifacts
    dhcp4_server_bootfile_ipxe_efi_url: http://netboot.my-company.com/ipxe.efi
    dhcp4_server_bootfile_ipxe_url: http://netboot.my-company.com/boot.ipxe

    # Services provided by the network zone
    dhcp4_server_netboot_dns_servers:
    - 8.8.8.8
    - 4.4.4.4
    dhcp4_server_netboot_ntp_servers:
    - 10.0.0.1
    dhcp4_server_netboot_gateways:
    - 10.0.0.1
```

## Testing

This role provides a `Vagrantfile` and a `Makefile`.

``` bash
# Makefile-provided commands
$ make help
help                This help message
lint                Test YAML syntax
vagrant-destroy     Destroy vagrant boxes
vagrant-variables   Test vagrant env variables
vagrant-vbox        Test the playbook using vagrant and virtualbox
# Mandatory variables
$ export VAGRANT_BOX_NAME="my-box"
# Optionnal variables
$ export VAGRANT_BOX_URL="http://repo/my-box.box"
$ export VAGRANT_VM_CPUS=5
$ export VAGRANT_VM_MEMORY=5120
$ export VAGRANT_DATA_DISK_USAGE=true
$ VAGRANT_DATA_DISK_SIZE_MB=10240
# Vagrant using Virtualbox
$ make vagrant-vbox
$ make vagrant-destroy
```

## License

GPL-3+

## Author Information

Mathieu GRZYBEK
