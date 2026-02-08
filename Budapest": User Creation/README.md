# Budapest Scenario: Automated User Creation

## Project Overview
This task involved automating the creation of multiple Linux user accounts based on a provided text file (`user_list.txt`). The goal was to process the file and set up accounts with their respective passwords efficiently.

## Problem Description
The system contained a file `user_list.txt` with entries formatted as `username;password`. Manual creation was not feasible due to the number of users (20 entries).

## Technical Solution
I utilized a Bash `while` loop combined with `IFS` (Internal Field Separator) to parse the file and execute system commands for account management.

### The Automation Script:
```bash
while IFS=";" read -r username password; do 
    # Create the user with a home directory
    sudo useradd -m "$username" 
    # Set the password in a non-interactive way
    echo "$username:$password" | sudo chpasswd
done < user_list.txt
