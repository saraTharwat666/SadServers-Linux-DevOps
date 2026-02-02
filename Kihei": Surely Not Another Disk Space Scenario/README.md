# Scenario: "Kihei" - Linux Disk Space Management & LVM

This project demonstrates a real-world Linux system administration scenario where a program fails due to insufficient disk space, even when critical files cannot be deleted.

## 📝 Scenario Description
There is a program located at `/home/admin/kihei` that fails to run because the root partition is nearly full (93% usage). A large file `/home/admin/datafile` (5GB) is consuming most of the space and cannot be deleted.

**Goal:** Make the `kihei` program run successfully (returning "Done..") without deleting the existing `datafile`.

## 🔍 Investigation
1. **Error Check:** Running `/home/admin/kihei` resulted in a `panic: exit status 1`.
2. **Disk Usage:** Using `df -h`, I found the root partition had only ~500MB available.
3. **Storage Discovery:** Using `lsblk`, I identified two unused 1GB disks (`/dev/nvme1n1` and `/dev/nvme2n1`).
4. **Program Requirements:** Using `strace`, it was determined that the program attempts to create a 1.5GB file in `/home/admin/data/`.

## 🛠️ The Solution (LVM & Mounting)
Since no single unused disk was large enough (1.5GB needed > 1GB available), I used **LVM (Logical Volume Management)** to aggregate the two disks into one logical volume.

### Steps Taken:

1. **Create Physical Volumes (PVs):**
   ```bash
   sudo pvcreate /dev/nvme1n1 /dev/nvme2n1
   ```

Create a Volume Group (VG):

```Bash
sudo vgcreate vg /dev/nvme1n1 /dev/nvme2n1
```
Create a Logical Volume (LV):

```Bash
sudo lvcreate -n lv -l 100%FREE vg
```
Format the Logical Volume:

```Bash
sudo mkfs.ext4 /dev/vg/lv
```
Mount to the Target Directory:

```Bash
mkdir -p /home/admin/data
sudo mount /dev/vg/lv /home/admin/data
sudo chown -R admin:admin /home/admin/data
```
✅ Result
After mounting the new 2GB volume to the expected data directory, running /home/admin/kihei returned: ```Done..```
