# Resizing the Persistent Disk on the fly

## Add more space to the disk

4GB more for example

```
$ qemu-img resize vm/barge-data.qcow2 +4GB
Image resized.
```

## Re-partition the disk

```
$ make up
Booting up...
$ make ssh
barge-xhyve: running on 192.168.64.2
bargee@192.168.64.2's password:
Welcome to Barge 2.0.2, Docker version 1.9.1, build 66c06d0-stripped
[bargee@barge ~]$ (echo d; echo 1; echo n; echo p; echo 1; echo; echo; echo w) | sudo fdisk /dev/vda
[bargee@barge ~]$ sudo reboot
reboot[332]: Executing shutdown scripts in /etc/init.d
Stopping crond... OK
docker[340]: Loading /etc/default/docker
docker[340]: Stopping Docker daemon
Stopping sshd... OK
Saving random seed... done.
reboot[332]: reboot
Connection to 192.168.64.2 closed by remote host.
```

## Resize the disk after reboot

```
$ make ssh
barge-xhyve: running on 192.168.64.2
bargee@192.168.64.2's password:
Welcome to Barge 2.0.2, Docker version 1.9.1, build 66c06d0-stripped
[bargee@barge ~]$ sudo resize2fs /dev/vda1
resize2fs 1.42.13 (17-May-2015)
Filesystem at /dev/vda1 is mounted on /mnt/vda1; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 1
The filesystem on /dev/vda1 is now 1852672 (4k) blocks long.

```

Done.
