KERNEL   := bzImage
INITRD   := initrd
CMDLINE  := quiet loglevel=3 barge.switch_root
ISO      := barge.iso
HDD      := barge-data.img
UUID     := A01FB25C-3A19-4759-A47A-2E353E51807D
USERNAME := bargee
PASSWORD := bargee

all: $(KERNEL) $(INITRD) $(HDD)

upgrade: 
	$(RM) $(ISO)
	$(MAKE)

$(KERNEL) $(INITRD): $(ISO)
	hdiutil mount $(ISO)
	cp /Volumes/BARGE/boot/$@ . && sync
	hdiutil unmount /Volumes/BARGE

$(ISO):
	curl -L https://github.com/bargees/barge-os/releases/download/2.3.2/$(@F) -o $@

$(HDD): barge-data.tar.gz
	tar zxmvf $<

barge-data.tar.gz:
	curl -L https://github.com/bargees/barge-xhyve/releases/download/2.0.0/$(@F) -o $@

xhyve_uuid:
	@echo $(UUID)

xhyve_kernel:
	@echo $(CURDIR)/$(KERNEL)

xhyve_initrd:
	@echo $(CURDIR)/$(INITRD)

xhyve_cmdline:
	@echo $(CMDLINE)

xhyve_hdd:
	@echo $(CURDIR)/$(HDD)

username:
	@echo $(USERNAME)

password:
	@echo $(PASSWORD)

ssh_args:
	@echo "-q -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no"

clean:
	$(RM) $(KERNEL) $(INITRD) $(HDD)
	$(RM) $(ISO) barge-data.tar.gz
	$(RM) .mac_address

.PHONY: all upgrade xhyve_uuid xhyve_kernel xhyve_cmdline xhyve_hdd \
	username password ssh_args clean
