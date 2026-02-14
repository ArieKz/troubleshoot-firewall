# troubleshoot-firewall
While attempting to recreate the Lab I came accross an issue where i was unable to receive ping respionse back from Linux. In the wireshark I observed a stream of Ping reguests with no linux reply. I believed the error was due to the rules previous set to disrupt the perpetual ping the first time i completed the lab. however after trouble shooting with the help of chatgpt, the issues seemed to stem from the Windows VM itself. I Ran PowerShell as administrator and used ssh to open the linux vm, as well as keeping one open for the windows Vm. i ran a ICMP Echo Request on.  Every single ICMP Echo rule on Windows is: Enabled : False



Ping-linuxVm-from-windows- shell
<img width="1512" height="982" alt="Ping-linuxVm-from-windows- shell" src="https://github.com/user-attachments/assets/e9475f8a-6087-4f78-86ab-256b82b1b848" />

Connecting_2-LinuxVM-and observingSSH
<img width="1512" height="982" alt="Connecting_2-LinuxVM-and observingSSH" src="https://github.com/user-attachments/assets/fe872a5e-f291-473c-9529-0470050fdf36" />

Troubleshoot-Ping-FromLinuxVm-toWindowsVm
<img width="1512" height="982" alt="Troubleshoot-Ping-FromLinuxVm-toWindowsVm" src="https://github.com/user-attachments/assets/235bf641-2637-43fc-af7f-56bca77c0205" />

Fix for no return reply ping issues-- PS C:\Users\Labuser> netsh advfirewall firewall add rule name="Allow ICMPv4-In" protocol=icmpv4:8,any dir=in action=allow >> Ok. Every single ICMP Echo rule on Windows is: Enabled : False. which was why Wireshark says “no response found” and ping times out. first cConfirm the rule exists, netsh advfirewall firewall show rule name="Allow ICMPv4-In"
then Explicitly allow outbound, input command - netsh advfirewall firewall add rule name="Allow ICMPv4-Out" protocol=icmpv4 dir=out action=allow. whcih 
<img width="1512" height="982" alt="Fix for no return reply ping issues" src="https://github.com/user-attachments/assets/472c15c3-b258-4f7e-b26a-9f5479a514f4" />

The chatGPT trouble shooting solution fixed the firewall issue enabling ICMP Echo rules as true and allowing me to successfuly observe ping requests and replies within WireShark.
<img width="1512" height="982" alt="Ping-Window-Vm-to Linux_Vm" src="https://github.com/user-attachments/assets/99c65f63-b220-4751-9454-04cf6ee7c4a9" />

Ending the Linux Virtual Machine Connection inputing command exit and closing Lab.
<img width="1512" height="982" alt="exiting- LInuxVM-Connection" src="https://github.com/user-attachments/assets/13fc28a5-cfa1-4de3-983d-9802683da1c0" />

