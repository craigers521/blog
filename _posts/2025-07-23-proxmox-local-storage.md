---
title: Resizing Proxmox local data store
header:
  teaser: /assets/images/20250723/proxmox_local.png
excerpt: resize local storage parition to make more room for iso's and imports
description: How to resize local data store in proxmox 
tags: [ai, mcp]
---

#### The Problem

I stood up my first proxmox server with one 2TB boot drive for the OS and local storage for ISOs, images, OVA imports etc. I have four 4 TB drives in a ZFS pool used for my VM disk and Container storage.  By default proxmox partitions off 100GB of the local data store and the allocates the rest of the disk as LVM-thin store for VM/CT.  I want all my VM/CT storage on my ZFS pool and I need more than 100GB space for ISOs and OVA imports, so I wanted to recapture some of that space that was allocated to LVM-thin. 

#### Solution
it only took 3 commands for me to drop the LVM space and reclaim the rest of the local store: 

```bash
lvremove pve/data
lvextend -l +100%FREE /dev/pve/root
resize2fs /dev/mapper/pve-root 
```
A reload of disks in the GUI and the new space was showing up.  I still see the old now empty LVM store in the GUI but i think that can be solved by removing the config from /etc/pve/storage.cfg

Obviously do your own research to make sure your system was mapped the same as mine using handy show commands like: 
- `lsblk`
- 'df -h`
- `pvs`, `vgs`, and `lvs`


![proxmox local store]({{ site.url }}{{ site.baseurl }}/assets/images/20250723/proxmox_local.png) 

