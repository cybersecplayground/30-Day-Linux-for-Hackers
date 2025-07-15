# Day 3 – Understanding Linux File Permissions
![30day_linux](https://github.com/user-attachments/assets/f99d5a61-7e2f-4961-b91e-0d6cb0f05c9a)


In hacking and cybersecurity, file permissions can either **stop you cold** or be the **door you walk through**. Misconfigured permissions often lead to **privilege escalation** or **information disclosure**.

---

## 🔍 File Permission Breakdown

Run:
```bash
ls -l
```

Example output:

```
-rwxr-xr-- 1 user group 1337 Jan 1  exploit.sh
```

Explanation:

`-` → regular file (d = directory, l = symlink)
`rwx` → owner can read, write, execute   
`r-x` → group can read, execute   
`r--` → others can only read

🧠 Permission Notation
There are two formats:

| Symbolic  | Octal | Meaning                             |
| --------- | ----- | ----------------------------------- |
| rwxr-xr-- | 754   | Owner: all, Group: rx, Others: r    |
| rwxrwxrwx | 777   | Full access to everyone (dangerous) |


Use this table when setting permissions using chmod.

🔧 Key Commands
```
chmod +x file          # Add execute permission
chmod 755 file         # Set rwxr-xr-x
chown user:group file  # Change file ownership
```

🧪 Task of the Day
Create a basic script:

```
echo 'echo hacked' > test.sh
chmod +x test.sh
./test.sh
```

Try changing permissions:
```
chmod 777 test.sh
ls -l
```

Modify ownership (you might need sudo):
```
sudo chown root:root test.sh
```

🚩 Real-World Hacking Example
In many real-world scenarios:
 
🔥 A root-owned script with group-writable access → potential escalation    
🔥 Misconfigured crontabs (`/etc/cron.d/`) running user-editable scripts = root shell    
🔥 Understanding permissions is not optional — it's a hacker's tool.    

🎯 Learn this and more on our Telegram channel:    
👉 https://t.me/cybersecplayground

