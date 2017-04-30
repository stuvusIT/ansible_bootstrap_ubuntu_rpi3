# bootstrap-ubuntu-rpi3

This role sets up a Raspberry Pi 3 SD card with a minimal Ubuntu configuration for use with Ansible.
Only a SSH server is installed along with Python.
Also, the network is configured with a static IP address.

This role can not run on the Raspberry Pi directly, instead it's supposed to be run on the control machine.
The SD card must be inserted into the control machine and this role will install everything on it.
While doing this, qemu-user-static is used.
It must be installed and configured in the binfmt.

This role is not capable of updating an existing Raspberry Pi without reinstalling.

You need internet access on the control machine to download the kernel, the firmware, and Ubuntu.

You need internet access on the server to clone Vault.

## Requirements

A dpkg- or pacman-based Linux distribution on the host machine.
qemu-user-static must be configured in binfmt.

On Arch, you also need the Ubuntu keyring package from the AUR.

## Role Variables

#### Global

| Name               | Required                 | Default | Description                                                                                   |
|--------------------|:------------------------:|---------|-----------------------------------------------------------------------------------------------|
| `global_cache_dir` | :heavy_check_mark:       |         | Path where files are downloaded and extracted. Also, the SD card is mounted in a subdirectory |
| `global_cores`     | :heavy_multiplication_x: | `1`     | Amount of CPU cores in the control machine to build with                                      |


#### General

| Name                  | Required                 | Default                        | Description                                   |
|-----------------------|:------------------------:|--------------------------------|-----------------------------------------------|
| `bootstrap_device`    | :heavy_check_mark:       |                                | The SD card to install on (`/dev` path)       |
| `bootstrap_qemu`      | :heavy_multiplication_x: | `/usr/bin/qemu-aarch64-static` | Path to the static aarch64 qemu on your sytem |
| `bootstrap_boot_size` | :heavy_multiplication_x: | `1GiB`                         | Size of `/boot` on the Raspberry Pi           |

#### Versions

| Name                       | Required                 | Default   | Description                                              |
|----------------------------|:------------------------:|-----------|----------------------------------------------------------|
| `bootstrap_kernel_version` | :heavy_multiplication_x: | `4.11.y`  | Version of the Raspberry Pi kernel to build (git branch) |
| `bootstrap_extraversion`   | :heavy_multiplication_x: | `+stuvus` | Extraversion to append on the kernel                     |
| `bootstrap_ubuntu_release` | :heavy_multiplication_x: | `yakkety` | Ubuntu release name to install                           |

#### User

| Name                      | Required                 | Default | Description                                        |
|---------------------------|:------------------------:|---------|----------------------------------------------------|
| `bootstrap_root_key`      | :heavy_multiplication_x: |         | Public SSH key to add to `authorized_keys` of root |
| `bootstrap_root_password` | :heavy_multiplication_x: |         | `crypt(3)` password hash to set for the root user  |

#### Network

| Name                | Required           | Default | Description                                   |
|---------------------|:------------------:|---------|-----------------------------------------------|
| `bootstrap_address` | :heavy_check_mark: |         | Address of the Raspberry Pi network interface |
| `bootstrap_netmask` | :heavy_check_mark: |         | Netmask of the Raspberry Pi network interface |
| `bootstrap_gateway` | :heavy_check_mark: |         | Gateway of the Raspberry Pi network interface |

## Dependencies

None

## Example Playbook

```
- hosts: localhost
  roles:
    - role: bootstrap-ubuntu-rpi3
      global_cores: 8
      bootstrap_device: /dev/sdh
      bootstrap_boot_size: "20%"
      bootstrap_root_key: "ssh-ed25519 AAAAC3NzaC1lZDI1N...."
      bootstrap_address: 192.168.0.25
      bootstrap_netmask: 255.255.255.0
      bootstrap_gateway: 192.168.0.1
```

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

## Author Information

- [Janne Heß](https://github.com/dasJ)
