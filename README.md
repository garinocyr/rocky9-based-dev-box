# Vagrant box with Debian Bookworm64 for development purpose

## Description

* Provides a Vagrant box based on [Debian Bookworm64](https://app.vagrantup.com/debian/boxes/bookworm64).
* The box is run on a Windows 11 host.

### Features

|Feature    |Value        |
|:----------|:------------|
|Provider   |VirtualBox   |
|Provisioner|Ansible local|

### VirtualBox provider features

|Feature  |Value        |
|:--------|:------------|
|cpus     |2            |
|memory   |1024 MB      |
|vram     |128 MB       |
|clipboard|bidirectional|

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

## Running the Vagrant box from a Windows console

### First time

* Run:
```sh
> git clone https://github.com/garinocyr/debian-bookworm64-based-dev-box.git
```
* Create a Vagrant-synced-folder.config.yml yaml file at the same location of the Vagrantfile file with content (given as an example):
```yaml
host_path: "~/git/projects"
guest_path: "/home/vagrant/projects"
```
* Run:
```sh
> cd debian-bookworm64-based-dev-box
> vagrant up
```

### Next time

* To start the box, run:
```sh
> vagrant up
```

* To reprovision the box, run:
```sh
> vagrant up --provision
```

* To reload the box without stopping it, run:
```sh
> vagrant reload
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

## ⚠️ Danger zone

### Destroying the Vagrant box

* From a Windows console, run:
```sh
> vagrant destroy
```

## Known limitations

### Vagrant box hangs after guest additions upgrade

#### Description

* After the guest has upgraded to the lastest guest additions, it hangs...

#### Resolution / Workaround

* Send a shutdown signal from VirtualBox Manager.
* `vagrant up` the Vagrant box from a Windows console.

### Vagrant gathered an unknown ansible version and falls back on ... 1.8

#### Description

* At guest startup, there is the following error:
  * "Vagrant gathered an unknown ansible version and falls back on ... 1.8".

#### Resolution / Workaround

* No resolution so far, cf. https://github.com/hashicorp/vagrant/issues/13234.
