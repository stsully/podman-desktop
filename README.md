## Podman-desktop (for MacOS)
This project attempts to replace docker-desktop with a working, open-source solution that still "lives" on your mac. Running vagrant + virtualbox allows us to run a Linux VM with podman, which serves a local REST API for container operations. podman on the macOS can be configured to interact with this VM, and effectively replace docker.

### Pre-requisites
 * Oracle Virtualbox >= 6.1
   * [download here](https://download.virtualbox.org/virtualbox/6.1.26/VirtualBox-6.1.26-145957-OSX.dmg)
 * Hashicorp Vagrant >= 2.2.18
   * `brew install vagrant`
 * podman >= 3.3
   * `brew install podman`

### Installation
1. install pre-reqisite software above.

2. Drop your ssh pub key into the ssh_key.pub file in the vm directory
```
cp ~/.ssh/id_rsa.pub ./vm/ssh_key.pub
```
3. Create and provision the fedora33 VM using Vagrant. Note you must be in the `vm` directory where the Vagrantfile lives, or you can export VAGRANT_CWD environment variable to point to your Vagrantfile:
```
cd vm
vagrant up
```
4. Vagrant will start the machine, and put your ssh key (if you followed step 2. above) into root's .ssh/authorized_keys file. The VM will start the podman REST API, which you can connect over ssh. To set the local MacOS podman client to use this, export the following ENV Variables:
```
CONTAINER_SSHKEY=$HOME/.ssh/id_rsa
CONTAINER_HOST=ssh://root@127.0.0.1:2222/run/podman/podman.sock
```

5. Test it out:
```
podman images
podman pull alpine
podman images
podman run -it alpine bash
```
@TODO: test building with Dockerfile
