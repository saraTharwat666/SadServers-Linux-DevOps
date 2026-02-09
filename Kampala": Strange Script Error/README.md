# Kampala Scenario: Solving the "Missing" Interpreter Mystery

## Problem Description
Execution of shell scripts in `/home/admin/deploy/` failed with the error:
`-bash: cannot execute: required file not found`
Despite the files being present and permissions set correctly, they refused to run.

## Root Cause
The scripts were created on a **Windows** environment and transferred to **Linux**. Windows uses **CRLF** (`\r\n`) line endings, while Linux expects **LF** (`\n`). 
The hidden `\r` character at the end of the shebang line (`#!/bin/bash\r`) made Linux look for an interpreter that doesn't exist.

## Solution
Used `sed` to perform a global search and replace for the carriage return character:

```bash
# Remove all carriage returns (\r) from the scripts
sed -i 's/\r//' /home/admin/deploy/*.sh

# Ensure execute permissions are granted
chmod +x /home/admin/deploy/*.sh
