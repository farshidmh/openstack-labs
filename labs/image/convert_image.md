# Convert img to raw and create an image from it

## Objective:

In this lab you will learn how to convert an image to the raw format and create an OpenStack image from it.

## Time Estimate:

You should be able to complete this lab in less than `30 minutes`.

## Prerequisites:

- OpenStack CLI tools installed.
- `openrc` file available.
- `qemu-img` installed.
- `wget` installed.

## Steps:

### Download img file

Download cirros `.img` file using `wget`

```bash
wget https://download.cirros-cloud.net/0.6.2/cirros-0.6.2-x86_64-disk.img
```

verify that the file is downloaded

```bash
ls
```

result:

```bash
cirros-0.6.2-x86_64-disk.img
```

### Convert img to qcow2

The `qemu-img` convert command can do conversion between multiple formats, including `qcow2`, `qed`, `raw`, `vdi`, `vhd`, and `vmdk`.

**Note:** `ISO` files are not supported by `qemu-img` since it is a snapshot of a `DVD`.


```bash
cd /home/pure

qemu-img convert -f qcow2 -O raw cirros-0.6.2-x86_64-disk.img cirros-0.6.2-x86_64-disk.raw
```

to verify that the file is converted

```bash
ls
```

Result:

```bash
cirros-0.6.2-x86_64-disk.raw
```


### Source the OpenRC File

Follow `labs/source_ubuntu.md` to source the `openrc` file.

### Crate an Image

After sourcing the `openrc` file, you can create an image using the `openstack image create` command.

```bash
openstack image create --public --protected --disk-format raw --file cirros-0.6.2-x86_64-disk.raw "CirrOs 0.6.2-x86_64"
```

- `--public`: Makes the image accessible to all users.
- `--protected`: Prevents the image from being deleted.
- `--disk-format`: Specifies the disk format (e.g., "qcow2", "raw").
- `"CirrOs 0.6.2-x86_64"`: The name of the image.
- `--file`: The path to the image file.


## Conclusion:

In this comprehensive lab, you learned how to convert an image to the qcow2 format and create an OpenStack image from it.