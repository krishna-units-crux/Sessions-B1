# **Introduction to Linux File Systems**

This is mostly described in Stevens, Chapter 4. Read it. Now.

Layout of the file system:

· Each physical drive can be divided into several partitions

· Each partition can contain one file system

· **Each file system contains**:

1.              boot block(s);

1.              superblock;

1.              inode list;

1.              data blocks.

· A boot block may contain the bootstrap code that is read into the machine upon booting.

· A superblock describes the state of the file system:

        how large it is;
        how many files it can store;
        where to find free space on the file system;
        who has ownership of it;
        and more.

· The inode list is an array of "information nodes" analogous to the FAT (File Allocation Table) system in MS-DOS.

· data blocks start at the end of the inode list and contain file data and directory blocks.

The term file system can mean a single disk, or it can mean the entire collection of devices on a system. It's held together in this second sense by the directory structure.

The directory "tree" usually spans many disks and/or partitions by means of mount points. For example, in Red Hat Linux, there are pre-defined mount points for floppy disks and CD-ROMs at floppy and cdrom in /mnt. See also fstab and mtab in /etc.

##Linux-supported File Systems

minix is the filesystem used in the Minix operating system, the first to run under Linux. It has a number of shortcomings, mostly in being small.

ext is an elaborate extension of the minix filesystem. It has been completely superseded by ext2.

ext2 is the disk filesystem used by Linux for both hard drives and floppies. ext2, designed as an extension to ext, has in its turn generated a successor, ext3.

ext3 offers the best performance (in terms of speed and CPU usage) combined with data security of the file systems supported under Linux due to its journaling feature.

xiafs was designed as a stable, safe file system by extending minix. It's no longer actively supported and is rarely used.

msdos is the filesystem used by MS-DOS and Windows. msdos filenames are limited to the 8 + 3 form. It's especially good for floppies that you move back and forth.

umsdos extends msdos by adding long filenames, ownership, permissions, and special files while remaining compatible with MS-DOS and Windows.

vfat extends msdos to be compatible with Microsoft Windows' support for long filenames (a good choice for dual-boot).

proc is a pseudo-filesystem which is used as an interface to kernel data. Its files do not use disk space. See proc(5).

iso9660 is a CD-ROM filesystem type conforming to the ISO 9660 standard, including both High Sierra and Rock Ridge.

nfs is a network filesystem used to access remote disks.

smb is a network filesystem used by Windows.

ncpfs is a network filesystem that supports the NCP protocol, used by Novell NetWare.

###Partition Structure

**Boot Block(s)**

Blocks on a Linux (and often a Unix) filesystem are 1024 bytes in length, but may be longer or shorter. The blocks are normally a power of 2 in size (1024 is 2 to the 10th power). Some systems use 512 bytes (2 to the 9th) but 2048 and 4096 are also seen.

The first few blocks on any partition may hold a boot program, a short program for loading the kernel of the operating system and launching it. Often, on a Linux system, it will be controlled by LILO or Grub, allowing booting of multiple operating systems. It's quite simple (and common) to have a multiple boot environment for both Linux and a flavour of Windows.

**Superblock** :

The boot blocks are followed by the superblock, which contains information about the geometry of the physical disk, the layout of the partition, number of inodes and data blocks, and much more.

**Inode Blocks** :

Disk space allocation is managed by the inodes (information node), which are created by the mkfs(1) (make filesystem) command. Inodes cannot be manipulated directly, but are changed by many commands, such as touch(1) and cp(1) and by system calls, like open(2) and unlink(2), and can be read by stat(2). Both chmod(1) and chmod(2) change access permissions.

**Data Blocks** :

This is where the file data itself is stored. Since a directory is simply a specially formatted file, directories are also contained in the data blocks. An allocated data block can belong to one and only one file in the system. If a data block is not allocated to a file, it is free and available for the system to allocate when needed.
