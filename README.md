# troubleshoot-firewall

<p align="center">
<img width="617" height="219" alt="Screenshot 2026-02-15 at 10 31 42 AM" src="https://github.com/user-attachments/assets/80dd86d9-53e4-4d16-9990-1f9e8712dc62" />


<h1>Inspecting Network Protocols: Pre-Production Through Network Traffic Monitoring</h1>
This tutorial follows the creation and deployment of Virtual Machines- observation of Network traffic within the public cloud computing platform Microsoft Azure.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- MacOS</b> 
- Windows 10</b> 
- Linux</b>

<h2>Troubleshooting Networking Traffic Issues</h2>

- Observe Failed ICMP Traffic
- Open Dual Windows/Linux PowerShell
- Test No Return/Reply Ping Between VMs
- ChatGPT & Enabling ICMP Echo rules

While attempting to recreate a previous Lab on Network monitoring, for documentation purposes. I came across an issue where I was no longer able to receive a ping response from Linux. In Wireshark, I continued to observe a stream of Ping requests with no Linux reply. I believed the error was due to operator error, and the rules I had previously set in Azure to disrupt a perpetual ping. After assessing my Azure security group setup, I found that all rules were disabled, yet the issues persisted. Therefore, I began troubleshooting with the help of ChatGPT.
<img width="1512" height="982" alt="Ping-linuxVm-from-windows- shell" src="https://github.com/user-attachments/assets/e9475f8a-6087-4f78-86ab-256b82b1b848" />

I ran PowerShell as an administrator and used the command (ssh-username-privateIP) to open the Linux VM, as well as run the Windows Vm to communicate between the two machines. 
<img width="1512" height="982" alt="Connecting_2-LinuxVM-and observingSSH" src="https://github.com/user-attachments/assets/fe872a5e-f291-473c-9529-0470050fdf36" />

At this point, I still believed that the Linux firewall was blocking the ping request from the Windows VM. ChatGPT suggested running ICMP Third check/ iptables directly (sudo iptables -L -n), with the intention to look for any rule that drops the ICMP. Then, to temporarily (flush (diagnostic only)/sudo iptables -F). I also ran (icmp tcpdump), showing that Linux was (listening on eth0). This did not resolve the issue; however, it did show that Linux was receiving the ping. I tried to ping a public website from the Windows VM, but there was no return response, so I began investigating the Windows VM. Windows Defender Firewall issue, this was the final Fix for no return reply ping issues. First, I attempted to restart the firewall service with (Restart-Service mpssvc), which was blocked by Azure. ChatGPT requested to confirm the rule exists, command (allow ICMPv4-In) to verify, and force-enable, which would allow for outbound traffic. The overall issue was that every single ICMP Echo rule on Windows VM was (Enabled : False), causing no response found, and ping times out. 
<img width="1512" height="982" alt="Fix for no return reply ping issues" src="https://github.com/user-attachments/assets/472c15c3-b258-4f7e-b26a-9f5479a514f4" />

The ChatGPT troubleshooting solution fixed the firewall issue by enabling ICMP Echo rules as true. Allowing for successful observation of ping requests and replies within Wireshark.
<img width="1512" height="982" alt="Ping-Window-Vm-to Linux_Vm" src="https://github.com/user-attachments/assets/99c65f63-b220-4751-9454-04cf6ee7c4a9" />

Ending the Linux Virtual Machine connection with the command (exit) and closing, after documenting the steps to complete the Performing Activities on the Network Lab.
<img width="1512" height="982" alt="exiting- LInuxVM-Connection" src="https://github.com/user-attachments/assets/13fc28a5-cfa1-4de3-983d-9802683da1c0" />

