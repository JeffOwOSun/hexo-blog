title: Auto-mount VMWare Shared Folder to Customized Locations
id: 70
categories:
  - 'Coding &amp; Geek Stuff'
date: 2015-01-18 19:18:44
tags:
---

The first idea was adding a upstart script to <span class="lang:default decode:true  crayon-inline">/etc/init</span>
<pre class="lang:sh decode:true">#
# bind mounts
#

description "bind"

start on stopped mountall 

script 

   mount --bind /mnt/hgfs/UPSharedDisk /home/jowos/UPSharedDisk
   mount --bind /mnt/hgfs/UPSharedDisk/Videos /var/ftp/Videos 

end script</pre>
It had no obvious changes, frankly saying. Doesn't work. (By the way I noticed mountall scripts in the same location. Maybe useful?)

The second trial was editing <span class="lang:sh decode:true  crayon-inline ">/etc/fstab</span>

However, the consequent reboot stuck, complaining that <span class="lang:default decode:true  crayon-inline ">/mnt/hgfs</span>  had yet to exist. Presumably because fstab is being executed before the actual shared folder is somehow mounted from <span class="lang:default decode:true  crayon-inline ">.host:/</span>  to <span class="lang:default decode:true  crayon-inline ">/mnt/hgfs</span>

A silly incident was trying to modify <span class="lang:default decode:true  crayon-inline ">/etc/mtab</span>  and rebooted only to realize it was an auto-generated file basically logging the stuff that would normally appear when you hit <span class="lang:default decode:true  crayon-inline ">mount</span> .

Then I thought I might look at when <span class="lang:default decode:true  crayon-inline ">.host:/</span>  is mounted. I tried a recursive grep in <span class="lang:default decode:true  crayon-inline ">/etc</span>  directory and noticed
<pre class="lang:sh decode:true ">jowos@ubuntu:~$ sudo grep -r -n ".host:/" /etc
/etc/vmware-tools/services.sh:1102:    vmware_exec_selinux "mount -t vmhgfs .host:/ $vmhgfs_mnt"
/etc/mtab:16:.host:/ /mnt/hgfs vmhgfs rw,ttl=1 0 0</pre>
Hey, I might directly go and modify <span class="lang:default decode:true  crayon-inline ">services.sh</span> !

Mimicked this line
<pre class="lang:sh decode:true ">...
vmware_exec_selinux "mount -t vmhgfs .host:/ $vmhgfs_mnt"
...</pre>
And the resulting file was
<pre class="lang:sh decode:true ">...
# Mount all hgfs filesystems
vmware_mount_vmhgfs() {
  if [ "`is_vmhgfs_mounted`" = "no" ]; then
    vmware_exec_selinux "mount -t vmhgfs .host:/ $vmhgfs_mnt"

    #JOwOS's CUSTOM Bindings
    vmware_exec_selinux "mount --bind $vmhgfs_mnt/UPSharedDisk /home/jowos/UPSharedDisk"
    vmware_exec_selinux "mount --bind $vmhgfs_mnt/UPSharedDisk/Videos /var/ftp/Videos"
  fi
}
...</pre>
Reboot, success! now mount tells me
<pre class="lang:sh decode:true ">jowos@ubuntu:~$ mount
...
.host:/ on /mnt/hgfs type vmhgfs (rw,ttl=1)
/mnt/hgfs/UPSharedDisk on /home/jowos/UPSharedDisk type none (rw,bind)
/mnt/hgfs/UPSharedDisk/Videos on /var/ftp/Videos type none (rw,bind)</pre>
&nbsp;