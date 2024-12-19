### **Host Reconnaissance**

### Processes enum

The ps command lists running processes, displaying PID, name, user, and PPID, useful for identifying targets

```bash
ps
```

### Seatbelt

https://github.com/GhostPack/Seatbelt

**collect system-related information**

```bash
execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Release\Seatbelt.exe -group=system
```

**Logged-On Users Information**

```bash
execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Release\Seatbelt.exe -group=loggedon
```

**Stored Credentials Search**

```bash
execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Release\Seatbelt.exe -group=credentials
```

**Local Group Policy Review**

```bash
execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Release\Seatbelt.exe -group=policy
```

**Available Networks Enumeration**

```bash
execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Release\Seatbelt.exe -group=network
```

**Installed Software Detection**

```bash
execute-assembly C:\Tools\Seatbelt\Seatbelt\bin\Release\Seatbelt.exe -group=software
```

### **Screenshots**

Takes a single screenshot using the PrintScr method.

```bash
printscreen
```

Takes a single screenshot.

bash
Copy code

```bash
screenshot
```

Captures periodic screenshots of the desktop.

```bash
screenwatch
```

### **Keylogger**

A keylogger can capture the keystrokes of a user.

```bash
 keylogger
```

The keylogger runs as a job that can be stopped.

kill jobs

```bash
jobs
```

```bash
jobkill <JID>
```

### **Clipboard**

The clipboard command will capture any text from the user's clipboard (it does not capture images or any other types of data).

```bash
clipboard
```

### **User Sessions**

List the logon sessions.

```bash
net logons
```

List all users in the domain.

```bash
net user /domain
```

View groups in the domain.

```bash
net group /domain
```

List groups a specific user belongs to.

```bash
net user <username> /domain
```

View all permissions and settings for a user.

```bash
net user <username> /all
```

View members of a specific group.

```bash
net group "GroupName" /domain
```

List active sessions on the system.

```bash
net session
```

View shared resources on a machine.

```bash
net share
```

Show computers in the domain.

```bash
net view /domain
```

Identify domain controllers.

```bash
net group "Domain Controllers" /domain
```

View the network configuration of the machine.

```bash
net config workstation
```
