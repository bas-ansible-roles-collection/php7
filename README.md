# PHP7 (`php7`)

Master: [![Build Status](https://semaphoreci.com/api/v1/bas-ansible-roles-collection/php7/branches/master/badge.svg)](https://semaphoreci.com/bas-ansible-roles-collection/php7)
Develop: [![Build Status](https://semaphoreci.com/api/v1/bas-ansible-roles-collection/php7/branches/develop/badge.svg)](https://semaphoreci.com/bas-ansible-roles-collection/php7)

Installs and configures PHP 7 with selected extensions and the Composer package manager

**Part of the BAS Ansible Role Collection (BARC)**

**This role uses version 0.3.2 of the BARC flavour of the BAS Base Project - Pristine**.

## Overview

* Installs the latest stable version of PHP 7 from non-system sources
* Harmonises defaults for SAPIs, and some optional extensions, between CentOS and Ubuntu Operating Systems
* Configures the PHP configuration file for the CLI SAPI using recommended settings to improve security
* Optionally, installs, enables and configures the Zend OpCache extension, this is enabled by default
* Optionally, installs and enables the cURL PHP extension, this is enabled by default
* Optionally, installs and enables the GMP PHP extension, this is enabled by default
* Optionally, installs and enables the XSL PHP extension, this is enabled by default
* Optionally, installs and enables the PDO PHP extension, this is enabled by default
* Optionally, installs and enables the Zip PHP extension, this is enabled by default
* Optionally, installs, enables and configures the XDebug debugger extension, this is disabled by default
* Optionally, installs the latest stable version of Composer, the PHP package manager, this is enabled by default

## Quality Assurance

This role uses manual and automated testing to ensure its features work as advertised.
See [here](tests/README.md) for more information.

## Ansible compatibility

* this role supports Ansible 1.8 or higher in the 1.x series
* this role supports Ansible 2.x

**Note:** Support for Ansible 1.x is deprecated by this role, future versions will support Ansible 2.x only.

More information on Ansible compatibility is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Ansible-compatbility).

## Dependencies

* none

More information on role dependencies is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-dependencies)

## Requirements

* none

More information on role requirements is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-requirements)

## Limitations

* The relevant 'php-cli' package is selected over the main 'php' package to avoid Apache web-server dependency

The `php` meta packages have dependencies which will, by default, install the Apache web-server. Though this is likely 
convenient for some, as a generic role, this is completely unsuitable. Other roles can be used to integrate PHP with 
a web-server if that is desired. Applies to all supported Operating Systems, with or without non-system package sources.

*This limitation is considered to be significant. A workaround is in place to mitigate this limitation, pending a*
*permanent resolution from upstream sources. Pull requests addressing this limitation will be considered.*

See [BARC-99](https://jira.ceh.ac.uk/browse/BARC-99) for further details.

* On CentOS, the PHP configuration file is the same for all SAPIs

CentOS uses a single configuration file, `/etc/php.ini`, for all SAPIs, there is no CLI SAPI specific configuration,
unlike on Ubuntu.

CentOS does support loading configuration files from a directory of configuration files, specifically, 
`/etc/php.d/*.ini`, but these configuration files are included by the main configuration file, and so used by all SAPIs.

*This limitation is considered to be significant. Solutions will be actively pursued. Pull requests to address this* 
*will be gratefully considered and given priority.*

See [BARC-100](https://jira.ceh.ac.uk/browse/BARC-100) for further details.

* Extensions which have been enabled, and later disabled, are not actively disabled on machines this role is applied to

This behaviour relates to how *disabled* applies to extensions managed by this role.

* Where an extension is *enabled* it will be installed, if necessary, and enabled on machines this role is applied to
* Where an extension is *disabled* it will not installed on machines this role is applied to

This means:
* If an extension would not normally be enabled, and is disabled in this role it will remain disabled
* If an extension would normally be enabled, and is disabled in this role it will remain enabled
* Additionally, if an extension would not normally be enabled, is enabled in this role, applied, then disabled and 
applied, it will remain enabled

This is considered acceptable a machines are assumed to be easily replaceable, and so in situations where a previously 
enabled extension should now be disabled, it is assumed it is acceptable to rebuild the relevant machines where the 
extension will then start disabled.

It is recognised some extensions are initially enabled (by the relevant PHP package, not this role) and that these
extensions cannot therefore be disabled. However disabling these extensions is by definition not typical and therefore 
acceptable to require users to override with their own tasks. If this is needed, this role will not interfere, unless
the relevant extension is enabled in this role. This currently only applies to the Zend OpCache extension.

*This limitation is **NOT** considered to be significant.*
*Solutions will be **NOT** actively pursued as it is intentional.*
*Pull requests to address this will **NOT** be considered.*

See [BARC-101](https://jira.ceh.ac.uk/browse/BARC-101) for further details.

* Not all extensions installed by default are available on all supported Operating Systems or PHP versions

Specifically a number of optional, lesser used, PHP extensions are installed by default in some Operating Systems and 
PHP versions and not others.

Note: The version of PHP installed can be considered a proxy as to whether non-system or system only packages are used.

In the case of well-used or important modules, this role will harmonise which extensions are available (for example 
OpCache). For these other, lesser used, extensions there will be an inconsistency.

Where these lesser used extensions are a requirement, for a particular project for example, it is NOT safe to rely on 
this role to ensure they are available. Instead it would be required to use additional roles or playbooks to test for, 
and if necessary ensure, these extensions being available.

*This limitation is **NOT** considered to be significant. Solutions will **NOT** be actively pursued.*
*Pull requests addressing this limitation will be considered.*

See [BARC-102](https://jira.ceh.ac.uk/browse/BARC-102) for further details.

* PHP 5 and PHP 7 coexisting is not a supported configuration

I.e. Applying the `php5` and `php7` to the same machine.

This is not a scenario that is expected within BAS projects as separate VMs would always be used. Therefore testing for
this configuration, and addressing any issues this brings to light are not priorities.

If there is significant demand for this support this can of course be reviewed. It may be there are no, or very few,
incompatibilities, however the testing to confirm this would first need to performed.

*This limitation is **NOT** considered to be significant. Solutions will **NOT** be actively pursued.*
*Pull requests addressing this limitation will be considered.*

See [BARC-129](https://jira.ceh.ac.uk/browse/BARC-129) for further details.

* XDebug package is missing on CentOS

The non-system source used for PHP 7 packages on CentOS is missing the package for XDebug. This package should exist 
and is assumed to be an oversight, rather than a fundamental incompatibility etc. The missing package has been reported 
to the package maintainer for a fix.

Until this package is available, XDebug cannot be used on CentOS.

*This limitation is considered to be significant. Solutions will be actively pursued.*
*Pull requests addressing this limitation will be considered.*

See [BARC-130](https://jira.ceh.ac.uk/browse/BARC-130) for further details.

More information on role limitations is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-limitations)

## Usage

### BARC Manifest

By default, BARC roles will record that they have been applied to a system, this is termed the BAS Manifest.
The specific local facts set for this role are:

* `ansible_local.barc_php7.general.role_applied` - (boolean) records whether a role has been applied
* `ansible_local.barc_php7.general.role_version` - (string) records the version of the role that was applied

**Note:** You **SHOULD** use this feature to determine whether this role has been applied to a system.
If you do not want these facts to be set by this role, you **MUST** skip the **BARC_SET_MANIFEST** tag.

More information is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-Manifest)

### Coexistence with PHP 5

Coexistence with PHP 5 is not supported by this role. I.e. Applying the `php5` role alongside this role is not 
supported. Instead it is assumed machines will run either PHP 5 **OR** PHP 7.

Upgrading from PHP 5 to PHP 7 is not supported by this role. Instead it is assumed machines will rebuilt with the new
version.

These issues are considered a limitation, see the *Limitations* section for more information.

### PHP version

This role will ensure the version of PHP installed is at least PHP *7.0*. This role will ensure the Major and Minor 
versions are the same across supported operating systems. This role will **NOT** ensure the patch version is the same.

In future, as PHP 7 is available through system-only package sources, the Minor PHP version may not be uniform across
supported operating systems. In this case additional guidance will be provided in this role, when this support is 
available.

Note: The relevant `php-cli` package is installed by this role, rather than the more general `php` package. This is to
avoid undesired dependencies on the Apache web-server.

This is considered a limitation, see the *limitations* section for more information.

### Non-system packages

It is a convention of BARC roles to use the latest version of packages. Where a suitable non-system package source is 
available it will be used. Suitable non-system packages require a reputable, maintainer, typically a company or well 
respected individual. If this is for some reason unsuitable, it is possible to only use system packages by setting the 
*BARC_use_non_system_package_sources* variable to `false`.

Note: As the package policy varies between system and non-system package sources, and between operating systems, the 
version of installed packages is variable.

Note: System only packages for PHP 7 are not yet available for the supported operating systems of this role. 
If non-packages are disabled, this role will intentionally fail immediately.

### PHP configuration files

PHP uses a configuration file to set various options, such as which modules are enabled and how these are configured, 
e.g. which time-zone to use.

This configuration file uses the INI format, which consists of sections containing options with a value. In PHP this
configuration file can be a single file (e.g. `php.ini`) and or a directory of files (e.g. `php.d/*.ini`).

E.g. Core configuration options can be set in a main `php.ini` file, with module specific configuration contained in
per-module configuration files included in the main configuration file.

In addition, configuration options can be set on a per SAPI (Server API) basis. For example settings may differ between
the `cli` SAPI (i.e. the Command Line) and the `apache` SAPI (i.e. a Web Server).

The combination of these various configuration options differ by Operating System, and package used to install PHP.
This variation is considered a limitation, see the *limitations* section for more information.

This role, which configures the `cli` SAPI only, will only configure options relevant to the `cli` SAPI specifically 
(such as a longer execution time). However, on CentOS there is no configuration file limited to the `cli` SAPI, and 
the options set by this role therefore apply to *all* SAPIs. This is considered a limitation, see the *limitations* 
section for more information.

Settings for other SAPIs, such as web-servers, **SHOULD** be set in other roles, or project/purpose specific playbooks.
The tasks used in this role can be used as a template for setting additional options. Providing the Ansible INI module
is used, setting additional INI options will not cause conflicts with this role and so preserve idempotency.

The configuration options set by this role are considered suitably generic and applicable to all situations such that 
they are safe to be used as defaults - however it is accepted there are situations they would not be suitable. They 
include options for:

* Maximum execution time and memory limits
* Default timezone - localised to *Europe/London*
* Error reporting and logging
* Concealing the PHP version for security purposes

See the *php7_sapi_cli_options* variable in the role defaults file (`defaults/main.yml`) for the specific options set.
Where any of these options are unsuitable, override this variable with a copy of these defaults, omitting the unsuitable
options.

### PHP extensions

#### Non-Harmonised extensions

This role aims to ensure the same extensions are available regardless of the Operating System used, or the version of
PHP that is installed.

For the extensions in the named sub-sections listed here this is true. However there are some, lesser used, extensions
which may exist on some combinations of Operating System and PHP version, and not others. This inconsistency for lesser
used extensions is considered a limitation, see the *Limitations* section for more information.

The following extensions are **NOT** harmonised:

* *bcmath*
  * "For arbitrary precision mathematics PHP offers the Binary Calculator which supports numbers of any size and 
  precision, represented as strings"
  * [More information](http://php.net/manual/en/book.bc.php)
  * As this extension is so similar to the GMP extension, which is made available on all OS/PHP combinations by this
  role, it is not considered necessary to install this extension as well
  * Available on: Ubuntu - all PHP versions
  * NOT available on: CentOS - all PHP versions 
* *dba*
  * "Foundation for accessing Berkeley DB style databases, a general abstraction layer for several file-based databases"
  * [More information](http://php.net/manual/en/book.dba.php)
  * Available on: Ubuntu - all PHP versions
  * NOT available on: CentOS - all PHP versions 

#### Zend OpCache

"OPcache improves PHP performance by storing pre-compiled script byte-code in shared memory, thereby removing the need 
for PHP to load and parse scripts on each request. This extension is bundled with PHP 5.5.0 and later"

Source: http://php.net/manual/en/book.opcache.php

This extension is enabled by default - it is controlled by the *php7_use_opcache* variable.

This role configures options for this extension. It is not safe to override options this role sets, however it is safe 
to override the *php7_ext_opcache_options* variable to remove options or change values this role sets.

It is safe to set any other options this role does not outside this role, they will not be overridden.

The following options are set:

* `opcache.fast_shutdown`
  * [OpCache documentation](http://php.net/manual/en/opcache.configuration.php#ini.opcache.fast-shutdown)
  * Enabled for consistency between CentOS and Ubuntu defaults (CentOS wins)
* `opcache.interned_strings_buffer`
  * [OpCache documentation](http://php.net/manual/en/opcache.configuration.php#ini.opcache.interned-strings-buffer)
  * Enabled for consistency between CentOS and Ubuntu defaults (CentOS wins)
* `opcache.max_accelerated_files`
  * [OpCache documentation](http://php.net/manual/en/opcache.configuration.php#ini.opcache.max-accelerated-files)
  * Enabled for consistency between CentOS and Ubuntu defaults (CentOS wins)
* `opcache.memory_consumption`
  * [OpCache documentation](http://php.net/manual/en/opcache.configuration.php#ini.opcache.memory-consumption)
  * Enabled for consistency between CentOS and Ubuntu defaults (CentOS wins)

Note: As the version of PHP installed is inconsistent, this role ensures the OpCache extension is present and enabled in 
all versions of PHP on all supported Operating Systems, where this feature is desired.

Note: If you enable this extension and then later choose to disable it, or where this extension is enabled by default,
such as on Ubuntu, this role will not disable the extension. Instead you will need to re-build any machines this role 
has been applied to.

This is considered a limitation, but by intention and will not be addressed, see the *Limitations* section for more 
information.

#### cURL

"PHP supports libcurl, a library created by Daniel Stenberg, that allows you to connect and communicate to many 
different types of servers with many different types of protocols."

Source: http://php.net/manual/en/book.curl.php

This extension is enabled by default - it is controlled by the *php7_use_curl* variable.
Currently this role does not configure options for this extension, however it is safe to do this outside this role.

Note: If you enable this extension and then later choose to disable it, or where this extension is enabled by default,
such as on CentOS, this role will not disable the extension. Instead you will need to re-build any machines this role 
has been applied to.

This is considered a limitation, but by intention and will not be addressed, see the *Limitations* section for more 
information.

#### GMP

"These functions allow for arbitrary-length integers to be worked with using the GNU MP library."

Source: http://php.net/manual/en/book.gmp.php

This extension is enabled by default - it is controlled by the *php7_use_gmp* variable.
Currently this role does not configure options for this extension, however it is safe to do this outside this role.

Note: If you enable this extension and then later choose to disable it, or where this extension is enabled by default,
such as on CentOS, this role will not disable the extension. Instead you will need to re-build any machines this role 
has been applied to.

This is considered a limitation, but by intention and will not be addressed, see the *Limitations* section for more 
information.

#### XSL

"The XSL extension implements the XSL standard, performing XSLT transformations using the libxslt library"

Source: http://php.net/manual/en/book.xsl.php

This extension is enabled by default - it is controlled by the *php7_use_xsl* variable.
Currently this role does not configure options for this extension, however it is safe to do this outside this role.

Note: If you enable this extension and then later choose to disable it, this role will not disable the extension. 
Instead you will need to re-build any machines this role has been applied to.

This is considered a limitation, but by intention and will not be addressed, see the *Limitations* section for more 
information.

Note: On CentOS, the  'XML' package includes the 'XSL' extension.

#### MBString

"mbstring provides multibyte specific string functions that help you deal with multibyte encodings in PHP"

Source: http://php.net/manual/en/book.mbstring.php

This extension is enabled by default - it is controlled by the *php7_use_mbstring* variable.
Currently this role does not configure options for this extension, however it is safe to do this outside this role.

Note: If you enable this extension and then later choose to disable it, or where this extension is enabled by default,
such as on CentOS, this role will not disable the extension. Instead you will need to re-build any machines this role 
has been applied to.

This is considered a limitation, but by intention and will not be addressed, see the *Limitations* section for more 
information.

#### PDO

"The PHP Data Objects (PDO) extension defines a lightweight, consistent interface for accessing databases in PHP."

Source: http://php.net/manual/en/book.pdo.php

This extension is enabled by default - it is controlled by the *php7_use_pdo* variable.
Currently this role does not configure options for this extension, however it is safe to do this outside this role.

Note: If you enable this extension and then later choose to disable it, or where this extension is enabled by default,
such as on CentOS, this role will not disable the extension. Instead you will need to re-build any machines this role 
has been applied to.

This is considered a limitation, but by intention and will not be addressed, see the *Limitations* section for more 
information.

Note: By default only the SQLite PDO driver will be installed. Other drivers can be installed outside of this role,
either using other BARC roles, or through additional playbooks.

#### Zip

"This extension enables you to transparently read or write ZIP compressed archives and the files inside them."

Source: http://php.net/manual/en/book.zip.php

This extension is enabled by default - it is controlled by the *php7_use_pdo* variable.
Currently this roles does not configure options for this extension, however it is safe to do this outside this role.

Note: If you enable this extension and then later choose to disable it, or where this extension is enabled by default, 
this role will not disable the extension. Instead you will need to re-build any machines this role has been applied to.

This is considered a limitation, but by intention and will not be addressed, see the *Limitations* section for more 
information.

Note: This extension is principally installed for use by Composer when installing packages.

#### XDebug

"Xdebug is a PHP extension which provides debugging and profiling capabilities. It uses the DBGp debugging protocol."

Source: https://xdebug.org/

This extension is disabled by default - it is controlled by the *php7_use_xdebug* variable.

Important: Enabling this extension introduces additional performance and security considerations. This extension 
**MUST NOT** be enabled in production environments, or in any environment where sensitive information is involved,
unless these considerations are suitably addressed.

This role configures options for this extension. It is not safe to override options this role sets, however it is safe 
to override the *php7_ext_xdebug_options* variable to remove options or change values this role sets.

It is safe to set any other options this role does not outside this role, they will not be overridden.

The following options are set:

* `xdebug.remote_connect_back`
  * [XDebug documentation](https://xdebug.org/docs/all_settings#remote_connect_back)
  * Enabled to allow any machine to debug a session - note this has security implications meaning this extension 
    **MUST NOT** be used in production environments
* `xdebug.remote_enable`
  * [XDebug documentation](https://xdebug.org/docs/all_settings#remote_enable)
  * Instructs XDebug to contact potentially listening remote clients, or otherwise continue with normal execution

Note: XDebug cannot currently by used on CentOS machines as the required package is missing.
This is considered a limitation, see the *limitations* section for more information.

Note: If you enable this extension and then later choose to disable it, this role will not disable the extension.
Instead you will need to re-build any machines this role has been applied to.

This is considered a limitation, but by intention and will not be addressed, see the *Limitations* section for more 
information.

### Composer

[Composer](https://getcomposer.org/) is the *de-facto* package manager for PHP. Its use is similar to other language
package managers such as Gem for Ruby, Pip for Python and NPM for NodeJS.

Composer uses a `composer.json` to define project dependencies, which are then downloaded to a `vendors` directory and
can be easily imported into a project using the bundled `autoload.php` file, which leverages the PSR-0 and PSR-4 
autoloading standards. Dependency versions can be expressed using Semantic Versioning or with more direct identifiers.

This role will install Composer as a locally available utility as it is considered best practice. This means to run
`composer install` is as simple as:

```shell
$ cd /srv/project-containing-composer.json
$ composer install
```

Though it is strongly encouraged to use Composer, if not desired it can be ignored and will not interfere with the use
of PHP. Alternatively you can set the *php7_use_composer* variable to `false` to prevent its installation.

### Typical playbook

```yaml
---

- name: ...
  hosts: all
  become: yes
  vars: []
  roles:
    - bas-ansible-roles-collection.php7
```

### Tags

BARC roles use standardised tags to control which aspects of an environment are changed by roles.
Tasks in this role will 'tag' themselves with these tags as appropriate, typically in `tasks/main.yml`.

More information is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Appendix-B---BARC-Standardised)

### Variables

#### *php7_barc_role_name*

* **MUST NOT** be specified
* specifies the name of this role within the BAS Ansible Roles Collection (BARC) used for setting local facts
* see the *BARC roles manifest* section for more information
* example: `php7`

#### *php7_barc_role_version*

* **MUST NOT** be specified
* specifies the name of this role within the BAS Ansible Roles Collection (BARC) used for setting local facts
* see the *BARC roles manifest* section for more information
* example: `2.0.0`

#### *php7_sapi_cli_options*

**MAY** be specified.

Specifies configuration options for the CLI (Command Line) SAPI.

Note: On some Operating Systems, these options may apply beyond the scope of the CLI SAPI. See the *Limitations*
section for more information.

Structured as a list of items, with each item having the following properties:

* *section*
    * **MUST** be specified
    * Specifies the section of the INI configuration file options for this item belong to
    * Values **MUST** be valid section names as determined by the INI configuration format
* *options*
    * **MAY** be specified
    * Specifies the list of options, and values, to be set within the section set by the *section* item
    * Structured as a list of sub-items, with each sub-item having the following properties
        * *option*
            * **MUST** be specified if any part of the sub-item is specified
            * Specifies the *option* of the INI option/value pair
            * Values **MUST** be valid option names as determined by the INI configuration format and **MUST** be valid
            option names as determined by PHP and the XDebug extension
        * *value*
            * *MUST** be specified if any part of the sub-item is specified
            * Specifies the *value* of the INI option/value pair
            * Values **MUST** be valid values as determined by the INI configuration format and **MUST** be valid
            option names as determined by PHP and the XDebug extension
            * Boolean values **MUST** be quoted to prevent Ansible coercing values to True/False which is invalid for 
            PHP configurations

Example:

```yml
php7_sapi_cli_options:
  - section: "Resource Limits"
    options:
      - option: max_execution_time  # (seconds)
        value: 30
```

Default: *See role defaults*

#### *php7_use_opcache*

* **MAY** be specified
* Specifies whether the Zend OpCache extension should be installed to improve performance
* This variable is used as a 'feature flag' for whether tasks related to the OpCache extension will be applied
* See the *Usage* section for more information on this feature
* Values **MUST** use one of these options, as determined by Ansible:
  * `true`
  * `false`
* Values **SHOULD NOT** be quoted to prevent Ansible coercing values to a string
* Where not specified, a value of `true` will be assumed
* Default: `true`

#### *php7_ext_opcache_options*

**MAY** be specified.

Specifies configuration options for the OpCache Extension.
See the *Usage* section for details on the default options set for this extension.

Structured as a list of items, with each item having the following properties:

* *section*
    * **MUST** be specified as `OPCache`
    * Specifies the section of the INI configuration file options for this item belong to
    * Values **MUST** be the valid section names for this extension as determined by PHP
* *options*
    * **MAY** be specified
    * Specifies the list of options, and values, to be set within the section set by the *section* item
    * Structured as a list of sub-items, with each sub-item having the following properties
        * *option*
            * **MUST** be specified if any part of the sub-item is specified
            * Specifies the *option* of the INI option/value pair
            * Values **MUST** be valid option names as determined by the INI configuration format and **MUST** be valid
            option names as determined by PHP and the OpCache extension
        * *value*
            * *MUST** be specified if any part of the sub-item is specified
            * Specifies the *value* of the INI option/value pair
            * Values **MUST** be valid values as determined by the INI configuration format and **MUST** be valid
            option names as determined by PHP and the OpCache extension
            * Boolean values **MUST** be quoted to prevent Ansible coercing values to True/False which is invalid for 
            PHP configurations

Example:

```yml
php7_ext_opcache_options:
  - section: "XDebug"
    options:
      - option: remote_connect_back
        value: "On"
```

Default: *See role defaults*

#### *php7_use_curl*

* **MAY** be specified
* Specifies whether the cURL PHP extension should be installed to interact with remote services
* This variable is used as a 'feature flag' for whether tasks related to the cURL extension will be applied
* See the *Usage* section for more information on this feature
* Values **MUST** use one of these options, as determined by Ansible:
  * `true`
  * `false`
* Values **SHOULD NOT** be quoted to prevent Ansible coercing values to a string
* Where not specified, a value of `true` will be assumed
* Default: `true`

#### *php7_use_gmp*

* **MAY** be specified
* Specifies whether the GMP PHP extension should be installed to support arbitrary precision arithmetic
* This variable is used as a 'feature flag' for whether tasks related to the GMP extension will be applied
* See the *Usage* section for more information on this feature
* Values **MUST** use one of these options, as determined by Ansible:
  * `true`
  * `false`
* Values **SHOULD NOT** be quoted to prevent Ansible coercing values to a string
* Where not specified, a value of `true` will be assumed
* Default: `true`

#### *php7_use_xsl*

* **MAY** be specified
* Specifies whether the XLS PHP extension should be installed to support XSL transformations
* This variable is used as a 'feature flag' for whether tasks related to the XSL extension will be applied
* See the *Usage* section for more information on this feature
* Values **MUST** use one of these options, as determined by Ansible:
  * `true`
  * `false`
* Values **SHOULD NOT** be quoted to prevent Ansible coercing values to a string
* Where not specified, a value of `true` will be assumed
* Default: `true`

#### *php7_use_mbstring*

* **MAY** be specified
* Specifies whether the MBString PHP extension should be installed to support Multibyte encodings in PHP 
* This variable is used as a 'feature flag' for whether tasks related to the MBString extension will be applied
* See the *Usage* section for more information on this feature
* Values **MUST** use one of these options, as determined by Ansible:
  * `true`
  * `false`
* Values **SHOULD NOT** be quoted to prevent Ansible coercing values to a string
* Where not specified, a value of `true` will be assumed
* Default: `true`

#### *php7_use_pdo*

* **MAY** be specified
* Specifies whether the PDO PHP extension should be installed to interact with various databases
* This variable is used as a 'feature flag' for whether tasks related to the PDO extension will be applied
* See the *Usage* section for more information on this feature
* Values **MUST** use one of these options, as determined by Ansible:
  * `true`
  * `false`
* Values **SHOULD NOT** be quoted to prevent Ansible coercing values to a string
* Where not specified, a value of `true` will be assumed
* Default: `true`

#### *php7_use_zip*

* **MAY** be specified
* Specifies whether the Zip PHP extension should be installed to support reading and writing Zip archives in PHP 
* This variable is used as a 'feature flag' for whether tasks related to the Zip extension will be applied
* See the *Usage* section for more information on this feature
* Values **MUST** use one of these options, as determined by Ansible:
  * `true`
  * `false`
* Values **SHOULD NOT** be quoted to prevent Ansible coercing values to a string
* Where not specified, a value of `true` will be assumed
* Default: `true`

#### *php7_use_composer*

* **MAY** be specified
* Specifies whether the Composer PHP package manager should be installed to manage PHP project dependencies
* This variable is used as a 'feature flag' for whether tasks related to Composer will be applied
* See the *Usage* section for more information on this feature
* Values **MUST** use one of these options, as determined by Ansible:
  * `true`
  * `false`
* Values **SHOULD NOT** be quoted to prevent Ansible coercing values to a string
* Where not specified, a value of `true` will be assumed
* Default: `true`

#### *php7_use_xdebug*

* **MAY** be specified
* Specifies whether the XDebug extension should be installed to aid in debugging
* This variable is used as a 'feature flag' for whether tasks related to the XDebug extension will be applied
* See the *Usage* section for more information on this feature
* Values **MUST** use one of these options, as determined by Ansible:
  * `true`
  * `false`
* Values **SHOULD NOT** be quoted to prevent Ansible coercing values to a string
* Where not specified, a value of `true` will be assumed
* Default: `false`

#### *php7_ext_xdebug_options*

**MAY** be specified.

Specifies configuration options for the XDebug Extension.
See the *Usage* section for details on the default options set for this extension.

Structured as a list of items, with each item having the following properties:

* *section*
    * **MUST** be specified as `XDebug`
    * Specifies the section of the INI configuration file options for this item belong to
    * Values **MUST** be the valid section names for this extension as determined by PHP
* *options*
    * **MAY** be specified
    * Specifies the list of options, and values, to be set within the section set by the *section* item
    * Structured as a list of sub-items, with each sub-item having the following properties
        * *option*
            * **MUST** be specified if any part of the sub-item is specified
            * Specifies the *option* of the INI option/value pair
            * Values **MUST** be valid option names as determined by the INI configuration format and **MUST** be valid
            option names as determined by PHP and the XDebug extension
        * *value*
            * *MUST** be specified if any part of the sub-item is specified
            * Specifies the *value* of the INI option/value pair
            * Values **MUST** be valid values as determined by the INI configuration format and **MUST** be valid
            option names as determined by PHP and the XDebug extension
            * Boolean values **MUST** be quoted to prevent Ansible coercing values to True/False which is invalid for 
            PHP configurations

Example:

```yml
php7_ext_xdebug_options:
  - section: "XDebug"
    options:
      - option: remote_connect_back
        value: "On"
```

Default: *See role defaults*

## Developing

### Issue tracking

Issues, bugs, improvements, questions, suggestions and other tasks related to this package are managed through the
[BAS Ansible Roles Collection](https://jira.ceh.ac.uk/projects/BARC) (BARC) project on Jira.

This service is currently only available to BAS or NERC staff, although external collaborators can be added on request.
See our contributing policy for more information.

More information is also available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Issue-Tracking)

### Source code

All changes should be committed, via pull request, to the canonical repository:

`ssh://git@stash.ceh.ac.uk:7999/barc/php7.git`

A read-only mirror of this repository is maintained on GitHub:

`git@github.com:bas-ansible-roles-collection/php7.git`

More information is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Source-Code)

### Contributing policy

This role welcomes contributions, see `CONTRIBUTING.md` for our general policy.

### Release procedure

The general release procedure for BARC roles is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Release-procedures)

## Licence

Copyright 2016 NERC BAS.

Unless stated otherwise, all documentation is licensed under the Open Government License - version 3.
All code is licensed under the MIT license.

Copies of these licenses are included within this role.
