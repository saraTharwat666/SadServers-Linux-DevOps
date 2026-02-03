To bypass the filter, I used the --user-agent flag to pretend the request was coming from a browser:

```Bash
# Spoofing the User-Agent to bypass the block
curl --user-agent "Mozilla/5.0" localhost:5000
```

# Output: Welcome! Password is FDZPmh5AX3oiJt
🏁 Final Steps:
To satisfy the lab's test condition (MD5 Hash):

```Bash
echo -n "FDZPmh5AX3oiJt" > ~/mysolution
md5sum ~/mysolution
```
# Result: d8bee9d7f830d5fb59b89e1e120cce8e
🧠 Key Takeaways (Lessons Learned)
User-Agent Spoofing: Security that relies on identifying a client (like blocking curl) is easily bypassed.

Header Manipulation: Knowing how to use curl flags like -A or -H is crucial for web debugging.

MD5 Integrity: The difference between echo and echo -n is vital when dealing with hashes, as trailing newlines change the entire hash value.
