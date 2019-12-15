# Kali LUKS Encryption Fix for Raspberry Pi 4 


I had tried all the online guides I could find to get Kali installed on a Raspberry Pi 4 with LUKS Encryption and Dropbear working. None appeared to work properly with a Pi 4, where as they did when I had previously did this with a Pi 3, so I set about trying to get it working myself and found a fix. probably not a good fix, but it did work and has been reliable, so far.

These are the guides I had found and followed:

https://www.kali.org/docs/arm/raspberry-pi-with-luks-disk-encryption/

https://www.kali.org/tutorials/secure-kali-pi-2018/

https://github.com/tothi/kali-rpi-luks-crypt

What happens is the Dropebear SSH server fails to run the 'cryptsetup luksOpen' part.

So to work around this, I created a script: luks-mount

This script forces: cryptsetup luksOpen /dev/mmcblk0p2 crypt_sdcard
Thus you get the password prompt to decrypt the LUKS partition
Whilst building in the chroot, place it in: /usr/share/initramfs-tools/scripts/fattusrattus

Now to get it to run when Dropbear SSH starts, you need to change the command put at the start (in front of the ssh keys) of: 
/etc/dropbear-initramfs/authorized_keys

The command should be: command="/scripts/fattusrattus/luks-mount && kill -9 \`ps | grep -m 1 'cryptroot' | cut -d ' ' -f 3\`"

There should be a space at the end where the keys should start with: ssh-rsa
