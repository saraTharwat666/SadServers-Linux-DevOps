# 🛠️ SadServers Challenge: "Oaxaca" - Closing an Open File Descriptor

## 📋 Scenario Overview
The task was to close a specific file (`/home/admin/somefile`) that was being held open for writing by a running process. The constraint was to close the file **without killing the process**.

- **Level:** Medium
- **Category:** Linux Internals / File Descriptors
- **Target File:** `/home/admin/somefile`

---

## 🔍 Root Cause Analysis
In Linux, "everything is a file." When a process opens a file, the kernel assigns it a **File Descriptor (FD)**. Even if you delete the file from the disk, the disk space won't be freed as long as the process keeps the FD open. To solve this without stopping the service, we must force the process to release the FD.

---

## 🛠️ Solution Steps

### Step 1: Identify the Process and File Descriptor (FD)
First, we need to find which process is using the file and what the FD number is.
```bash
lsof /home/admin/somefile
```
Example Output: COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME python3 1234 admin 3w REG 8,1 0 123 /home/admin/somefile

PID: 1234 (The Process ID)

FD: 3 (The File Descriptor number, 'w' means write mode)

Step 2: Close the FD using GDB (The "Surgical" Approach)
Since we cannot kill the process, we use the GNU Debugger (GDB) to attach to the running process and call the close() system function on that specific FD.

```Bash
sudo gdb -p [PID] -ex 'call close([FD])' -ex 'detach' -ex 'quit'
```
Replace [PID] with 1234 and [FD] with 3 based on your findings.

Step 3: Verification
Verify that the file is no longer held by any process:

```Bash
lsof /home/admin/somefile
```
Expected Result: No output returned.

🧐 Key Concepts Learned
/proc/[PID]/fd/: A virtual directory where Linux stores links to all files opened by a specific process.

File Descriptors: Integer handles used by the OS to manage open files/sockets.

GDB Injection: Using a debugger to execute system calls inside a live process without restarting it.
