.. _section-cookbook-roles-certified:

Host certificates
================================================================================


.. _section-cookbook-roles-certified-addcerts:

Custom server certificates
--------------------------------------------------------------------------------

* You will probably use same certificate authorities on all your servers. To make
  sure they are truly available everywhere place the files into directory
  ``inventory/group_files/servers/honzamach.certified/ca_certs``. When the role
  :ref:`certified <section-role-certified>` is executed it will  copy all files
  from that directory to ``/etc/ssl/certs`` on target hosts and index them.

* Use directory ``inventory/host_files/[server-name]/honzamach.certified/host_certs``
  to provide certificates specific for particular server. These will end up in
  :envvar:`hm_certified__cert_host_dir` directory. Please do not forget to encrypt
  at least certificate keys with :ref:`ansible-vault <section-overview-vault>`.
  Use variations of following commands to do that::

    $ ansible-vault encrypt --vault-id msms@prompt inventory/host_files/[server_name]/honzamach.certified/host_certs/key.pem

  Please make sure to use the same combination of ``vault-id``:``password``, unexpected
  errors may occur otherwise. You may however use multiple pairs of ``vault-id``:``password``
  throughout your playbooks if that suits your needs, for example if you have multiple
  groups of administrators that should be permitted to manage only certain set of
  servers.

Then execute the role::

    apbf role_certified.yml

Please refer to role :ref:`certified <section-role-certified>` for more details.
