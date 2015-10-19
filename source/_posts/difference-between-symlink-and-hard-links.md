title: Difference between symlink and hard links
id: 111
categories:
  - 'Coding & Geek Stuff'
date: 2015-03-22 14:04:50
tags:
---

> <span style="text-decoration: underline;"></span>Underneath the file system files are represented by inodes (or is it multiple inodes not sure)> 
> 
> A file in the file system is basically a link to an inode.> 
> A hard link then just creates another file with a link to the same underlying inode.> 
> 
> When you delete a file it removes one link to the underlying inode. The inode is only deleted (or deletable/over-writable) when all links to the inode have been deleted.> 
> 
> A symbolic link is a link to another name in the file system.> 
> 
> Once a hard link has been made the link is to the inode. deleting renaming or moving the original file will not affect the hard link as it links to the underlying inode. Any changes to the data on the inode is reflected in all files that refer to that inode.> 
> 
> Note: Hard links are only valid within the same File System. Symbolic links can span file systems as they are simply the name of another file.
See: [http://stackoverflow.com/questions/185899/what-is-the-difference-between-a-symbolic-link-and-a-hard-link](http://stackoverflow.com/questions/185899/what-is-the-difference-between-a-symbolic-link-and-a-hard-link)