# Install NixOS on a Lenovo Thinkpad P15 Gen 2i with a ZFS Encrypted Root

1. Partition and format the drive:
- Open a terminal and run the following commands:
- To erase all partitions and partitioning information on `/dev/nvme0n1`, use the command:
``` bash
sudo sgdisk --zap-all /dev/nvme0n1
```
- To open the `fdisk` utility for `/dev/nvme0n1`, use:
```bash
  sudo fdisk /dev/nvme0n1
```
- Inside the `fdisk` command, follow these steps:
	- Press `g` to create a new GPT partition table.
	- Press `n` to create a new partition.
	- Accept the default partition number.
	- Accept the default first sector.
	- Specify the last sector as "+2G" to create a 2GB partition.
	- Press `t` to change the partition type.
	- Use partition type 1 for EFI System.
	- Press `n` to create another new partition.
	- Accept the default partition number.
	- Accept the default first and last sectors.
	- Press `w` to write the changes and exit.
	  
- Make sure to have a backup of any important data on the drive before executing these above commands, as it permanently modifies data on the specified drive. Exercise caution to avoid accidentally wiping data on the wrong device.
- It is recommended to omit a swap partition if you have a large amount of memory and for enhanced security purposes. The absence of a swap partition reduces the risk of sensitive data being written to disk.

2. Create the boot volume:
```bash
sudo mkfs.fat -F 32 /dev/nvme0n1p1
sudo fatlabel /dev/nvme0n1p1 NIXBOOT
```

3. Create a ZFS pool:
```bash
sudo zpool create -f \
  -o altroot="/mnt" \
  -o ashift=12 \
  -o autotrim=on \
  -O compression=lz4 \
  -O acltype=posixacl \
  -O xattr=sa \
  -O relatime=on \
  -O normalization=formD \
  -O dnodesize=auto \
  -O sync=disabled \
  -O encryption=aes-256-gcm \
  -O keylocation=prompt \
  -O keyformat=passphrase \
  -O mountpoint=none \
  NIXROOT /dev/nvme0n1p2
```

4. Create ZFS volumes:
```bash
sudo zfs create -o mountpoint=legacy NIXROOT/root
sudo zfs create -o mountpoint=legacy NIXROOT/home
```
- To reserve some disk space to handle potential scenarios where the disk runs out of available space, you can create a reserved ZFS volume. This reserved volume can act as a safeguard against complete disk usage, ensuring that essential system operations can still occur even if the disk reaches its capacity.
- To create a reserved ZFS volume, you can use the following command:
```bash
sudo zfs create -o refreservation=1G -o mountpoint=none NIXROOT/reserved
```
- The `-o refreservation=1G` option reserves 1GB of disk space for the `NIXROOT/reserved` volume. Adjust the size as per your requirements. The `-o mountpoint=none` option specifies that this volume should not be mounted.
- By reserving space in advance, you provide a buffer to prevent critical system operations from failing due to insufficient disk space.

5. Mount Volumes and Sub-Volumes:
```bash
sudo mount -t zfs NIXROOT/root /mnt
sudo mkdir /mnt/boot
sudo mkdir /mnt/home
sudo mount /dev/nvme0n1p1 /mnt/boot
sudo mount -t zfs NIXROOT/home /mnt/home
```

6. Generate the configuration:
```bash
sudo nixos-generate-config --root /mnt
```

7. Edit the configuration:
    - You can use a text editor to modify the generated configuration file at `/mnt/etc/nixos/configuration.nix`. Make the necessary changes according to your requirements and preferences. You can refer to the provided `configuration.nix` file for guidance.

8. Install NixOS:
```bash
sudo nixos-install
```   

During the installation process, you'll be prompted to set the encryption passphrase for the ZFS root. Follow the on-screen instructions to complete the installation.

Please note that this guide assumes you have a basic understanding of partitioning and formatting drives, and it's always a good idea to have a backup of your important data before performing any system installations or modifications.