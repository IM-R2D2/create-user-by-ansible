# Create User Playbook

This repository contains an Ansible playbook designed to create a new user with sudo privileges across different Linux distributions. The playbook handles SSH key management, ensures user creation, and verifies SSH connectivity after setup.

## Features
- Automatically detects the operating system family (Debian, RedHat, etc.) to determine appropriate user group privileges (`sudo` or `wheel`).
- Create a new user with sudo or wheel privileges, depending on the Linux distribution.
- Manage user home directory, including creating necessary folders.
- Copy SSH public keys to the server and set up authorized keys for SSH access.
- Verify SSH connectivity after setup to ensure everything works as expected.

## Requirements
- Ansible 2.9+
- SSH access to the target hosts
- `vaults.yml` file to store encrypted passwords (password: `1234`)
- `password.txt` file containing the vault password (`1234`)- Properly configured `ansible.cfg` and inventory files.

## Files Overview

### Password File: `password.txt`
Contains the vault password (`1234`) used for decrypting `vaults.yml`.

### Playbook: `create_user.yml`
- Detects the operating system family to assign the correct user group (`sudo` for Debian-based systems, `wheel` for RedHat-based systems).
The main playbook creates a user on the target servers:
- The playbook uses different groups, such as `front_servers` and `bkend_servers`, defined in the inventory.
- The new user is created with specified privileges (`sudo` for Debian-based systems, `wheel` for RedHat-based systems).
- Public keys are copied to the server for SSH access.

### Configuration: `ansible.cfg`
Specifies Ansible settings such as:
- `host_key_checking = false`: Disables host key checking for convenience.
- `inventory = ./inventory`: Specifies the inventory file to be used.

### Inventory: `inventory`
Contains the list of target servers:
- Defines `front_servers` and `bkend_servers` groups, each with individual server IP addresses.

### Encrypted Variables: `vaults.yml`
Contains encrypted credentials, such as:
- User passwords
- Admin credentials

The vault file is encrypted for security with a password (`1234`).

### Host Variables: `server1.yml`
Host-specific variables for `server1` include:
- `ansible_user`, `ansible_password`, and other connection settings.
- Private SSH key file (`./ssh/server`).

### Group Variables: `front_servers.yml` and `bkend_servers.yml`
Contains variables that apply to specific groups of servers.

## Usage

1. **Clone the Repository**
   ```bash
   git clone <repository-url>
   cd create-user-by-ansible
   ```

2. **Edit Inventory and Variables**
   Update `inventory` and variable files to match your environment.

3. **Run the Playbook**
   Run the playbook with the following command:
   ```bash
   ansible-playbook create_user.yml --vault-password-file=password.txt
   ```
   This will use the vault password (`1234`) stored in `password.txt` to decrypt sensitive information.

## Notes
- Ensure that SSH keys are properly set up and available in the `./ssh` directory.
- Modify the host and group variable files as needed to match the environment settings.

## Security
- The `vaults.yml` file is encrypted to protect sensitive information.
- SSH key-based authentication is used to improve security when accessing servers.

## License
This project is licensed under the MIT License.

