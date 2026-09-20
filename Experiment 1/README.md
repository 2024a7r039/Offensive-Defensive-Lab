# Experiment 1: Scanning for Vulnerabilities in a Network

## Objective

To identify active hosts, open ports, running services, and known vulnerabilities on a target network using Nmap and Nessus.

## Tools Used

- Kali Linux
- Nmap
- Nessus
- Target Machine

## Procedure

### Step 1: Check IP Address and Connectivity

Check the IP address of the Kali machine and verify connectivity with the target machine.

```bash
ifconfig
ping <target-ip>
```

### Step 2: Discover Live Hosts

Scan the network to identify active hosts.

```bash
nmap -sn 192.168.56.0/24
```

### Step 3: Scan Open Ports

Scan the target machine to identify open ports and services.

```bash
nmap -sS <target-ip>
```

### Step 4: Detect Services and Operating System

Detect the service versions and operating system of the target machine.

```bash
nmap -sV -O <target-ip>
```

### Step 5: Save Nmap Results

Save the Nmap scan results into a text file.

```bash
nmap -sV -oN nmap_scan_results.txt <target-ip>
```

### Step 6: Launch Nessus

Open the Nessus web interface and select **New Scan → Basic Network Scan**.

```text
https://localhost:8834
```

### Step 7: Configure and Launch the Scan

Enter the target IP address, save the configuration, and launch the scan.

### Step 8: Review Vulnerabilities

Review the vulnerabilities detected by Nessus according to their severity:

- Critical
- High
- Medium
- Low
- Info

### Step 9: Document Findings

For Critical and High vulnerabilities, record:

- CVE ID
- Affected service/port
- Recommended remediation

## Result

The active hosts, open ports, running services, and known vulnerabilities of the target network were identified and documented.

## Conclusion

The experiment demonstrated the use of Nmap for network scanning and Nessus for vulnerability assessment.

## Ethical Consideration

The experiment should be performed only on an authorized lab network or test machine.
