Create RHEL template and VM's for KVM + libvirt
=========

I use these ansible roles to quickly spin up RHEL VM's on my laptop. These roles create a VM template and then allow you to use the template to create additional VM's.

Pre-Reqs
------------

* Download the latest RHEL7 KVM qcow2 image from the Red Hat portal.

* Setup your laptop to act as a KVM host - yum install libvirt-client virt-manager libguestfs-tools-c python-lxml

* Create an Activation key in the Red Hat subscription Portal with relevant subs attached. Make a note of the activation key name and the org ID.

* An SSH key is injected into the VM as part of creation. I just create a SSH keypair as root on my laptop and run it as root but feel free to do what you want. The ssh key location is set in the role so you'd need to change it there.


Variables
--------------

source_image: < the location of the RHEL qcow image that you downloaded >
template_name: < name of RHEL template >

There are a few vars which you should manage with vault as well. Create an ansible vault file:

# ansible-vault create group_vars/all/vault

Set these vars:

vault_root_password: < root password that will be injected into your VM template >
vault_rhsm_org: < RHSM org ID for activation key >
vault_rhsm_key: < RHSM activation key name >

These vars can be set as well to modify CPU and RAM allocated to VM's:

vcpu: < defaults to 1 vcpu >
memory: < memory in GB. Defaults to 1 >

ansible_host: < sets a static IP for each VM. Example provided in example_inventory >


Running Playbooks
----------------

You need to use --ask-vault-pass when running playbooks.

create_template.yml can be run as is to create the template. 

example_create_vm.yml provides an example in conjunction with example_inventory on how you can create a VM from the template. 
