# Network Security Lab: Observing an HTTP Session (Experiment 5)

## Purpose

This experiment illustrates why unencrypted web traffic is unsafe on a shared local network. In a closed Kali–Metasploitable 2 lab, ARP replies were used to route the target's traffic through Kali, then Wireshark was used to inspect DVWA traffic.

**Authorization boundary:** The steps and evidence below describe a controlled classroom VM setup only. Never perform interception on a real network without explicit authorization.

## Environment captured

| Component | Lab detail |
|---|---|
| Interceptor / packet analyzer | Kali Linux |
| Web application host | Metasploitable 2 with DVWA |
| Target | `192.168.1.100` |
| Gateway | `192.168.1.1` |
| Kali capture adapter | `eth0` |

## Walkthrough

### 1. Check connectivity first

The initial terminal capture records successful ping replies from the target and the gateway, establishing that both were reachable before the traffic inspection.

![Initial target and gateway reachability from Kali](Screenshot%202026-10-15%20122202.png)

### 2. Turn on forwarding

IP forwarding was enabled on Kali so it could pass packets along while observing the traffic. The separate screenshot records the enabled setting.

![Kali forwarding setting](Screenshot%202026-10-15%20123051.png)

### 3. Redirect the local exchange with ARP spoofing

The two terminal views show spoofed ARP replies addressed toward the target and toward the gateway. Both directions are needed for the illustrated interception path.

![Spoofed gateway mapping sent to the target](Screenshot%202026-10-15%20122835.png)

![Spoofed target mapping sent to the gateway](Screenshot%202026-10-15%20122925.png)

### 4. Observe packets at Kali

A Wireshark capture filtered for traffic involving the target shows network packets passing during the exercise. On the Metasploitable terminal, a ping to the gateway completes while the capture is running.

![Wireshark capture with packets involving the target](Screenshot%202026-10-15%20123249.png)

![Target-side ping and request generation](Screenshot%202026-10-15%20123257.png)

### 5. Inspect the web exchange and outcome

The filtered capture lists a `POST /dvwa/login.php` request using HTTP. The final browser view shows the DVWA page after login, confirming that the test session was authenticated.

![DVWA login POST visible in Wireshark](Screenshot%202026-10-15%20123526.png)

![DVWA home page after authentication](Screenshot%202026-10-15%20124548.png)

## Finding

In this isolated test, Kali could observe the target's HTTP exchange after ARP redirection and forwarding were enabled. HTTP carries application data without transport encryption, so a network observer may read sensitive request contents or session material. Using HTTPS and appropriately protected session cookies reduces this risk.

## Screenshot files

Keep the `screenshots/` folder beside this README in the repository. The nine images shown here are the non-secret evidence supplied for the report. Four additional originals reveal the test login fields or a reusable session-cookie value; they are not copied into this GitHub-ready set. Do not commit those originals. If they are required for grading, submit them privately or redact the sensitive values before adding them.

## Notes

The addresses and interface names shown are from this specific VM run. Repeat the activity only in an isolated lab with accounts and systems you are authorized to test.

