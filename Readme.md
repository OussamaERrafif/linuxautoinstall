# AutoInstall Configuration for Jenkins

This `autoinstall.yaml` file is designed for an unattended installation of a Linux system (e.g., Ubuntu) with a pre-configured Jenkins environment. Below is a breakdown of each section within the configuration file.

## Configuration Breakdown

### 1. `version`
- **`version: 1`**
  - Specifies the version of the autoinstall configuration format being used.

### 2. `identity`
- **`hostname: jenkins`**
  - Sets the hostname of the system to `jenkins`.
  
- **`username: jenkins`**
  - The default username is set to `jenkins`.
  
- **`password: "$6$rounds=4096$your_salt$your_encrypted_password_here"`**
  - Encrypted password for the `jenkins` user. It uses SHA-512 encryption. Replace `your_salt` and `your_encrypted_password_here` with the actual salt and encrypted password.

- **`ssh: install-server: yes`**
  - Installs the SSH server on the system to allow remote connections.

### 3. `locale`
- **`locale: en_US.UTF-8`**
  - Sets the system locale to `en_US.UTF-8` (English, United States).

### 4. `timezone`
- **`timezone: Africa/Casablanca`**
  - Sets the system timezone to `Africa/Casablanca`.

### 5. `keyboard`
- **`layout: fr`**
  - Configures the keyboard layout to French (`fr`).
  
- **`variant: azerty`**
  - Sets the keyboard variant to `azerty`.

### 6. `storage`
- **`layout:`**
  - **`name: lvm`**: Uses LVM (Logical Volume Manager) for partitioning.
  - **`match:`**
    - **`size: largest`**: Applies the partition layout to the largest available disk.
  - **`wipe: superblock-recursive`**: Wipes existing data on the disk recursively, including superblocks.

### 7. `packages`
- Installs the following essential packages:
  - `build-essential`: Tools for compiling software.
  - `git`: Version control system.
  - `curl`: Command-line tool for transferring data with URLs.
  - `vim`: Text editor.
  - `openjdk-11-jdk`: Java Development Kit (JDK) version 11.
  - `maven`: Build automation tool for Java projects.
  - `docker.io`: Docker engine.
  - `python3`: Python 3 interpreter.
  - `python3-pip`: Package installer for Python 3.

### 8. `user-data`
- **`disable_root: false`**
  - Enables the root account. The root user is not disabled in this setup.

### 9. `network`
- Configures network settings:
  - **`ethernets:`**
    - **`eth0:`**
      - **`dhcp4: true`**: Enables DHCP for IPv4 on the `eth0` interface.
      - **`dhcp-identifier: mac`**: Uses the MAC address for DHCP identification.
  - **`version: 2`**: Specifies the version of the network configuration.

### 10. `late-commands`
- Executes custom commands at the end of the installation:
  - **`echo "jenkins ALL=(ALL) NOPASSWD:ALL" > /target/etc/sudoers.d/jenkins`**
    - Grants the `jenkins` user passwordless sudo access.
    
  - **`iptables -I INPUT -p tcp --dport 22 -j ACCEPT`**
    - Configures firewall to accept incoming SSH connections on port 22.
    
  - **`iptables -I INPUT -s 0.0.0.0/0 -j ACCEPT`**
    - Opens all incoming traffic (not recommended for production environments without further security configurations).

## Usage

This configuration file is meant to be used with cloud-init for automating the installation and setup of a Jenkins server. Ensure that you replace the placeholders in the password field with your actual encrypted password before use.

## Notes

- **Security**: The configuration opens all incoming traffic, which can be a significant security risk. Adjust firewall rules as necessary for your environment.
- **Password**: Remember to replace the password field with your actual encrypted password.
- **Customization**: Feel free to adjust the timezone, locale, keyboard layout, and installed packages according to your needs.
