# Create User Playbook

This repository contains an Ansible playbook for creating a new user with sudo privileges on target Linux servers, managing SSH key generation, and verifying SSH connectivity.

## Features
- Automatically detects the operating system family (Debian, RedHat, etc.) to determine appropriate user group privileges (`sudo` or `wheel`).
- Creates a new user with sudo or wheel privileges, depending on the system.
- Generates SSH key pairs on the remote host, configures authorized_keys, and copies keys to the local machine for backup.
- Verifies SSH connectivity from localhost to the new user on the remote host using the generated keys.
- Cleans up temporary variables and files after the playbook run.

## Requirements
- Ansible 2.9+
- SSH access to the target hosts
- `vaults.yml` file to store encrypted passwords (password: `1234`)
- `password.txt` file containing the vault password (`1234`)- Properly configured `ansible.cfg` and inventory files.

## Files Overview

### Password File: `password.txt`
Contains the vault password (`1234`) used for decrypting `vaults.yml`.

### Playbook: `create_user.yml`
The main playbook for user creation, SSH key generation, and verification:
- Prompts for input values like key_name, key_comment, and protect_phrase to customize SSH key details.
- Saves key configurations and other settings temporarily in extra_vars.yml.
- Runs tasks in blocks for modularity: user creation, SSH directory setup, key generation, and local key backup.
 Cleans up extra_vars.yml after the playbook completes to maintain security.

### Configuration: `ansible.cfg`
Specifies Ansible settings such as:
- `host_key_checking = false`: Disables host key checking for convenience.
- `inventory = ./inventory`: Specifies the inventory file to be used.

### Inventory: `inventory`
Contains the list of target servers:
- Defines `front_servers` and `bkend_servers` groups, each with individual server IP addresses.

### Encrypted Variables: `vaults.yml`
Contains encrypted credentials, such as:
- User and admin passwords
- The vault file is encrypted to protect sensitive information, with a password file (password.txt).

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
   ansible-playbook create_user.yml --ask-vault-pass
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

