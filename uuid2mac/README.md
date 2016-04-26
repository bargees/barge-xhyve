# Get Mac Address and IP Address from UUID

License: BSD as well as [xhyve](https://github.com/mist64/xhyve)

config.mk and uuid.h are from xhyve and most of main.c is from xhyve, too.

```
$ git clone https://github.com/bargees/barge-xhyve
$ cd barge-xhyve
$ make uuid2ip
/Library/Developer/CommandLineTools/usr/bin/make -C uuid2ip
cc main.c
ld uuid2mac.sym
dsym uuid2mac.dSYM
strip uuid2mac
```

```
$ uuid2ip/build/uuid2mac
Usage: uuid2ip/build/uuid2mac <UUID>
```

```
$ uuid2ip/mac2ip.sh
Usage: mac2ip.sh <mac_address>
```
