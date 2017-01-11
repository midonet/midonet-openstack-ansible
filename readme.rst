Midonet for OpenStack-Ansible
#########################################
:date: 2017-01-11
:tags: openstack, ansible
:category: \*openstack, \*nix
:OS: Ubuntu 16.04
:Midonet: 5.2.1


About this repository
---------------------

This set of playbooks will deploy Midokura Midonet services in LXC containers to get high availability in OpenStack Neutron.

Process
-------

Clone this repo

.. code-block:: bash

    cd /opt
    git clone -b <TAG> https://github.com/fitbeard/openstack-ansible-midonet

Copy the env.d and conf.d files into place

.. code-block:: bash

    cd openstack-ansible-midonet
    cp etc/env.d/* /etc/openstack_deploy/env.d/
    cp etc/conf.d/* /etc/openstack_deploy/conf.d/

Edit ``/etc/openstack_deploy/conf.d/{{cassandra}}{{zookeeper}}{{midonet-api}}.yml`` config depending on environment.

Copy the Midonet Neutron plugin template into place

.. code-block: bash
    cp -r templates/plugins/midonet /etc/ansible/roles/os_neutron/templates/plugins/

Overwrite neutron.conf with modified version. See diff for changes.

.. code-block: bash
    cp templates/neutron.conf.j2 /etc/ansible/roles/os_neutron/templates/

Add the export to update the inventory file location

.. code-block:: bash

    export ANSIBLE_INVENTORY=/opt/openstack-ansible/playbooks/inventory/dynamic_inventory.py

WORK IN PROGRESS