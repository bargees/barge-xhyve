# Barge running on xhyve hypervisor

This is a toolbox to run [Barge OS](https://github.com/bargees/barge-os) on xhyve hypervisor easily.

For VirtualBox or QEMU, see https://github.com/bargees/barge-packer.

## Features

- [Barge OS](https://github.com/bargees/barge-os)
- Disable TLS
- Expose the official IANA registered Docker port 2375
- Support NFS synced folder: $HOME is NFS-mounted on the Barge VM.

## Requirements

- [xhyve](https://github.com/mist64/xhyve)
  - Mac OS X Yosemite 10.10.3 or later
  - A 2010 or later Mac (i.e. a CPU that supports EPT)

## Caution

- **Kernel Panic** will occur on booting, if VirtualBox (< v5.0) has run before.
- Pay attention to **exposing the port 2375 without TLS**, as you see the features.

## Installing xhyve

```
$ git clone https://github.com/mist64/xhyve
$ cd xhyve
$ make
$ cp build/xhyve /usr/local/bin/    # You may require sudo
```

or

```
$ brew install xhyve
```

## Setting up Barge images and tools

```
$ git clone https://github.com/bargees/barge-xhyve
$ cd barge-xhyve
$ make init
```

## Booting Up

```
$ sudo ./xhyverun.sh [<dir>]

Welcome to Barge barge /dev/ttyS0
barge login: 
```

or

```
$ [SHARED_FOLDER=<dir>] make up    # You may be asked for your sudo password
Booting up...
```

- On Terminal.app: This will open a new window, then you will see in the window as below.
- On iTerm.app: This will split the current window, then you will see in the bottom pane as below.

```
Welcome to Barge barge /dev/ttyS0
barge login: 
```

## Logging In

- ID: bargee
- Password: bargee (in most instances you will not be prompted for a password)

```
$ make ssh
barge-xhyve: running on 192.168.64.2
bargee@192.168.64.2's password: 
Welcome to Barge 2.13.0, Docker version 1.10.3, build 20f81dd
[bargee@barge ~]$ 
```

## Shutting Down

Use `halt` command to shut down in the VM:

```
[bargee@barge ~]$ sudo halt
halt[324]: Executing shutdown scripts in /etc/init.d
Stopping crond... OK
docker[332]: Loading /etc/default/docker
docker[332]: Stopping Docker daemon
Stopping sshd... OK
Saving random seed... done.
halt[324]: halt
[bargee@barge ~]$ reboot: System halted
$ 
```

or, use `make halt` on the host:

```
$ make halt
barge-xhyve: running on 192.168.64.2
bargee@192.168.64.2's password:
halt[326]: Executing shutdown scripts in /etc/init.d
Stopping crond... OK
docker[334]: Loading /etc/default/docker
docker[334]: Stopping Docker daemon
Stopping sshd... OK
Saving random seed... done.
halt[326]: halt
Connection to 192.168.64.2 closed by remote host.
Shutting down...
```

## Using Docker

You can simply run Docker within the VM. However, if you install the Docker client on the host, you can use Docker commands natively on the host Mac. Install the Docker client as follows:

```
$ curl -L https://get.docker.com/builds/Darwin/x86_64/docker-latest -o docker
$ chmod +x docker
$ mv docker /usr/local/bin/    # You may require sudo
```

Alternatively install with Homebrew:

```
$ brew install docker
```

Then, in the VM, or on the host if you have installed the Docker client:

```
$ make env
barge-xhyve: running on 192.168.64.2
export DOCKER_HOST=tcp://192.168.64.2:2375;
unset DOCKER_CERT_PATH;
unset DOCKER_TLS_VERIFY;
$ eval $(make env)
barge-xhyve: running on 192.168.64.2

$ docker info
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 1.10.3
Storage Driver: overlay
 Backing Filesystem: extfs
Execution Driver: native-0.2
Logging Driver: json-file
Plugins:
 Volume: local
 Network: null host bridge
Kernel Version: 4.14.125-barge
Operating System: Barge 2.13.0
OSType: linux
Architecture: x86_64
CPUs: 1
Total Memory: 995.8 MiB
Name: barge
ID: CJOC:6SXR:3WOA:4LCE:AG6K:J22I:UYSN:T7M7:UF7E:JN3B:6J7Y:F776
Debug mode (server): true
 File Descriptors: 10
 Goroutines: 15
 System Time: 2019-06-12T05:04:50.354063203Z
 EventsListeners: 0
 Init SHA1:
 Init Path: /opt/bin/docker
 Docker Root Dir: /mnt/data/var/lib/docker
Username: ailispaw
Registry: https://index.docker.io/v1/
```

## Upgrading Barge

When Barge is upgraded and barge-xhyve is updated,

```
$ git pull origin master
$ make upgrade
$ make up
$ make ssh
[bargee@barge ~]$ sudo cp /etc/init.d/start.sh /etc/init.d/start.old
[bargee@barge ~]$ sudo wget -qO /etc/init.d/start.sh https://raw.githubusercontent.com/bargees/barge-xhyve/master/contrib/configs/start.sh
[bargee@barge ~]$ sudo chmod +x /etc/init.d/start.sh
[bargee@barge ~]$ sudo reboot
```

## Resources

- /var/db/dhcpd_leases
- /Library/Preferences/SystemConfiguration/com.apple.vmnet.plist
  - Shared_Net_Address
  - Shared_Net_Mask
