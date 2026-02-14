# troubleshoot-firewall
While attempting to recreate a previous Lab for documentation, I came across an issue where I was no longer able to receive ping response from Linux. In the Wireshark I continued to observe a stream of Ping requests with no Linux reply. I believed the error was due to rules I had previous set in Azure to disrupt a perpetual ping. All rules were disabled and yet the issues persisted, so I began troubleshooting with the help of ChatGPt.
<img width="1512" height="982" alt="Ping-linuxVm-from-windows- shell" src="https://github.com/user-attachments/assets/e9475f8a-6087-4f78-86ab-256b82b1b848" />

I ran PowerShell as an administrator and used command (ssh-username-privateIP) to open linux VM, as well as run the Windows Vm to communicate between the two machines.
<img width="1512" height="982" alt="Connecting_2-LinuxVM-and observingSSH" src="https://github.com/user-attachments/assets/fe872a5e-f291-473c-9529-0470050fdf36" />

I believed that it was the Linux firewall that was blocking the ping request from the Windows VM. ChatGPT suggested running ICMP Third check/ iptables directly (sudo iptables -L -n), with the intention to look for any rule that drops the ICMP. Then to temporarily (flush (diagnostic only)/  sudo iptables -F). I also ran (icmp tcpdump) showing that Linux was (listening on eth0). This did not resolve the issue, however, it did show that Linux was receiving the ping. I tried to ping public website from the Windows VM with no reply back and began investigating the Windows Vm. Windows Defender Firewall issue. This was the final Fix for no return reply ping issues. First I attempted to restart the firewall service with (Restart-Service mpssvc), which was blocked by Azure. ChatGPT requested to confirm the rule exists, command (allow ICMPv4-In) to verify and force-enable, which would allow for outbound traffic. The overall issue was that every single ICMP Echo rule on Windows VM was (Enabled : False), causing no response found, and ping times out. 
<img width="1512" height="982" alt="Fix for no return reply ping issues" src="https://github.com/user-attachments/assets/472c15c3-b258-4f7e-b26a-9f5479a514f4" />

The chatGPT trouble shooting solution fixed the firewall issue by enabling ICMP Echo rules as true, and allowing for successful observation of ping requests and replies within WireShark.
<img width="1512" height="982" alt="Ping-Window-Vm-to Linux_Vm" src="https://github.com/user-attachments/assets/99c65f63-b220-4751-9454-04cf6ee7c4a9" />

Ending the Linux Virtual Machine connection by inputing command (exit) and closing after documenting steps to complete the Performing Activities on the Network Lab.
<img width="1512" height="982" alt="exiting- LInuxVM-Connection" src="https://github.com/user-attachments/assets/13fc28a5-cfa1-4de3-983d-9802683da1c0" />

