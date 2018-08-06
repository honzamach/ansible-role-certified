.. _section-role-certified:

Role **certified**
================================================================================

Ansible role for managing certificates on all target servers.

* `Ansible Galaxy page <https://galaxy.ansible.com/honzamach/certified>`__
* `GitHub repository <https://github.com/honzamach/ansible-role-certified>`__
* `Travis CI page <https://travis-ci.org/honzamach/ansible-role-certified>`__


Description
--------------------------------------------------------------------------------

Main purpose of this role is server certificate management. It takes care of following
tasks:

* Management of custom server certificate repository, handling certificate changes.
* Management of default CA certificate repository.
* Management of default CA certificate key repository.
* Management of trusted CA certificate repository.

.. note::

    This role requires the :ref:`secure registry <section-overview-secure-registry>` feature.


Requirements
--------------------------------------------------------------------------------

This role does not have any special requirements.


Dependencies
--------------------------------------------------------------------------------

This role is not dependent on any other role.

Following roles have dependency on this role:

* :ref:`logged <section-role-logged>`
* :ref:`logserver <section-role-logserver>`
* :ref:`mentat <section-role-mentat>`
* :ref:`warden_client <section-role-warden-client>`


Internal variables
--------------------------------------------------------------------------------

.. envvar:: hm_certified__cert_host_dir

    Path to host certificate directory. This directory will contain custom server
    certificate.

    * *Datatype:* ``string``
    * *Default value: /etc/ssl/servercert*

.. envvar:: hm_certified__cert_ca_dir

    Path to CA certificate repository.

    * *Datatype:* ``string``
    * *Default value: /etc/ssl/certs*

.. envvar:: hm_certified__cert_key_dir

    Path to private certificate key repositoryy.

    * *Datatype:* ``string``
    * *Default value: /etc/ssl/private*

.. envvar:: hm_certified__trustedcert_ca_dir

    Path to trusted CA certificate repository.

    * *Datatype:* ``string``
    * *Default value: /etc/ssl/trusted_ca*


Usage and customization
--------------------------------------------------------------------------------

This role is (attempted to be) written according to the `Ansible best practices <https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html>`__. The default implementation should fit most users,
however you may customize it by tweaking default variables and providing custom
templates.


Variable customizations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Most of the usefull variables are defined in ``defaults/main.yml`` file, so they
can be easily overridden almost from `anywhere <https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable>`__.


Installation
--------------------------------------------------------------------------------

To install the role `honzamach.certified <https://galaxy.ansible.com/honzamach/certified>`__
from `Ansible Galaxy <https://galaxy.ansible.com/>`__ please use variation of
following command::

    ansible-galaxy install honzamach.certified

To install the role directly from `GitHub <https://github.com>`__ by cloning the
`ansible-role-certified <https://github.com/honzamach/ansible-role-certified>`__
repository please use variation of following command::

    git clone https://github.com/honzamach/ansible-role-certified.git honzamach.certified

Currently the advantage of using direct Git cloning is the ability to easily update
the role when new version comes out.


Example Playbook
--------------------------------------------------------------------------------

Example content of inventory file ``inventory``::

    [servers-certified]
    localhost

Example content of role playbook file ``playbook.yml``::

    - hosts: servers-certified
      remote_user: root
      roles:
        - role: honzamach.certified
      tags:
        - role-certified

Example usage::

    # Run everything:
    ansible-playbook -i inventory playbook.yml


License
--------------------------------------------------------------------------------

MIT


Author Information
--------------------------------------------------------------------------------

Jan Mach <honza.mach.ml@gmail.com>
