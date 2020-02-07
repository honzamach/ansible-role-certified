.. _section-role-certified:

Role **certified**
================================================================================

* `Ansible Galaxy page <https://galaxy.ansible.com/honzamach/certified>`__
* `GitHub repository <https://github.com/honzamach/ansible-role-certified>`__
* `Travis CI page <https://travis-ci.org/honzamach/ansible-role-certified>`__

Main purpose of this role is server certificate management. It takes care of following
tasks:

* Management of custom server certificate repository, handling certificate changes.
* Management of default CA certificate repository.
* Management of default CA certificate key repository.
* Management of trusted CA certificate repository.

**Table of Contents:**

* :ref:`section-role-certified-installation`
* :ref:`section-role-certified-dependencies`
* :ref:`section-role-certified-usage`
* :ref:`section-role-certified-variables`
* :ref:`section-role-certified-files`
* :ref:`section-role-certified-author`

This role is part of the `MSMS <https://github.com/honzamach/msms>`__ package.
Some common features are documented in its :ref:`manual <section-manual>`.


.. _section-role-certified-installation:

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


.. _section-role-certified-dependencies:

Dependencies
--------------------------------------------------------------------------------

This role is not dependent on any other role.

Following roles have dependency on this role:

* :ref:`alchemist <section-role-alchemist>`
* :ref:`griffin <section-role-griffin>` *[SOFT]*
* :ref:`griffin_watchee <section-role-griffin-watchee>` *[SOFT]*
* :ref:`logged <section-role-logged>`
* :ref:`logserver <section-role-logserver>`
* :ref:`postgresql <section-role-postgresql>` *[SOFT]*


.. _section-role-certified-usage:

Usage
--------------------------------------------------------------------------------

Example content of inventory file ``inventory``::

    [servers_certified]
    your-server

Example content of role playbook file ``role_playbook.yml``::

    - hosts: servers_certified
      remote_user: root
      roles:
        - role: honzamach.certified
      tags:
        - role-certified

Example usage::

    # Run everything:
    ansible-playbook --ask-vault-pass --inventory inventory role_playbook.yml

It is recommended to follow these configuration principles:

* Create/edit file ``inventory/group_vars/all/vars.yml`` and within define some sensible
  defaults for all your managed servers::

        # This is mandatory for soft dependency mechanism to work.
        hm_certified__cert_host_dir: /etc/ssl/servercert
        hm_certified__cert_ca_dir: /etc/ssl/certs
        hm_certified__cert_key_dir: /etc/ssl/private
        hm_certified__trustedcert_ca_dir: /etc/ssl/trusted_ca

* Use files ``inventory/host_vars/[your-server]/vars.yml`` to customize settings
  for particular servers. Please see section :ref:`section-role-certified-variables`
  for all available options.

* Use directory ``inventory/group_files/servers/honzamach.certified/ca_certs`` to
  provide additional certification authority certificates to be uploaded to all
  servers. These will end up in both :envvar:`hm_certified__cert_ca_dir` and
  :envvar:`hm_certified__trustedcert_ca_dir` directories.

* Use directory ``inventory/host_files/[your-server]/honzamach.certified/host_certs``
  to provide certificates specific for particular server. These will end up in
  :envvar:`hm_certified__cert_host_dir` directory. Please do not forget to encrypt
  at least certificate keys with :ref:`ansible-vault <section-overview-vault>`.


.. _section-role-certified-variables:

Configuration variables
--------------------------------------------------------------------------------


Internal role variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. envvar:: hm_certified__cert_host_dir

    Path to host certificate directory. This directory will contain custom server
    certificate. For soft dependency mechanism to work this must be defined globally,
    otherwise this variable will be undefined.

    * *Datatype:* ``string``
    * *Default:* ``/etc/ssl/servercert``

.. envvar:: hm_certified__cert_ca_dir

    Path to CA certificate repository.

    * *Datatype:* ``string``
    * *Default:* ``/etc/ssl/certs``

.. envvar:: hm_certified__cert_key_dir

    Path to private certificate key repository.

    * *Datatype:* ``string``
    * *Default:* ``/etc/ssl/private``

.. envvar:: hm_certified__trustedcert_ca_dir

    Path to trusted CA certificate repository. These certificates may be anything
    you use and trust in your organization, even self-signed certificates for some
    custom internal application. One example use-case is a configuration setting
    `ca_dir() <https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.25/administration-guide/ca_dir>`__
    of syslog-ng <https://www.syslog-ng.com/>`__ daemon, which you typically do not
    point to general ``/etc/ssl/certs`` directory, but to a directory containing
    a subset of certification authorities used by your log servers.

    * *Datatype:* ``string``
    * *Default:* ``/etc/ssl/trusted_ca``


.. _section-role-certified-files:

Managed files
--------------------------------------------------------------------------------

This role uploads certificates to remote directories according to configration
:ref:`variables <section-role-certified-variables>`. By default these directories
are following:

* ``/etc/ssl/servercert``
* ``/etc/ssl/certs``
* ``/etc/ssl/trusted_ca``


.. _section-role-certified-author:

Author and license
--------------------------------------------------------------------------------

| *Copyright:* (C) since 2019 Honza Mach <honza.mach.ml@gmail.com>
| *Author:* Honza Mach <honza.mach.ml@gmail.com>
| Use of this role is governed by the MIT license, see LICENSE file.
|
