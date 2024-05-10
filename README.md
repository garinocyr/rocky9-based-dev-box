# Vagrant box with Debian Bookworm64 for software development purpose

## Description

* Provides a Vagrant box based on [Debian Bookworm64](https://app.vagrantup.com/debian/boxes/bookworm64).
* The box is run on a Windows 11 host.

### Features

|Feature    |Value        |
|:----------|:------------|
|Provider   |VirtualBox   |
|Provisioner|Ansible local|

### Installed tools on guest

|Tool          |Version       |
|:-------------|:-------------|
|git           |1:2.39.2-1.1  |
|miniconda     |24.3.0-0      |
|podman        |4.3.1+ds1-8+b1|
|podman-compose|1.0.3-3       |

### Users on guest

|User   |Password|
|:------|:-------|
|vagrant|None    |

## Before running the Vagrant box

* Install [Vagrant 2.4.1](https://www.vagrantup.com/).
* Install Vagrant `vbguest` plugin:
  * From a Windows console, run:
```sh
> vagrant plugin install vagrant-vbguest
```
* Install [VirtualBox 7.0.18](https://www.virtualbox.org/).
* Install associated [VirtualBox Extension Pack 7.0.18](https://www.virtualbox.org/wiki/Downloads).
* Install [Git client for Windows 2.45.0](https://git-scm.com/download/win).

## Running the Vagrant box

* From a Windows console, run:
```sh
> git clone https://github.com/garinocyr/vm-dev-on-debian-bookworm64.git
> cd vm-dev-on-debian-bookworm64
> vagrant up
```

* If you need to reprovision the box, run:
```sh
> vagrant up --provision
```

## Connection to the Vagrant box with the `vagrant` user

* From a Windows console, run:
```sh
> vagrant ssh
```

## Stopping the Vagrant box

* From a Windows console, run:
```sh
> vagrant halt
```

## Danger zone

### Destroying the Vagrant box

* From a Windows console, run:
```sh
> vagrant destroy
```

## Known limitations

* After the guest has upgraded to the lastest guest additions, it hangs...
  * You need to reboot it via sending a signal from VirtualBox Manager and then `vagrant up` from a Windows console.
* At guest startup, there is the following error: 
  * "Vagrant gathered an unknown ansible version and falls back on ... 1.8".
  * No resolution so far, cf. https://github.com/hashicorp/vagrant/issues/13234.
