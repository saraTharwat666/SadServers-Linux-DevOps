# Bondo Scenario: The Inode Exhaustion Mystery

## Problem Description
The `bondo` tool failed to generate part files, throwing a `panic: no space left on device` error, even though `df -h` showed plenty of available disk space.

## Investigation
- `df -h`: Showed only 1% disk usage.
- `df -i`: Revealed **100% Inode usage** (512/512). The partition was formatted with a very low number of Inodes.
- Running the tool as `admin` initially failed with `permission denied` after reformatting.

## Solution
1. **Unmount** the problematic partition.
2. **Reformat** the partition to increase the Inode count:
   `sudo mkfs.ext4 /dev/mapper/vg--backup-lv--bondo`
3. **Remount** and verify with `df -i`.
4. **Fix Ownership**: Change the owner of the mount point to the `admin` user:
   `sudo chown admin:admin /srv/bondo`
5. **Persistence**: Ensured the entry in `/etc/fstab` is correct for reboots.

## Result
The tool executed successfully as the `admin` user, returning `part files generation completed!`.
