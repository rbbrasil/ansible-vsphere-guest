# Ansible play to provision VMs in a vCenter infrastructure
Simple Ansible play that provision a guest VM in a VMware vCenter Infrastructure. It uses the **vsphere_guest** module, which in turn uses the Python **pyvmomi** library.

## Before running
This play depends on the following configs/packages to work properly:

### Python API "pyvmomi"
The Ansible module **vsphere_guest** makes use of the Python **pyvmomi** library to manage the VMware guests. So, you have to install this lib thru pip:
```
$ sudo {apt-get|yum|dnf} install python-pip
$ sudo pip install pyvmomi
```

### Ansible v2.3
The **vsphere_guest** module works only on version 2.3 or higher of Ansible.

## Required vars
The following vars are available to setup an new VMware vCenter guest (some are optional):
- **vcenter_datacenter**: Datacenter where the guest will be created
- **vcenter_clustername**: Cluster where the guest will be created
- **vcenter_resource_pool**: Resource Pool where the guest will be created (optional var)
- **vcenter_datastore**: Datastore where the guest will be created
- **vcenter_folder**: Path location in vCenter (optional var)
- **guest_id**: Guest OS id (https://www.vmware.com/support/developer/converter-sdk/conv61_apireference/vim.vm.GuestOsDescriptor.GuestOsIdentifier.html)
- **guest_name**: Guest name (will appear in inventory)
- **guest_disk**: Size of the guest disk (in GB) [optional var; default=30]
- **guest_memory**: Amount of guest memory (in MB) [optional var; default=1024]
- **guest_cpu**: Number of guest vCPUs [optional var; default=1]
- **net_name**: Name of the vCenter network in which the guest will be connected
- **net_ip**: Guest IP address
- **net_domain**: Your local domain name
- **net_dns_server**: Your DNS server address


## Running
This play runs locally, so you must have an *localhost* entry in your inventory file.

```
$ git clone https://github.com/rbbrasil/ansible-vsphere-guest
$ sudo echo "localhost" >> /etc/ansible/hosts
$ ansible-playbook --ask-vault-pass -e "\
> vcenter_datacenter=DC_REGION01 \
> vcenter_clustername=CLUSTER_PRD \
> vcenter_resource_pool=WEBSERVER \
> vcenter_datastore=SSD01 \
> vcenter_folder='/GUESTS/PRD/WEBSERVER/NGINX/' \
> guest_id=rhel7Guest \
> guest_name=ANSIBLE-TESTVM01 \
> guest_disk=35 \
> guest_memory=2048 \
> guest_cpu=2 \
> net_domain=your.domain.name \
> net_dns_server=10.1.2.3 \
> net_name=PRD \
> net_ip=10.2.3.4" ansible-vsphere-guest/main.yml
```
