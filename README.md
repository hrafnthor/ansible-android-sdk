# Ansible Android SDK

A Ansible role for installing Android SDK on linux machines.

## Requirements
------------

This role requires two separate tools be installed.

First it requires the 'ansible.utils' collection be installed from Ansible-Galaxy via:

```shell
ansible-galaxy collection install ansible.utils
```

Secondly it requires the `jsonschema` Python package be installed via:

```shell
pip install jsonschema
```

## Setup

Before the role can be used it needs to be added to the machine running the playbook, and as of writing this, this role is not hosted on Ansible-Galaxy only on Github.

1. Create a `requirements.yml` file in the root directory of the playbook being worked on.

2. Add the following definition inside the `requirements.yml` file:

```yml
- name: hth-android-sdk
  src: https://github.com/hrafnthor/ansible-android-sdk.git
  scm: git
```

3. Install the requirements by executing 

```shell
ansible-galaxy install -r .requirements.yml
``` 

This will allow any playbook run from this machine to use the role `hth-android-sdk`

## Variables

The role expects a top level variable `android_sdk` with the following structure (all are optional unless otherwise specified):

```yaml
android_sdk:
  remove: [boolean]   If present, will remove all files and environment variables that the role sets
  bootstrap:
    build: [integer]   The cmdtool build version to download
    checksum: [string]  The checksum of the cmdline archive.
  environment:
    sdk_home:
      path: [string]  The location where the sdk should be installed. Defaults to '/usr/lib/android/sdk'
      owner: [string] The user who should own the installation directory. Defaults to 'root'
      group: [string] The group who should own the installation directory. Defaults to 'root'
      mode: [string]  The chmod of the sdk installation directory. Defaults to '0755'
    variables:
      android_user_home: [string] The location android configuration directory per user. This path is always prefaced with $HOME. Defaults to '.android'
      android_emulator_home: [string] The location for the android emulator configuration directory per user. This path is always prefaced with $HOME. Defaults to '.android/emulator'
      android_avd_home: [string] The location for android avd configuration directory per user. This path is always prefaced with $HOME. Defaults to '.android/avd'
    link:
      adb: [boolean]  If true, adb will be symlinked to '/usr/bin'
      sdkmanager: [boolean] If true, sdkmanager will be symlinked to '/usr/bin'
    platform_tools:
      channel: [stable, beta, canary] Indicates the channel to install platform tools from. Defaults to 'stable'
    build_tools:
      - version: [string] The version to install [required]
        channel: [stable, beta, development, canary] Indicates the the channel to install from. Defaults to 'stable'
    platforms:
      - version: [integer] The version to install [required]
        channel: [stable, beta, development, canary]  Indicates the the channel to install from. Defaults to 'stable'
    ndk:
      - version: [string] The version to install [required]
        channel: [stable, beta, canary]  Indicates the the channel to install from. Defaults to 'stable'
    cmake:
      - version: [string] The version to install [required]
        channel: [stable, beta, canary]  Indicates the the channel to install from. Defaults to 'stable'
    plugdev_users: [string array] A list of users to add the 'plugdev' group to.
```

#### Bootstrap

The cmdline version information and associated checksums can be found [here](https://developer.android.com/studio#command-line-tools-only).

#### Platform Tools

The release information and versions for platform tools can be found [here](https://developer.android.com/tools/releases/platform-tools).

#### Build Tools

The release information and versions for build tools can be found [here](https://developer.android.com/tools/releases/build-tools).

#### Command-Line Tools

The release information and versions for command line tools can be found [here](https://developer.android.com/tools/releases/cmdline-tools).

### Example playbook


```yaml
- hosts: all
    vars:
      android_sdk:
        bootstrap:
          build: 11076708
          checksum: "2d2d50857e4eb553af5a6dc3ad507a17adf43d115264b1afc116f95c92e5e258"
        environment:
          sdk_home:
            group: "developers"
            mode: "2774"
          variables:
            android_user_home: ".android"
          link:
            adb: true
            sdkmanager: true
        build_tools:
          - version: "34.0.0"
            channel: "stable"
        platform_tools:
          channel: "stable"
        platforms:
          - version: 34
            channel: "stable"
        plugdev_users:
          - hrafn
  roles:
     - hth-android-sdk
```


License
-------

MIT license. See attached license file.

Author Information
------------------

Hrafn Thorvaldsson
Find me at https://www.hth.is
