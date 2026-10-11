# Experiment 4: Firewall and IDS Setup Using pfSense and Snort

## Aim

To build a small virtual network, configure pfSense as its firewall, install Snort as an intrusion detection system (IDS), and examine traffic generated from Kali Linux with Nmap.

## Lab requirements

- VMware Workstation or VMware Player
- pfSense firewall virtual machine
- Kali Linux virtual machine
- Metasploitable virtual machine
- VMware virtual networks: NAT (VMnet8) and a private network (VMnet1)
- Snort package for pfSense and GPLv2 Community Rules

## Network topology

The pfSense WAN interface connects to VMware NAT (VMnet8). Its LAN interface and both test machines connect to the private VMnet1 network. Kali is used to generate test traffic; Metasploitable is the target host.

```text
                    Internet
                       |
              VMware NAT (VMnet8)
                       |
                 pfSense WAN
                 pfSense firewall
                 pfSense LAN
                  192.168.1.1
                       |
               Private LAN (VMnet1)
                 /             \
       Kali Linux               Metasploitable
       192.168.1.101            192.168.1.100
       Scanner                  Test target
```

| Device or interface | IPv4 address | VMware network | Purpose |
|---|---|---|---|
| pfSense WAN | DHCP from VMnet8 | NAT (VMnet8) | Upstream connectivity |
| pfSense LAN | `192.168.1.1/24` | Private (VMnet1) | LAN gateway and firewall |
| Kali Linux | `192.168.1.101/24` | Private (VMnet1) | Nmap traffic source |
| Metasploitable | `192.168.1.100/24` | Private (VMnet1) | Authorized lab target |

## Procedure

### 1. Create and connect the virtual machines

1. Create the pfSense, Kali, and Metasploitable virtual machines in VMware.
2. Connect pfSense’s WAN adapter to NAT (VMnet8) and its LAN adapter to the private network (VMnet1).
3. Connect Kali and Metasploitable to VMnet1. The two test machines should remain on the private lab network.
4. Start the VMs and confirm that the intended virtual adapters are connected.

### 2. Configure the pfSense LAN

1. From the pfSense console or WebGUI, assign the interfaces so the WAN uses VMnet8 and LAN uses VMnet1. In this lab the LAN interface is `em1`.
2. Set the pfSense LAN IPv4 address to `192.168.1.1/24`.
3. Set Kali to `192.168.1.101/24` and Metasploitable to `192.168.1.100/24`.
4. Use `192.168.1.1` as the default gateway on the LAN hosts when a gateway is needed.
5. Check local connectivity between the hosts and pfSense before testing IDS behavior. For example, from Kali:

   ```bash
   ping -c 4 192.168.1.1
   ping -c 4 192.168.1.100
   ```

### 3. Configure LAN firewall rules

In pfSense, open **Firewall → Rules → LAN**. Add only the pass rules needed for the lab, with the LAN network or required host as the source and the appropriate destination. Use TCP/UDP as appropriate for the service.

| Action | Protocol and port | Use |
|---|---|---|
| Pass | TCP/UDP 53 | DNS queries |
| Pass | TCP 80 | HTTP |
| Pass | TCP 443 | HTTPS |
| Block | Any other traffic not required by the lab | Reduce unnecessary access |

Place specific pass rules before a final block rule if using an explicit catch-all block. Review the existing rules and rule order before applying changes. pfSense evaluates rules on the interface where traffic enters; the LAN rules govern traffic arriving from VMnet1. Rules should be scoped to the intended lab hosts and services rather than exposing services broadly.

### 4. Install Snort

1. Sign in to the pfSense WebGUI.
2. Open **System → Package Manager → Available Packages**.
3. Find and install **Snort**.
4. After installation, open **Services → Snort** and check that the Snort service page is available.

### 5. Configure Snort on the LAN interface

1. Open **Services → Snort → Snort Interfaces**.
2. Add the LAN interface (`em1`) as the monitored interface.
3. Save and apply the interface settings.
4. Review the interface configuration and confirm that it monitors the private LAN containing Kali and Metasploitable.

### 6. Update and enable the community rules

1. Open Snort’s **Global Settings** or rules update settings.
2. Enable the **GPLv2 Community Rules** source and save the setting.
3. Run the rules update and wait for it to complete.
4. In the rules or SID management page, enable the relevant downloaded rules for the LAN interface.
5. Apply the configuration and verify the interface/service status before generating traffic.

### 7. Generate test traffic from Kali

From Kali, scan the Metasploitable host on the isolated lab network. For example:

```bash
nmap -sV 192.168.1.100
```

The `-sV` option requests service/version detection. Run scans only against systems in the authorized lab. Record the command and the observed result in the lab report if additional experiment notes are available.

### 8. Verify IDS alerts

1. Open **Services → Snort → Alerts** in pfSense after generating the scan traffic.
2. Review recent alerts and correlate their timestamps with the Nmap scan.
3. If the alert list is empty, check that the Snort service is running, the LAN interface is enabled, rules were downloaded and enabled, and the scan traffic reaches the monitored interface.
4. A packet capture can confirm that packets reach the LAN interface; it is separate evidence from a Snort alert and should not be reported as an alert by itself.

## Result

The experiment was completed using the VMware topology described above, with pfSense LAN at `192.168.1.1`, Kali at `192.168.1.101`, and Metasploitable at `192.168.1.100`. The lab workflow covered LAN firewall configuration, Snort installation and LAN monitoring, GPLv2 Community Rules, Nmap traffic generation, and alert review. The experiment demonstrated the setup and use of pfSense and Snort in an isolated virtual lab. No specific alert count, rule name, scan finding, or screenshot is included here because those details were not provided for this README.

## Conclusion

This lab provided practice configuring a stateful firewall and an IDS in a virtual network. It also demonstrated a basic workflow for generating traffic from Kali and reviewing Snort alerts on pfSense.
