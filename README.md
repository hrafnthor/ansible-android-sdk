Role Name
=========

An opinionated role that installs the Android SDK's command line toolset and 
any other requested part of the SDK.

Requirements
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


Role Variables
--------------

`android_cmdlinetools_bootstrap_build` : [number] (required) The build number of the `cmdline-tools` package which should be downloaded as part of the bootstrap process. The build numbers can be found [here](https://developer.android.com/studio#command-line-tools-only)

`android_cmdlinetools_bootstrap_checksum` : [string] (required) The checksum associated with the build version of the `cmdline-tools` package that will be used as part of the bootstrap process. The checksums can be found [here](https://developer.android.com/studio#command-line-tools-only)

`android_adb_to_path` : [boolean] Indicates if the `adb` binary should be added to the global path (default: `false`).

`android_sdkmanager_to_path` : [boolean] Indicates if the `sdkmanager` binary should be added to the global path (default: `false`).

`android_sdk_home` : [string] The absolute path to where the Android SDK will be 
installed (default: `/usr/lib/android/sdk`).

`android_sdk_bin` : [string] The absolute path to where Android SDK binaries (suchas `adb` and `sdkmanager`) will be linked and shared with the global path (default: `/usr/lib/android/bin`)

`android_user_home` : [string] The path relative to each user's home directory where user specific SDK configuration and files will be stored (default: `.android`) 

`android_emulator_home` : [string] The path relative to each user's home directory where emulator specific configuration and files will be stored (default: `.android`)

`android_avd_home` : [string] The absolute path to where the Android Virtual Disk image will be stored for the emulator (default: `$USER/.android`)

`android_requested_sdk` : [mapping] Defines the parts of the SDK that will be installed, their versions and from what channel.

###### Structure of android_requested_sdk map

`build_tools` : [array] Contains information on each version of build tools that will be installed:

    - `version` : [string] (required) Must follow the pattern `^[0-9]+.[0-9]+.[0-9]+$`
    - `channel` : [number] Must be one of the following 0: stable, 1: beta, 2: dev, 3: canary

Example:

```yaml
- build_tools:
    - version: "31.0.1"
      channel: 0
    - version: "29.0.2"
      channel: 2
 ```

`platforms` : [array] Contains information on each version of the platform that will be installed:

    - `version` : [number] (required) Must follow the pattern [0-9]+
    - `channel` : [number] Must be one of the following 0: stable, 1: beta, 2: dev, 3: canary

Example:

```yaml
- platforms:
    - version: 33
      channel: 1
    - version: 29
      channel: 0
```

`platform_tools` : [object] Contains information on the version of platform tools that will be installed. Only information contained is the channel, otherwise it will always install the latest version

 - `channel` : [number] Must be one of the following 0: stable, 1: beta, 2: dev, 3: canary


Example:

```yaml
- platform_tools:
    channel: 0
```

`ndk` : [array] Contains information on each version of the NDK that will be installed:

    - `version` : [string] (required) Must follow the pattern [0-9]+.[0-9]+.[0-9]+
    - `channel` : [number] Must be one of the following 0: stable, 1: beta, 3: canary
 
Example:
```yaml
- ndk:
    - version: "25.0.8775105"
      channel: 0
    - version: "25.1.8937393"
      channel: 3
```

`cmake` : [array] Contains information on each version of CMake that will be installed:

    - `version` : [string] (required) Must follow the pattern [0-9]+.[0-9]+.[0-9]+
    - `channel` : [number] Must be one of the following 0: stable, 1: beta, 3: canary

Example:
```yaml
- cmake:
    - version: "3.22.1"
      channel: 0
    - version: "3.6.4111459"
      channel: 0
```

`plugdev_users` : [array] Contains the user names who should be added to the `plugdev` group

Dependencies
------------

None, other then previously stated requirements.

Setup
-----

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


Example Playbook
----------------


```yaml
- hosts: all
    vars:
    - android_user_home: ".android"
    - android_adb_to_path: true
    - android_sdkmanager_to_path: true
    - android_cmdlinetools_bootstrap_build: 9123335
    - android_cmdlinetools_bootstrap_checksum: "0bebf59339eaa534f4217f8aa0972d14dc49e7207be225511073c661ae01da0a"
    - android_requested_sdk:
        build_tools:
        - version: "33.0.0"
          channel: 2
        platforms:
        - version: 33
          channel: 0
        platform_tools:
          channel: 0
        ndk:
        - version: "25.0.8775105"
          channel: 0
        - version: "25.1.8937393"
          channel: 0
        cmake:
        - version: "3.22.1"
          channel: 0
        - version: "3.6.4111459"
          channel: 0
      plugdev_users:
        - stevens
        - tina
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
