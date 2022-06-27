# proxmox-zfs-tips
proxmox tips
# tips 1 zfs

root@pve:/# zfs umount -f zfs-pool/encrypted
root@pve:/# zfs unload-key -r zfs-pool/encrypted

i was getting this error when trying to unmount an encrypted folder and forcing the unmount didn't help. even when it didn't give error the folder was still mounted. The solution was to move to root folder. once i moved to root folder i was able to unmount and unload the key.

### what i tried:

root@pve:~# umount -f zfs-pool/encrypted
umount: /zfs-pool/encrypted: target is busy.

$ root@pve:~# sudo zfs unload-key -r zfs-pool/encrypted

$ Key unload error: 'zfs-pool/encrypted' is busy.

root@pve:~# umount -l zfs-pool/encrypted
root@pve:~# ls /zfs-pool/encrypted

To fix the problem:
cd /
$ root@pve:/# zfs umount -f zfs-pool/encrypted

root@pve:/# zfs unload-key zfs-pool/encrypted

## create a folder
$ zfs create \
    -o encryption=on \
    -o keysource=passphrase,prompt \
    zfs-pool/encryption

## loading encrypted  folders:
To use zfs an encrypted folder, you need to upload the keyfile first:
root@pve:/# zfs load-key zfs-pool/encrypted
Enter passphrase for 'zfs-pool/encrypted':<password-key>
## creating a subfolder
Creating an encrypted child dataset, which inherits all parameters and keys from its parent:
$ zfs create zfs-pool/encrypted/child
