Midonet for OpenStack-Ansible
#########################################
:date: 2017-01-08
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

    cd openstack-ansible-scaleio-midonet
    cp etc/env.d/cassandra.yml /etc/openstack_deploy/env.d/
    cp etc/env.d/zookeeper.yml /etc/openstack_deploy/env.d/
    cp etc/env.d/midonet-api.yml /etc/openstack_deploy/env.d/

    cp etc/conf.d/cassandra.yml /etc/openstack_deploy/conf.d/
    cp etc/conf.d/zookeeper.yml /etc/openstack_deploy/conf.d/
    cp etc/conf.d/midonet-api.yml /etc/openstack_deploy/conf.d/

Edit ``/etc/openstack_deploy/conf.d/cassandra*zookeeper*midonet-api.yml`` config depending on environment.

Add the export to update the inventory file location

.. code-block:: bash

    export ANSIBLE_INVENTORY=/opt/openstack-ansible/playbooks/inventory/dynamic_inventory.py
