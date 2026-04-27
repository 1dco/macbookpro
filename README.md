# Overview

## Audio
For Audio certain thing need to be done.

Clone the Project here https://github.com/davidjo/snd_hda_macbookpro

Because CachyOS is using clang instead of gcc, we might need to modify the instruction.
and add LLVM=1 to the code.

```
# Skipping patch installation since dkms will do it
if [[ ! $dkms = true ]]; then

	if [ $PATCH_CIRRUS = true ]; then
		make PATCH_CIRRUS=1 LLVM=1
		make install PATCH_CIRRUS=1 LLVM=1

	else
		make KERNELRELEASE=$UNAME LLVM=1
		make install KERNELRELEASE=$UNAME LLVM=1

	fi
	echo -e "\ncontents of $update_dir/codecs/cirrus"
	ls -lA $update_dir/codecs/cirrus
fi
```

```
git clone https://github.com/davidjo/snd_hda_macbookpro.git
git clone https://github.com/1dco/macbookpro/
cd snd_hda_macbookpro/
git apply ../macbookpro/snd_hda_macbookpro.patch
#run the following command as root or with sudo
./install.cirrus.driver.sh
reboot
```

## Wifi

install iwd

```
sudo pacman -S iwd
```

set country to DE

```
sudo iw reg set DE
```

set the iwd as the backend of the networkmanager

```
sudo cat < EOF > /etc/NetworkManager/conf.d/iwd.conf
[device]
wifi.backend=iwd
sudo systemctl enable --now iwd
sudo systemctl stop wpa_supplicant
sudo systemctl mask wpa_supplicant
sudo systemctl restart NetworkManager
```

So far only works with 2.4 GHz, need to find out how to enable 5GHz

### enable 5GHz
Download

`https://bugzilla.kernel.org/attachment.cgi?id=285753` -> brcmfmac43602-pcie.txt

copy to `/lib/firmware/brcm`

Change the following:

```
- Changed the following lines:
macaddr=xx:xx:xx:xx:xx:xx
ccode=X3
regrev=15

to

macaddr=00:90:4c:0d:f4:3e
ccode=0
regrev=1
```

```
ccode=ALL ## this is also possible
```

can also be set to ALL

```
rmmod brcmfmac_wcc brcmfmac && modprobe brcmffmac
```

so far for 5GHz

```
boardflags3=0x40000108
boardflags3=0x00000300
boardflags3=0x00000303    ## seems to be the fastest in speed
```

so far for 2.4GHz

```
boardflags3=0x00000003
```


https://bugzilla.kernel.org/show_bug.cgi?id=193121#c74

