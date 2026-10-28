**Task Description:**
HELP! I accidentally deleted my pumpkins, its almost halloween! I need to have my pumpkins D:

Flag format: CTFkom{...}

**Solution steps:**
1. Checked what kind of filetype it was with the following command. Initial thought is that i need to extract the deleted file(s) from this disk image.
```
└─$ file pomkin.img 
pomkin.img: DOS/MBR boot sector; partition 1 : ID=0xb, start-CHS (0x0,1,1), end-CHS (0x3ff,254,63), startsector 63, 130977 sectors, extended partition table (last)
```
2. As of my knowledge, a file is not permanently removed from disk when deleted, but the section on the disk is marked as free so other files can be stored in that section. If the section of the deleted file is not overwritten by another file, it should be possible to extract the file. A suitable tool for this task is Sleuthkit, which is an open-source forensics tool used for disk and storage forensics.
3. Ran the following to obtain initial information about the disk. There is multiple partitions and the most interesting one is `002` which is a `Win95 FAT32` partition. This partition has a starting sector of `63`
```
└─$ mmls pomkin.img 
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000000062   0000000063   Unallocated
002:  000:000   0000000063   0000131039   0000130977   Win95 FAT32 (0x0b)
003:  -------   0000131040   0000131071   0000000032   Unallocated
```
4. By running the following command, I am able to obtain information about the file structure from the offset obtained earlier. The most interesting files here are the `.jpg` files with inode 6 and 8.
```
└─$ fls -o 63 pomkin.img 
r/r 3:  POMKIN      (Volume Label Entry)                                         d/d 5:  .fseventsd                                                               r/r * 6:        _omkin.jpg                                                       r/r * 8:        ._pomkin.jpg                                                     v/v 2091523:    $MBR                                                             
v/v 2091524:    $FAT1
v/v 2091525:    $FAT2
V/V 2091526:    $OrphanFiles
```
5. Following commands are able to extract the inodes back into `.jpg` files.
```
┌──(nordz㉿kali)-[~/CTFkom/281026]
└─$ icat -r -o 63 pomkin.img 6 > _omkin_recovered.jpg

┌──(nordz㉿kali)-[~/CTFkom/281026]
└─$ icat -r -o 63 pomkin.img 8 > ._pomkin_recovered.jpg   
```
6. Was then able to open the first image and got the flag. 
![lost-pumpkins-flag](lost-pumpkins-flag.png)
7. FLAG: `CTFkom{DUm_ahh_p0mkin}`