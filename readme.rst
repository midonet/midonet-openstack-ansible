Midonet for OpenStack-Ansible
#########################################
:date: 2017-01-25
:tags: openstack, ansible
:category: \*openstack, \*nix
:OS: Ubuntu 16.04
:Midonet: 5.2.1


About this repository
---------------------

This set of playbooks will deploy Midonet services in LXC containers to get high availability in OpenStack Neutron.

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

Edit ``/etc/openstack_deploy/conf.d/{{cassandra}}{{zookeeper}}{{midonet-cluster}}.yml`` config depending on environment.

Copy Midonet variables config into place and edit depending on environment

.. code-block:: bash

    cp etc/user_midonet_variables.yml /etc/openstack_deploy/

Unset any neutron_* variable from any other config except user_midonet_variables.yml

Add the export to update the inventory file location

.. code-block:: bash

    export ANSIBLE_INVENTORY=/opt/openstack-ansible/playbooks/inventory/dynamic_inventory.py

If you are running the HA Proxy you should run the following playbook as well to enable the Midonet Cluster API port 9981

.. code-block:: bash

    openstack-ansible setup-haproxy.yml

Create the containers

.. code-block:: bash

    openstack-ansible /opt/openstack-ansible/playbooks/lxc-containers-create.yml -e container_group=cassandra
    openstack-ansible /opt/openstack-ansible/playbooks/lxc-containers-create.yml -e container_group=zookeeper
    openstack-ansible /opt/openstack-ansible/playbooks/lxc-containers-create.yml -e container_group=midonet-cluster
    
Download all required roles for this installation

.. code-block:: bash

    openstack-ansible setup-roles.yml

Install Cassandra and Zookeeper clusters

.. code-block:: bash

    openstack-ansible setup-nsdb.yml

Install Midonet Cluster and Midolman agents

.. code-block:: bash

    openstack-ansible setup-midonet.yml

Patch Neutron server instances with Midonet code

.. code-block:: bash

    openstack-ansible playbook-midonet-neutron.yml


?????

Patch Neutron DB, Restart compute servers (libvirt/qemu.conf modification and libvirt locks) to apply /dev/tun acls

WORK IN PROGRESS
