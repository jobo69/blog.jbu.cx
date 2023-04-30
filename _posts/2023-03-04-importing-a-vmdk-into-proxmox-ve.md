---
title: "Importing a .vmdk into Proxmox VE"
date: 2023-03-04T02:00:00+01:00
draft: false
tags: [proxmox,vmware,esxi]
image: /static/media/posts/importing-a-vmdk-into-proxmox-ve/benjamin-lehman-GNyjCePVRs8-unsplash.jpg
---

1. Create a new Virtual Machine in Proxmox with no disk
2. Copy the vmdk to the Proxmox host
3. Import the VMDK to the `local-lvm` storage (or any other storage). Replace 100 with your VM ID
```bash
qm importdisk 100 /path/to/disk.vmdk local-lvm
```
4. Wait until the import is finished
5. Select the VM in the Proxmox Web Interface
6. Go to the Hardware Tab
    1. The new disk should appear here
    2. Double click on the disk and click the add button
7. Go to the options menu
    1. Select Boot Order
    2. Disselect everything except the disk
8. Start the VM