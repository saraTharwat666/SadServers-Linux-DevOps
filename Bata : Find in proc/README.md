# 🕵️ Scenario: Hunting Secrets in /proc/sys

## 📝 Challenge Description
A hidden password was left by a spy within the `/proc/sys` directory. The identifying characteristic of the file is that its content begins with the prefix `secret:`. The goal was to locate this file, extract the password, and save it to a specific location while adhering to strict MD5 checksum requirements.

---

## 🔍 Investigation & Strategy
The `/proc` directory in Linux is a pseudo-filesystem that provides an interface to kernel data structures. Searching through it requires tools that can handle a large volume of files efficiently.

### The Plan:
1. **Recursive Search:** Use `grep -r` to scan all files under `/proc/sys`.
2. **Error Handling:** Redirect standard error (`2>/dev/null`) to ignore "Permission Denied" messages typical of the `/proc` directory.
3. **Data Parsing:** Utilize string manipulation tools (`cut` or `awk`) to isolate the password from the `secret:` label.
4. **Validation:** Use `md5sum` to ensure the extracted string exactly matches the expected solution.

---

## 🛠️ The Fix

### 1. Locate and Extract
Executed a combined command to find the string and pipe it through a delimiter-based filter:
```bash
grep -r "secret:" /proc/sys 2>/dev/null | cut -d':' -f2 > /home/admin/secret.txt
```

2. Manual Verification
Verified the content of the generated file:

```Bash

cat /home/admin/secret.txt
```

3. Checksum Verification
Validated the solution against the required MD5 hash:

Bash

```md5sum /home/admin/secret.txt
```
# Expected: a7fcfd21da428dd7d4c5bb4c2e2207c4

✅ Results
Search Path: /proc/sys

Extraction Method: Delimiter-based cutting (cut -d':')



Security Context: Performed without root/sudo privileges.

Status: Solved 🏆
