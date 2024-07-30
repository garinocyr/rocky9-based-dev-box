# Vagrant box with Debian Bookworm64 for development purpose

## Description

* Provides a Vagrant box based on [Debian Bookworm64](https://app.vagrantup.com/debian/boxes/bookworm64).
* The box is run on a Windows 11 host (one CPU 13th Gen Intel(R) Core(TM) i7-13650HX 2.60 GHz with 14 cores, 32 GB RAM).

### Features

|Feature    |Value        |
|:----------|:------------|
|Provider   |VirtualBox   |
|Provisioner|Ansible local|

### VirtualBox provider features

|Feature  |Value        |
|:--------|:------------|
|cpus     |4            |
|memory   |6144 MB      |
|vram     |128 MB       |
|clipboard|bidirectional|

### Installed tools on guest

|Tool          |Version            |
|:-------------|:------------------|
|git           |1:2.39.2-1.1       |
|miniconda     |24.3.0-0           |
|podman        |4.3.1+ds1-8+deb12u1|
|podman-compose|1.0.3-3            |

### Users on guest

|User   |Password|
|:------|:-------|
|vagrant|None    |

## Before running the Vagrant box

### Install [VirtualBox 7.0.18](https://www.virtualbox.org/)

### Install associated [VirtualBox Extension Pack 7.0.18](https://www.virtualbox.org/wiki/Downloads)

### Install [Vagrant 2.4.1](https://www.vagrantup.com/)

### Install Vagrant `vbguest` plugin

* `vbguest` plugin will detect higher installed guest additions (cf. `VirtualBox Extension Pack 7.0.18`) on host and will upgrade guest automatically at box startup time.
* From a terminal, run:
```sh
$ vagrant plugin install vagrant-vbguest
```

### Install [Git client for Windows 2.45.0](https://git-scm.com/download/win)

## Running the Vagrant box from a terminal

### First time

* Run:
```sh
$ git clone https://github.com/garinocyr/debian-bookworm64-based-dev-box.git
```
* Create a `Vagrant-synced-folder.config.yml` yaml file at the same location of the Vagrantfile file with content (given as an example):
```yaml
host_path: "~/git/projects"
guest_path: "/home/vagrant/projects"
```
* Run:
```sh
$ cd debian-bookworm64-based-dev-box
$ vagrant up
```
* The Vagrant `Debian Bookworm64` box comes with guest additions `6.0`. They will be upgraded to version `7.0.18` thanks to the `vbguest` plugin.

### Next time

* To start the box, run:
```sh
$ vagrant up
```

* To reprovision the box, run:
```sh
$ vagrant up --provision
```

* To reload the box without stopping it, run:
```sh
$ vagrant reload
```

## Connection to the Vagrant box with the `vagrant` user

### Default for development purpose (ex.: through the `VSCode remote` extension)

* Thing to do once, run:
```sh
$ vagrant ssh-config --host dev-box >> ~/.ssh/config
```
* This will add the following in the .ssh `config` file:
```sh
$ cat ~/.ssh/config
Host dev-box
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile C:/Users/my_windows_user/git/vagrant/debian-bookworm64-based-dev-box/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
  PubkeyAcceptedKeyTypes +ssh-rsa
  HostKeyAlgorithms +ssh-rsa
```
* Then, run:
```sh
$ ssh vagrant@dev-box
```
* If the error `Bad owner or permissions on C:\Users\my_windows_user/.ssh/config` happens, follow [stackoverflow / openssh-windows-bad-owner-or-permissions](https://stackoverflow.com/questions/49926386/openssh-windows-bad-owner-or-permissions/58275268#58275268). See below:

> "Just go to .ssh, right-click Properties, Security Tab, Advanced. DISABLE INHERITANCE, then click on the Administrator user (the one that is not you) and Remove them. Apply. Done." (Jason Hughes, Oct 7, 2019 at 18:32).

> "When you are disabling the inheritance you will be asked if you want to copy the current inherited access rights. Select yes and then continue by removing the other user as described above." (pabouk - Ukraine stay strong, Feb 11, 2021 at 20:10).

### Alternative for other cases

* From a terminal, run:
```sh
$ vagrant ssh
```

## Stopping the Vagrant box

* From a terminal, run:
```sh
$ vagrant halt
```

## ⚠️ Danger zone

### Destroying the Vagrant box

* From a terminal, run:
```sh
$ vagrant destroy
```

## Known limitations

### Vagrant box hangs after guest additions upgrade

#### Description

* After the guest has upgraded to the lastest guest additions, it hangs...

#### Resolution / Workaround

* Send a shutdown signal from VirtualBox Manager.
* `vagrant up` the Vagrant box from a terminal.

### Vagrant gathered an unknown ansible version and falls back on ... 1.8

#### Description

* At guest startup, there is the following error:
  * "Vagrant gathered an unknown ansible version and falls back on ... 1.8".

#### Resolution / Workaround

* No resolution so far, cf. https://github.com/hashicorp/vagrant/issues/13234.
