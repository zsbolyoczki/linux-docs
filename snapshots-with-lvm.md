# Using snapshots on LVM (Ubuntu 18.04 LTS)


## Prereqs

1. LVM :)
2. Free unused space for a new LV




## Backup with LVM


Unmount the original LV and create the snapshot of the logical volume
```
unmount ...
lvcreate --snapshot -n <SNAPSHOTNAME> -L <SIZE><G|M> /dev/mapper/<VGNAME-TO-BACKUP>
```

Listing of logical volumes (new snapshot LV is should be there)
```
lvs
```

Mount the snapshot instead of the original LV and start using it
```
mount /dev/mapper/<VGNAME>-<SNAPSHOTNAME> /path/to/original/mountpoint
```



## Merge ('keep', 'commit') the changes

Merging the snapshot means that all the changes will be kept ('commit', 'go live').

First the snapshot LV has to be unmounted then merged with the original one
```
umount /path/to/original/mountpoint
lvconvert --merge /dev/mapper/<VGNAME>-<SNAPSHOTNAME>
```

Merging the snapshot takes time depending on the size of the difference of the old and new data.
Merging also removes the snapshot LV.



## Roll back to the original state ('restore', 'drop changes')

First unmount the snapshot LV then drop it and remount the original one
```
umount /path/to/original/mountpoint
lvremove <VGNAME> <SNAPSHOTNAME>
mount /dev/mapper/<VGNAME>-<ORIGINAL-LV>
```



