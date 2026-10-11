# QEMU/KVM Installer

A Bash script to automate the installation and initial configuration of QEMU/KVM virtualization on Debian and Ubuntu.

The script installs the required virtualization packages, adds a specified user to the `libvirt` and `kvm` groups, enables and starts the `libvirtd` service, and configures the default libvirt virtual network.

## Features

- **OS detection** — Verifies that the system is running Linux and identifies Debian or Ubuntu.
- **User validation** — Checks whether the specified user exists.
- **Repository update** — Updates APT package indexes before installation.
- **Automated installation** — Installs QEMU/KVM and related virtualization tools.
- **Group management** — Adds the specified user to the `libvirt` and `kvm` groups.
- **Service management** — Enables and starts the `libvirtd` service.
- **Network configuration** — Starts the default libvirt network and enables its autostart.
- **Colored terminal output** — Provides readable status messages, progress information, and error reporting.
- **Command-line options** — Supports username selection and help output.

## Supported Operating Systems

- Debian GNU/Linux
- Ubuntu

Other Linux distributions are not supported by the current implementation.

## Installed Packages

The script installs the following packages using APT:

| Package | Purpose |
|---|---|
| `qemu-kvm` | QEMU/KVM virtualization |
| `qemu-utils` | QEMU disk image utilities |
| `virt-manager` | Graphical virtual machine management |
| `libvirt-daemon-system` | System-level libvirt virtualization service |
| `libvirt-clients` | Command-line tools for managing libvirt |
| `libguestfs-tools` | Tools for inspecting and modifying virtual machine disk images |
| `virtinst` | Virtual machine provisioning utilities |
| `virt-top` | Virtual machine monitoring |
| `virtiofsd` | Virtio filesystem sharing support |
| `virt-viewer` | Graphical console access for virtual machines |
| `ovmf` | UEFI firmware for virtual machines |
| `swtpm` | Software TPM emulator |
| `cpu-checker` | CPU virtualization capability checks |
| `hwloc` | Hardware topology discovery and analysis |

Package availability may vary depending on the Debian or Ubuntu release and enabled repositories.

## Requirements

Before running the script, ensure that:

- The operating system is Debian or Ubuntu.
- APT repositories are configured correctly.
- The specified user already exists.
- The system has a CPU and firmware configuration that supports hardware virtualization.
- The script is executed with root privileges.
- Internet access is available to download packages.

## Installation

Clone the repository:

```bash
git clone https://github.com/morteza-j/qemu-kvm-installer.git
```

Enter the project directory:

```bash
cd qemu-kvm-installer
```

Make the script executable:

```bash
chmod u+x qemu-kvm-installer.sh
```

## Usage

The script requires a username to configure access to libvirt and KVM.

### Basic Usage

Run the installer as root and specify the target user:

```bash
./qemu-kvm-installer.sh --username <username_here>
```

Replace `mrt` with the username of the account that will manage virtual machines.

### Command-Line Options

| Option | Description |
|---|---|
| `-u`, `--username` | Specifies the existing user to configure |
| `-h`, `--help` | Displays the usage information |

Short-option example:

```bash
./qemu-kvm-installer.sh -u <username_here>
```

Display help:

```bash
./qemu-kvm-installer.sh --help
```

## What the Script Does

The installation process follows these steps:

1. Validates the supplied username.
2. Detects the operating system and checks distribution support.
3. Updates APT package indexes.
4. Installs the virtualization packages.
5. Adds the specified user to the `libvirt` and `kvm` groups.
6. Enables and starts `libvirtd`.
7. Checks whether the default libvirt network exists.
8. Starts the default network and enables its autostart.
9. Reports successful completion if all operations finish without errors.

The script exits when an operation handled by its error-checking functions fails.

## Post-Installation

### Apply Group Membership Changes

After the installation completes, log out and log back in so the user's new group memberships take effect.

Check group membership:

```bash
id <username_here>
```

Replace `<username_here>` with your configured username.

### Verify Virtualization Support

Check CPU virtualization support:

```bash
kvm-ok
```

Check the libvirt service:

```bash
systemctl status libvirtd
```

List available virtual machines:

```bash
virsh list --all
```

Check the default virtual network:

```bash
virsh net-list --all
```

Inspect the default network configuration:

```bash
virsh net-dumpxml default
```

## Important Notes

- **Root privileges are required.** The script does not automatically elevate privileges.
- **Group membership is security-sensitive.** Membership in the `libvirt` group can grant substantial control over virtual machines and, depending on configuration, the host system. Only grant it to trusted users.
- **Hardware virtualization must be available.** Check BIOS/UEFI settings and CPU support if KVM cannot initialize.
- **The default network must already exist.** The current script starts the existing `default` network; it does not create the network if it is missing.
- **Existing configurations are not comprehensively audited.** Review your libvirt networking and storage configuration before using the system for production workloads.
- **APT changes are system-wide.** Review repository configuration and package availability before running the installer on production systems.

## Limitations

The script performs installation and basic service/network setup. It does not:

- Configure advanced virtual networking or VLANs.
- Create virtual machines or storage pools.
- Configure host firewall rules.
- Tune CPU pinning, huge pages, or NUMA policies.
- Validate IOMMU or PCI passthrough readiness.
- Configure libvirt authentication policies or perform a complete security hardening procedure.

Additional configuration may be required for production virtualization hosts.

## Contributing

Bug reports, improvements, and suggestions are welcome.

Please keep contributions focused, maintainable, and compatible with the stated operating systems.

## License

This project is licensed under the **GNU General Public License v3.0 (GPL-3.0)**.

See the [`LICENSE`](LICENSE) file for the full license text.

## Author

**J. Morteza**

Created on September 10, 2025.
