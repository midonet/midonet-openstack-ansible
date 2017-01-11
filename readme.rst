Midonet for OpenStack-Ansible
#########################################
:date: 2017-01-11
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

Edit ``/etc/openstack_deploy/conf.d/{{cassandra}}{{zookeeper}}{{midonet-api}}.yml`` config depending on environment.

Copy Midonet variables config into place and edit depending on environment

.. code-block:: bash

    cp etc/user_midonet_variables.yml /etc/openstack_deploy/

Unset any neutron_* variable from any other config except user_midonet_variables.yml

Copy the Midonet Neutron plugin template into place

.. code-block:: bash

    cp -r templates/plugins/midonet /etc/ansible/roles/os_neutron/templates/plugins/

Overwrite neutron.conf template with modified version. See diff for changes

.. code-block:: bash

    cp templates/neutron.conf.j2 /etc/ansible/roles/os_neutron/templates/

Add the export to update the inventory file location

.. code-block:: bash

    export ANSIBLE_INVENTORY=/opt/openstack-ansible/playbooks/inventory/dynamic_inventory.py

If you are running the HA Proxy you should run the following playbook as well to enable the Midonet Cluster API port 9981

.. code-block:: bash

    openstack-ansible playbook-midonet-haproxy.yml

Create the containers

.. code-block:: bash

    openstack-ansible /opt/openstack-ansible/playbooks/lxc-containers-create.yml -e container_group=cassandra
    openstack-ansible /opt/openstack-ansible/playbooks/lxc-containers-create.yml -e container_group=zookeeper
    openstack-ansible /opt/openstack-ansible/playbooks/lxc-containers-create.yml -e container_group=midonet-api
    

WORK IN PROGRESS
