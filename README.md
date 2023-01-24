# ansible-distrohopper
---
Ansible playbook and roles for installing packages on different linux distros.  Have tested on Ubuntu, Fedora, openSUSE and Arch

![Shallow Backup GIF Demo](img/distrohopper.gif)


### Prerequisites
---
Ansible


### Usage
---
The `site.yml` playbook runs three different roles installing linux packages from distro repos, flatpak and VScode (Microsoft repo).

Variable yes/no prompts are used at the start of the `site.yml` playbook for each of these roles, to confirm whether or not to run each of these roles. 

These roles will install linux packages as listed in the `{package manager}_package_vars.yml` files under the `vars` directory.  (Refer to "Vars" section below).


### Run playbook
---
```bash
ansible-playbook -i inventory site.yml -K
```
`-i` specify inventory file
`-K` ask for privilege escalation password


### Inventory File
---
The hosts in `site.yml` file is set to "localhost" which is specified in the `inventory` file.  If running ansible playbook for a remote system change the hosts in `site.yml` file to "remote" and set the ip address and user name in the inventory file under "remote".  A commented out example is currently shown in the `inventory` file.


### Vars
---
Under the `vars` directory there are six `{package manager}_package_vars.yml` files that contain a list of packages to be installed.  One file for each package manager (dnf, zypper, apt and pacman), one for flatpak packages and one for snap packages.

The ansible playbook will skip `{package manager}_package_vars.yml` files that don't relate to the package manager used by the linux distro.  For example if running the ansible playbook for a Ubuntu system the playbook will skip the list of packages in the dnf, zypper and pacman `{package manager}_package_vars.yml` files. 

Example package names are currently listed in the `{package manager}_package_vars.yml` files.


### Roles
---

#### Distro Packages
This role will determine which package manager is used by the linux distro (dnf, zypper, apt or pacman) and install the packages listed in the `{package manager}_package_vars.yml` file under the `vars` directory.  The distro packages are installed using the "package" ansible module, but the reason the `{package manager}_package_vars.yml` files are split into package managers is because packages can vary by name between the distros.

#### Flatpak Packages
This role will install flatpak / check flatpak is installed, add flathub repo and install the packages listed in the `flatpak_package_vars.yml`.

#### VScode Packages
This role will enable Microsoft repo and install VScode.  This role will only work on linux distros with dnf, zypper and apt package managers.

