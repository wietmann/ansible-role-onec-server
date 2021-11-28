Ansible Role: 1C:Enterprise server
==================================

Ansible role to install 1C:Enterprise x86-64 server and related packages on Linux hosts.

This role intends that you have achieved a legal copy of official binary packages and placed them in specific directory on target host's filesystem. You should have valid 1C:Enterprise server and platform licenses and tech support subscription in order to download packages from releases.1c.ru.

This role uses HASP driver binary package from [EterSoft repo](http://ftp.etersoft.ru/pub/Etersoft/HASP/stable/). You can disable this behavior if you want to use alternative installation methods.

1C server is dependant from [Microsoft Core fonts for the Web](https://en.wikipedia.org/wiki/Core_fonts_for_the_Web), which license disallow distribution in repackaged form. In Debian-based systems there is `ttf-mscorefonts-installer` package, which downloads fonts set and registers them in system. In RHEL third-party [package](https://sourceforge.net/projects/mscorefonts2/) exists, which requires `cabextract` from EPEL repository.

In RHEL this role switches SELinux to `permissive` mode by default to avoid problems related to lack of official SELinux policies for 1C server.

Requirements
------------

Python, Ansible >= 2.9

Role Variables
--------------

|Variable|Default value|Description|
|-|-|-|
|`onec_version`|-|First 3 numbers of 1C server version string in format `8.3.XX`. This argument should be defined explicitly.|
|`onec_release`|-|Last number of 1C server version string in format `XXXX`. This argument should be defined explicitly.|
|`onec_packages_dir`|-|Path to local directory with binary packages. Package names should be preserved. This argument should be defined explicitly.|
|`onec_server_components`|`[common, server, ws]`|List of 1C server components to install: `common`, `common-nls`, `server`, `server-nls`, `ws`, `ws-nls`, `crs`. `*-nls` packages add localization support.|
|`onec_install_haspd`|`True`|Whether to install HASP driver.|
|`onec_install_mscorefonts`|`True`|Whether to install Microsoft Core fonts.|
|`onec_selinux_state`|`permissive`|Desired state of SELinux. Allowed values: `enforcing`, `permissive`, `disabled`.|
|`onec_service_state`|`started`|Desired state of 1C service.|
|`onec_service_enabled`|`True`|Whether to start 1C service on boot.|

Dependencies
------------

None.

Supported platforms
-------------------

* RHEL 7

Example Playbook
----------------

Install server version `8.3.16.1814` from local directory `/mnt/smb/1c`:

```yaml
---
- hosts: all
  become: yes
  vars:
    onec_version: "8.3.16"
    onec_release: "1814"
    onec_packages_dir: "/mnt/smb/1c"

  roles:
    - role: wietmann.onec_server
```

License
-------

SPDX:MIT  
See full text in [LICENSE](LICENSE) file.

Author Information
------------------

This role was created in 2021 by Dmitry Danilov.
