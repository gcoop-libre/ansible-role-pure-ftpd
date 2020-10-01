Pure-FTPd
=========

An Ansible Role that installs Pure-FTPd on Debian/Ubuntu.

Requirements
------------

This role only has requirements if the TLS support will be enabled and you need to generate the certificate.

If the value of `pureftpd_tls_certificate_method` is `generate`, `openssl` needs to be installed on the server.

If the value of `pureftpd_tls_certificate_method` is `certbot`, `certbot` should be available on the remote server. You may use `geerlingguy.certbot` to install it.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    pureftpd_packages:
      - pure-ftpd-common
      - pure-ftpd

List of packages to install with APT.

    pureftpd_global_config_mode: standalone
    pureftpd_global_config_virtualchroot: 'true'
    pureftpd_global_config_uploadscript: ''
    pureftpd_global_config_uploaduid: ''
    pureftpd_global_config_uploadgid: ''

Properties for the global configuration of Pure-FTPd. They are used to generate `/etc/default/pure-ftpd-common`. You can read more about these options on `templates/pure-ftpd-common.j2`.

    pureftpd_fortune: ''

Message to show on user's login.

    pureftpd_mysql:
      server: localhost
      port: 3306
      socket: /var/run/mysqld/mysqld.sock
      username: dbuser
      password: dbpass
      database: dbname
      crypt: crypt
      query_get_pw: SELECT Password FROM users WHERE User="\L"
      query_get_dir: SELECT Dir FROM users WHERE User="\L"
      query_get_uid: SELECT Uid FROM users WHERE User="\L"
      default_uid: 1000
      query_get_gid: SELECT Gid FROM users WHERE User="\L"
      default_gid: 1000
      query_get_qta_fs: SELECT QuotaFiles FROM users WHERE User="\L"
      query_get_qta_sz: SELECT QuotaSize FROM users WHERE User="\L"
      query_get_ratio_ul: SELECT ULRatio FROM users WHERE User="\L"
      query_get_ratio_dl: SELECT DLRatio FROM users WHERE User="\L"
      query_get_bandwidth_ul: SELECT ULBandwidth FROM users WHERE User="\L"
      query_get_bandwidth_dl: SELECT DLBandwidth FROM users WHERE User="\L"
      force_tilde_expansion: true
      transactions: true

This property sets the configuration needed for the storage of virtual users on a MySQL server. There is more information about these configurations on [Pure-FTPd documentation](https://download.pureftpd.org/pub/pure-ftpd/doc/README.MySQL).

    pureftpd_postgresql:
      server: localhost
      port: 5432
      username: dbuser
      password: dbpass
      database: dbname
      crypt: crypt
      query_get_pw: SELECT "Password" FROM "users" WHERE "User"='\L'
      query_get_dir: SELECT "Dir" FROM "users" WHERE "User"='\L'
      query_get_uid: SELECT "Uid" FROM "users" WHERE "User"='\L'
      default_uid: 1000
      query_get_gid: SELECT "Gid" FROM "users" WHERE "User"='\L'
      default_gid: 1000
      query_get_qta_fs: SELECT "QuotaFiles" FROM "users" WHERE "User"='\L'
      query_get_qta_sz: SELECT "QuotaSize" FROM "users" WHERE "User"='\L'
      query_get_ratio_ul: SELECT "ULRatio" FROM "users" WHERE "User"='\L'
      query_get_ratio_dl: SELECT "DLRatio" FROM "users" WHERE "User"='\L'
      query_get_bandwidth_ul: SELECT "ULBandwidth" FROM "users" WHERE "User"='\L'
      query_get_bandwidth_dl: SELECT "DLBandwidth" FROM "users" WHERE "User"='\L'

This property sets the configuration needed for the storage of virtual users on a PostgreSQL server. There is more information about these configurations on [Pure-FTPd documentation](https://download.pureftpd.org/pub/pure-ftpd/doc/README.PGSQL).

    pureftpd_ldap:
      ldaps: True
      tls: True
      server: ldap.example.com
      port: 389
      bind_dn: cn=Manager,dc=example,dc=com
      version: 3
      bind_password: bindpass
      base_dn: cn=Users,dc=example,dc=com
      filter: '&(objectClass=posixAccount)(uid=\L)'
      home_dir: homeDirectory
      default_uid: 1000
      force_default_uid: True
      default_gid: 1000
      force_default_gid: True

This property sets the configuration needed for the storage of virtual users on an LDAP server. There is more information about these configurations on [Pure-FTPd documentation](https://download.pureftpd.org/pub/pure-ftpd/doc/README.LDAP).

    pureftpd_config:
      AllowAnonymousFXP: 'no'
      AllowUserFXP: 'no'
      AltLog: 'clf:/var/log/pure-ftpd/transfer.log'
      AnonymousBandwidth: '8'
      AnonymousCanCreateDirs: 'no'
      AnonymousCantUpload: 'yes'
      AnonymousOnly: 'no'
      AnonymousRatio: '1 10'
      AntiWarez: 'yes'
      AutoRename: 'no'
      Bind: '127.0.0.1,21'
      BrokenClientsCompatibility: 'no'
      CallUploadScript: 'yes'
      ChrootEveryone: 'yes'
      ClientCharset: 'UTF-8'
      CreateHomeDir: 'yes'
      CustomerProof: 'yes'
      Daemonize: 'yes'
      DisplayDotFiles: 'yes'
      DontResolve: 'yes'
      ExtAuth: /var/run/ftpd.sock
      ForcePassiveIP: '192.168.0.1'
      FortunesFile: '/etc/pure-ftpd/cookie'
      FSCharset: 'utf8'
      IPV4Only: 'yes'
      IPV6Only: 'yes'
      KeepAllFiles: 'yes'
      LDAPConfigFile: /etc/pureftpd-ldap.conf
      LimitRecursion: '10000 8'
      LogPID: 'yes'
      MaxClientsNumber: '10'
      MaxClientsPerIP: "{{ ansible_processor_cores }}"
      MaxDiskUsage: '80'
      MaxIdleTime: '15'
      MaxLoad: '4'
      MinUID: '1000'
      MySQLConfigFile: /etc/pure-ftpd/mysql.conf
      NoAnonymous: 'yes'
      NoChmod: 'yes'
      NoRename: 'yes'
      NoTruncate: 'yes'
      PAMAuthentication: 'no'
      PassivePortRange: '30000 50000'
      PerUserLimits: '3 20'
      PGSQLConfigFile: /etc/pureftpd-pgsql.conf
      PIDFile: '/var/run/pure-ftpd.pid'
      ProhibitDotFilesRead: 'yes'
      ProhibitDotFilesWrite: 'yes'
      PureDB: /etc/pure-ftpd/pureftpd.pdb
      Quota: '1000 10'
      SyslogFacility: 'ftp'
      TLS: '0'
      TLSCipherSuite: 'ALL:!aNULL:!SSLv3'
      TrustedIP: '10.1.1.1'
      Umask: '113 002'
      UnixAuthentication: 'no'
      UserBandwidth: '8'
      UserRatio: '1 10'
      VerboseLog: 'no'

List of configuration options for Pure-FTPd. There is more information about these configurations on [Pure-FTPd documentation](https://download.pureftpd.org/pub/pure-ftpd/doc/README).

The `TLS` option has four possible values (from `0` to `3`). This value implies:

* `0`: support for SSL/TLS is disabled.
* `1`: clients can connect either the traditional way or through an SSL/TLS layer.
* `2`: cleartext sessions are refused and only SSL/TLS compatible clients are accepted.
* `3`: cleartext sessions are refused and only SSL/TLS compatible clients are accepted. Clear data connections are also refused, so private data connections are enforced.

There is more information available at [Pure-FTPd documentation](http://download.pureftpd.org/pub/pure-ftpd/doc/README.TLS).

    pureftpd_auth_puredb: 10
    pureftpd_auth_mysql: 0
    pureftpd_auth_postgresql: 0
    pureftpd_auth_ldap: 0
    pureftpd_auth_pam: 80
    pureftpd_auth_unix: 90

These properties set the priority of the different authentication methods. Only those with a value greater than `0` will be enabled.

    pureftpd_system_users:
      - name: user1
        password: <encrypted password>
        homedir: /var/ftp/user1

List of users that should be present on the system. The password of a system user should be encrypted. See the Ansible docs on [how to generate encrypted passwords](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module).

    pureftpd_system_deleted_users:
      - name: user2

List of users that should not be present on the system. These is useful to delete old FTP accounts on the system.

    pureftpd_virtual_users_user: ftp
    pureftpd_virtual_users_group: ftp

If the Pure-FTPd server will use virtual users, it needs at least a system user and his corresponding group.

    pureftpd_virtual_users_gid: ''
    pureftpd_virtual_users_uid: ''

These properties force a UID and GID for them. They are not defined by default.

    pureftpd_virtual_users:
      - name: vuser1
        password: p4ssW0rd
        homedir: /var/ftp/vuser1
        uid: 2000
        gid: 2000
        quota_files: 2000
        quota_size: 500
        bandwidth_ul: 5
        bandwidth_dl: 5
        ratio_ul: 10
        ratio_dl: 1

List of virtual users to create using PureDB as storage method. `name`, `password` and `homedir` are mandatory.

    pureftpd_virtual_deleted_users:
      - name: vuser2

List of users that should not be present on the PureDB database. This is useful to delete old FTP accounts.

    pureftpd_virtual_users_import: false

With this property enabled, the role will import the system users as virtual users.

It should be noted that only accounts that have shell access will be imported. Accounts with the shell set to nologin have to be added manually.

    pureftpd_tls_certificate_method: ''

This property has three valid values:

* `certbot`: This option will use [certbot](https://certbot.eff.org/) to request for a Let's Encrypt certificate.
* `generate`: This option will use `openssl` to create a selfsigned certificate.
* `upload`: This option will upload an existing certificate.

    pureftpd_tls_certificate_certbot:
      command: /opt/certbot/certbot-auto
      fqdn: ftp.example.com
      email: letsencrypt@example.com
      size: 4096
      port: 80

When using `certbot`, this dictionary set the `path` for certbot command and a few options needed for the certificate request. You need to set the FQDN for the certificate and the email for the Let's Encrypt account. You may change the certificate's key size and the port where `certbot` will wait for Let's Encrypt challenge.

    pureftpd_tls_certificate_openssl:
      size: 4096
      days: 365
      fqdn: ftp.example.com
      country: ''
      state: ''
      locality: ''
      organization: ''
      unit: ''

When using `generate`, this dictionary set the options for the `openssl` command.

    pureftpd_tls_certificate_file: ''
    pureftpd_tls_certificate_content: ''

When using `upload`, these options set the file to upload or the content of the certificate file to create on the server.

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: webservers
      roles:
         - gcoop-libre.pure-ftpd

License
-------

GPLv2

Author Information
------------------

This role was created in 2017 by [gcoop Cooperativa de Software Libre](https://www.gcoop.coop).
