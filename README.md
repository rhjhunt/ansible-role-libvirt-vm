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
libvirt_vm_ip | | | Public IP of the libvirt VM |
libvirt_vm_hostname | :heavy_check_mark: | | The FQDN of the VM |
libvirt_vm_root_pwd | :heavy_check_mark: | | root user password |
libvirt_vm_base_img | :heavy_check_mark: | | Name of the base image |
libvirt_vm_storage_pool | :heavy_check_mark: | `default` | libvirt storage pool |
libvirt_vm_network | :heavy_check_mark: | | Network type for libvirt vm |
libvirt_vm_vcpus | :heavy_check_mark: | ```1``` | Number of vCPUS for VM |
libvirt_vm_ram | :heavy_check_mark: | ```1024``` | Amount of RAM in megabytes |
libvirt_vm_os_image_name | :heavy_check_mark: | ```{{ libvirt_vm_hostname }}``` | Name of the VM image |
libvirt_vm_os_image_size | :heavy_check_mark: | ```10G``` | Size of OS image of the VM |
libvirt_vm_os_variant | :heavy_check_mark: | ```rhel8.1``` | libvirt os-variant |
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
        name: rhjhunt.libvirt-vm
```

License
-------

[GPLv3](LICENSE)

Author Information
------------------

Jacob Hunt <jhunt@redhat.com>
