# Ansible Act
An Ansible role for managing local Github workflow runner [act](https://github.com/nektos/act)


### Requirements

This role requires the `ansible.utils` collection be installed from Ansible-Galaxy via:

```shell
ansible-galaxy collection install ansible.utils
```

It then also requires the `jsonschema` Python package be installed. It can be installed via pip:

```shell
pip3 install jsonschema
```

### Role Variables

All variables are optional unless otherwise marked:

```yaml
act:
  remove:       [bool]    Indicates if every single act version should be removed along with any symlinks to the path
  trim:         [bool]    Indicates that any version found under the default installation path which isn't listed in the [versions] list
  os:           [string]  Is required if [architecture] is defined. Defines the operating system type to download an artifact for. Can by default take any of the following values ["linux", "darwin"]. If both [os] and [architecture] are omitted, the task defaults to [os="linux"] and [architecture="x86_64"]
  architecture: [string]  Is required if [os] is defined. Defines the architecture to download an artifact for. By default can take any of the following values ["x86_64", "riscv64", "i386", "armv6", "armv7", "arm64"] depending on if the [os] type supports it. If both [os] and [architecture] are omitted, the task defaults to [os="linux"] and [architecture="x86_64"]
  default:      [string]  The version number that should be added to the path.
  versions:     [array]   A array of versions to install
    - version:  [string]  [required] The numerical part only of version name to install.
      checksum: [string]  [required] The accompanying checksum for the version to install. Is required to be prefixed with the hash algorithm name, for example "sha256:<checksum>"
```


#### Defaults

These are the role variables used to define default values to use. These can be modified if different behavior is requested.

`hth_default_act_installation_path`:

Defines the default absolute path location to where versions should be extracted to. Defaults to `"/opt/act"`

`hth_default_act_path_symlink_path`:

Defines where the default version should be symlinked to. Defaults to `"/usr/bin/act"`

`hth_default_act_path_symlink_owner`:

Defines the default user who will own the default version symlink. Defaults to `"root"`

`hth_default_act_path_symlink_group`:

Defines the default user group who will own the default version symlink. Defaults to `"root"`

`hth_default_act_installation_owner`:

Defines the default user who will own the version installation directory. Defaults to `"root"`

`hth_default_act_installation_group`:

Defines the default user group who will own the version installation directory. Defaults to `"root"`

`hth_default_act_os_linux_architectures`:

Defines an array of architecture names that are officially supported on Linux operating systems. Is used when `act.os` is set to `linux`.

Defaults to ["x86_64", "riscv64", "i386", "armv6", "armv7", "arm64"].

Officially supported values can be found on [act's Github release page](https://github.com/nektos/act/releases)

`hth_default_act_os_darwin_architectures`:

Defines an array of architecture names that are officially supported on Darwin operating systems. Is used when `act.os` is set to `darwin`.

Defaults to ["x86_64", "arm64"].

Officially supported values can be found on [act's Github release page](https://github.com/nektos/act/releases)


### Setup

Before the role can be used it needs to be added to the machine running the playbook, and as of writing this, this role is not hosted on Ansible-Galaxy only on Github.

1. Create a `requirements.yml` file in the root directory of the playbook being worked on.

2. Add the following definition inside the `requirements.yml` file:

```yml
- name: hth-act
  src: https://github.com/hrafnthor/ansible-act.git
  scm: git
```

3. Install the requirements by executing

```shell
ansible-galaxy install -r .requirements.yml
```

This will allow any playbook run from this machine to use the role `hth-act`


### Example Playbook


```yaml
- hosts: all
    vars:
    - act:
      remove: false
      trim: true
      os: "linux"
      architecture: "x86_64"
      default: "0.2.81"
      versions:
      - version: "0.2.81"
        checksum: "sha256:2198d2b3b6f5751751c829475255d99f9558d0043970b4a043e3af16715fa0cf"
      - version: "0.2.79"
        checksum: "sha256:b4c14df1a6e713a63e8e153534091a2e0d93a73538da301bd3aaa3c1f2a6f571"
  roles:
     - hth-act
```
