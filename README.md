[![Build Status](https://travis-ci.com/rhjhunt/ansible-role-libvirt-vm.svg?branch=master)](https://travis-ci.com/rhjhunt/ansible-role-libvirt-vm) ![Ansible Role](https://img.shields.io/ansible/role/46390) ![Ansible Quality Score](https://img.shields.io/ansible/quality/46390) ![GitHub](https://img.shields.io/github/license/rhjhunt/ansible-role-libvirt-vm) ![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/rhjhunt/ansible-role-libvirt-vm)



libvirt-vm
=============

Ansible role to install libvirt virtual machine on a RHEL/CentOS kvm hypervisor.

Requirements
------------

- Ansible 2.9 or higher
- Red Hat Enterprise Linux (RHEL) or CentOS 7 or 8

Role Variables
--------------

The following variables are supported for this role.

Role Variable | Required | Default | Description
--------------|----------|---------|------------
libvirt_vm_ip | :x: | | Public IP of the libvirt VM |
libvirt_vm_hostname | :heavy_check_mark: | | The FQDN of the VM |
libvirt_vm_root_pwd | :heavy_check_mark: | | root user password |
libvirt_vm_base_img | :heavy_check_mark: | | Name of the base image |
libvirt_vm_storage_pool | :x: | `default` | libvirt storage pool |
libvirt_vm_network | :heavy_check_mark: | | Network type for libvirt vm |
libvirt_vm_vcpus | :x: | ```1``` | Number of vCPUS for VM |
libvirt_vm_ram | :x: | ```1024``` | Amount of RAM in megabytes |
libvirt_vm_os_image_name | :x: | ```{{ libvirt_vm_hostname }}``` | Name of the VM image |
libvirt_vm_os_image_size | :x: | ```10G``` | Size of OS image of the VM |
libvirt_vm_os_variant | :x: | ```rhel8.1``` | libvirt os-variant |
libvirt_vm_nics | :heavy_check_mark: | ```see example playbook``` | Dictionary to define the VM NIC |

Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: hypervisor
  tags: provision
  vars:
    libvirt_vm_hostname: "vm.example.com"
    libvirt_vm_root_pwd: "Pa$$w0rD!"
    libvirt_vm_base_img: rhel-guest-image-8.qcow2
    libvirt_vm_storage_pool: "default"
    libvirt_vm_network: "bridge=br0,model=virtio"
    libvirt_vm_vcpus: "2"
    libvirt_vm_ram: "4096"
    libvirt_vm_os_image_name: "{{ libvirt_vm_hostname }}"
    libvirt_vm_os_image_size: "20G"
    libvirt_vm_os_variant: "rhel8.1"
    libvirt_vm_nics:
      - name: eth0
        bootproto: static
        onboot: yes
        ip: "{{ libvirt_vm_ip }}"
        prefix: "24"
        gateway: "192.168.122.1"
        dns_server: "192.168.122.1"

  tasks:
    - name: Create a libvirt VM
      include_role:
        name: rhjhunt.libvirt_vm
```

The libvirt VM can also be assigned a DHCP address.

```yaml
- hosts: hypervisor
  tags: provision
  vars:
    libvirt_vm_hostname: "vm.example.com"
    libvirt_vm_root_pwd: "Pa$$w0rD!"
    libvirt_vm_base_img: rhel-guest-image-8.qcow2
    libvirt_vm_storage_pool: "default"
    libvirt_vm_network: "bridge=br0,model=virtio"
    libvirt_vm_vcpus: "2"
    libvirt_vm_ram: "4096"
    libvirt_vm_os_image_name: "{{ libvirt_vm_hostname }}"
    libvirt_vm_os_image_size: "20G"
    libvirt_vm_os_variant: "rhel8.1"
    libvirt_vm_nics:
      - name: eth0
        bootproto: dhcp
        onboot: yes

  tasks:
    - name: Create a libvirt VM
      include_role:
        name: rhjhunt.libvirt_vm
```

License
-------

[GPLv3](LICENSE)

Author Information
------------------

Jacob Hunt <jhunt@redhat.com>
