# How to setup LVM on Ubuntu 22.04 for Cinder

## Duration

Approximately 20 minutes

## Objective

To understand and learn how to setup LVM on Ubuntu 22.04.

## Prerequisites:

1. **Hardware**: 2 Hard Disks.
2. **Operating System**: Ubuntu 22.04 LTS.

## Important Notes:

- This should be done on storage node.
- This is a step-by-step guide, so you must follow it in the same order.
- This guide is tested on Ubuntu 22.04 LTS.
- If you are deploying OpenStack in all-in-one mode, then you must follow this guide on the same node.

## Step-by-step guide:

### Step A: Identify the disk that you want to use to setup LVM for Cinder.

List all available block devices (like hard drives, SSDs, and their partitions) on the system. This command provides a
structured view which helps in identifying devices by their names, sizes, and mount points.

```bash
lsblk
```

Result would be like:

```bash
loop0                       7:0    0  63.4M  1 loop /snap/core20/1974
loop1                       7:1    0 111.9M  1 loop /snap/lxd/24322
loop2                       7:2    0  53.3M  1 loop /snap/snapd/19457
loop3                       7:3    0  63.5M  1 loop /snap/core20/2015
sda                         8:0    0   931G  0 disk
├─sda1                      8:1    0     1G  0 part /boot/efi
├─sda2                      8:2    0     2G  0 part /boot
└─sda3                      8:3    0 927.9G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0   927G  0 lvm  /
sdb                         8:16   0   1.7T  0 disk
```

**Points to remember:**

- In this result, we have two disks, `sda` and `sdb`.
- `sda` is the main disk, which is used for the OS, and `sdb` is the disk that we are going to use for Cinder.
- Openstack mounts the disk under the name of `vda` and `vdb`, so don't get confused.

**Nice to know:** `sda` or `sdb` stands for SCSI disk A or B.

# You must identify the disk that you want to use for Cinder

### Step B: Create a new physical volumes group

**NOTE:** Replace `/dev/sdb` with the disk that you want to use for Cinder.

```bash
sudo pvcreate /dev/sdb
```

Result would be like:

```bash
  Physical volume "/dev/sdb" successfully created.
```

To confirm that the physical volume was created, run:

```bash
sudo pvs
```

You would see something like the following:

```bash
  PV         VG        Fmt  Attr PSize    PFree
  /dev/sda3  ubuntu-vg lvm2 a--  <927.95g 968.00m
  /dev/sdb             lvm2 ---    <1.75t  <1.75t
```

**NOTE:** The values that you would see depends on your system configuration.

### Step C: Create a new volume group for Cinder.

Create a new volume group named `cinder-volumes` to be used by Cinder:

**NOTE:** Replace `/dev/sdb` with the disk that you want to use for Cinder.

```bash
sudo vgcreate cinder-volumes /dev/sdb
```

Result:

```bash
  Volume group "cinder-volumes" successfully created
```

To confirm that the volume group was created, run:

```bash
sudo vgs
```

result would like the following:

**NOTE:** The values that you would see depends on your system configuration.

```bash
  VG             #PV #LV #SN Attr   VSize    VFree
  cinder-volumes   1   0   0 wz--n-   <1.75t  <1.75t
  ubuntu-vg        1   1   0 wz--n- <927.95g 968.00m
```

## Summary

In this guide, you learned how to setup LVM on Ubuntu 22.04 for Cinder.