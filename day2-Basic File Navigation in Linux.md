# Day 2 - Basic File Navigation in Linux
<img width="1200" height="400" alt="image" src="https://github.com/user-attachments/assets/889f5ffe-e362-4c06-a357-c83ab0bb131b" />

Today’s focus is learning to navigate the Linux file system — a critical skill for any hacker.

---

## 📂 Key Commands

```bash
ls         # List contents of a directory
cd         # Change directory
pwd        # Show current directory path
clear      # Clear terminal screen
tree       # Display directory structure
clear     # Clear terminal screen
history   # View command history

```

🧪 Task of the Day
Open your terminal

Try the following:
```
cd /
ls
cd etc
pwd
cd ~
ls -la
```

Observe how directories are organized

Use tree to visualize the structure (install it if not available):

```
sudo apt install tree
tree /home
```

🔍 Linux Directory Breakdown
```
/etc	Configuration files
/var	Logs, web files, variable data
/home	User directories
/tmp	Temporary storage (often writable by all users)
/root	Home directory of the root user
```

> These directories are often involved in privilege escalation or persistence in real-world attacks.

🚀 Daily updates on t.me/cybersecplayground
💡 Learn real-world Linux skills for hacking, CTFs, and red teaming.

