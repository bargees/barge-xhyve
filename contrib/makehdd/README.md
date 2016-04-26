# Making a fresh persistent disk

## Make a blank disk image on Max OS X

4GB for example

```
$ dd if=/dev/zero of=vm/barge-data.img bs=1g count=4
4+0 records in
4+0 records out
4294967296 bytes transferred in 11.520751 secs (372802719 bytes/sec)
```

## Set up a persistent disk

- Boot it on xhyve
- Download and execute [makehdd.sh](https://github.com/bargees/barge-xhyve/blob/master/contrib/makehdd/makehdd.sh)

```
$ sudo ./xhyverun.sh

Welcome to Barge barge /dev/ttyS0
barge login: bargee
Password: 
Welcome to Barge 2.0.0, Docker version 1.9.1, build 66c06d0-stripped
[bargee@barge ~]$ wget https://raw.githubusercontent.com/bargees/barge-xhyve/master/contrib/makehdd/makehdd.sh
[bargee@barge ~]$ chmod +x makehdd.sh
[bargee@barge ~]$ sudo ./makehdd.sh
[bargee@barge ~]$ sudo fdisk -l

Disk /dev/vda: 4294 MB, 4294967296 bytes
64 heads, 32 sectors/track, 4096 cylinders
Units = cylinders of 2048 * 512 = 1048576 bytes

   Device Boot      Start         End      Blocks  Id System
/dev/vda1             956        4096     3216384  83 Linux
/dev/vda2               1         955      977904  82 Linux swap

Partition table entries are not in disk order
[bargee@barge ~]$ df
Filesystem           1K-blocks      Used Available Use% Mounted on
tmpfs                   921012     47192    873820   5% /
devtmpfs                506040         0    506040   0% /dev
tmpfs                   511672         0    511672   0% /run
cgroup                  511672         0    511672   0% /sys/fs/cgroup
/dev/vda1              2949500      4760   2767540   0% /mnt/vda1
overlay                2949500      4760   2767540   0% /etc
[bargee@barge ~]$ ls -l /etc/default/docker
-rw-r--r--    1 root     root            47 Apr 26 19:21 /etc/default/docker
[bargee@barge ~]$ ls -l /etc/init.d/start.sh
-rwxr-xr-x    1 root     root          1105 Apr 26 19:21 /etc/init.d/start.sh*
[bargee@barge ~]$ sudo halt
halt[309]: Executing shutdown scripts in /etc/init.d
Stopping crond... OK
docker[317]: Loading /etc/default/docker
docker[317]: Stopping Docker daemon
Stopping sshd... OK
Saving random seed... done.
halt[309]: halt
[bargee@barge ~]$ reboot: System halted
```

Done.
