# Overview

# Audio
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
