# PHP7 (`php7`) - Change log

All notable changes to this role will be documented in this file.
This role adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

Remember: Make sure to update `php7_barc_role_version` variable when a new version is released.

## [Unreleased][unreleased]

## 0.2.0 - 25/07/2016

### Added

* Zip extension support for installing composer packages 

### Fixed

* Added missing system-only failure tasks and tests for installing PHP extensions
* Incorrect package name in test for XSL extension on CentOS

## 0.1.1 - 22/07/2016

### Fixed

* Variable typos in README, composer install tasks

### Added

* MBString extension is now installed by default

## 0.1.0 - 21/07/2016

### Added

* New role! - initial version based on 0.3.2 of the BARC flavour of the BAS Base Project - Pristine
