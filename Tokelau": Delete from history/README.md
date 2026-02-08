```bash
sed -i '/foo/d' ~/.bash_history && history -d $(history | grep "foo" | awk '{print $1}' | tac)
```

Cleared the current session's memory and reloaded the sanitized history from the disk:
```Bash
history -c && history -r
```
Verification

Ran ```history | grep "foo"``` which returned no results, confirming a clean environment.
