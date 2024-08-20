![image](https://github.com/user-attachments/assets/2a615584-5e30-4cd4-a1fa-ae0cefe2dfe4)

<h1>Observing Network Traffic in Azure</h1>
In this analysis, I am examining various types of network traffic to and from virtual machines within Azure using Wireshark, while also experimenting with network security groups in the same environment.  <br />



<h2>Technologies and Platforms</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Connection
- Command-Line Tools (PowerShell)
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems </h2>

- Windows 10 Pro (22H2)
- Ubuntu Server 24.04

<h2>Deployment</h2>

In Azure, I created two VMs within the same virtual network to enable communication between them. One VM runs Windows 10, while the other runs Ubuntu. The Windows VM will connect to the Ubuntu VM using PowerShell's command line. 

<h2>Procedure</h2>

![image](https://github.com/user-attachments/assets/dc9d1ebd-6ac4-4cc9-aa48-2195634aec8d)
![image](https://github.com/user-attachments/assets/490f550c-5d3a-4771-a28d-5c06f726fcd1)

I connected to the Windows VM using Remote Desktop Connection (RDP) through its public IP address. Once connected, I installed Wireshark to begin inspecting network traffic.
</p>
<br />

![image](https://github.com/user-attachments/assets/e3a7b0f2-8ef5-4352-a847-e8eb990ba4e0)
![image](https://github.com/user-attachments/assets/de90f4b2-8697-4990-925d-1fa51a53ce12)


In Wireshark, I filtered for Internet Control Message Protocol (ICMP) traffic and used PowerShell to execute a ping command. ICMP, which ping relies on, is used to monitor network traffic. In this instance, ping was employed to check communications with the Ubuntu VM using its private IP address, and subsequently with Google. I then performed a continuous ping to the Ubuntu VM to test network security groups within Azure, using the command 'ping <IP address> -t'. 

![image](https://github.com/user-attachments/assets/f486c2a6-d6a2-4511-b377-d52e1ad88cf2)

In the Azure portal, I adjusted the Ubuntu VM's networking settings to block ICMP traffic by adding an inbound security rule. The priority value of 259 ensures this rule is applied before the rule with a priority of 300 (SSH). 
</p>

![image](https://github.com/user-attachments/assets/1288c0a1-dde1-4be5-8a6b-c88c0b34e849)

Upon returning to the Windows VM, I observed that ICMP traffic is now blocked due to the newly established inbound security rule. After reverting the rule, the continuous ping resumed without timing out. 
</p>
<br />

![image](https://github.com/user-attachments/assets/a88ff510-70d4-4faf-b3dc-a3668205bd7b)

Additionally, I logged into the Ubuntu server via PowerShell using the SSH command on port 22.  By fintering for "tcp.port=22" in Wireshark, or alternatively "ssh," we can examine the corresponding traffic. Each command executed is subsequently reflected in Wireshark. 
</p>
<br />

![image](https://github.com/user-attachments/assets/1d54dae8-5685-42db-ab20-f71208b89ecf)

I then terminated the communication with the Ubuntu system to oserve DHCP traffic. Activity was evident after executing the command ipconfig /renew to obtain a new IP address.
</p>
<br />

![image](https://github.com/user-attachments/assets/74abdcf7-15df-4ede-bc1d-40ee42796c2c)

I then filterered for DNS traffic by specifiying UDP port 53 and executed the command nslookup google.com.
<br />

![image](https://github.com/user-attachments/assets/71f1b532-ad6a-4439-a516-6d16fb800463)

<p>
Finally, I filtered for RDP traffic, which uses the protocol on TCP port 3389. The analysis revealed a significant volume of traffic due to the active RDP session with the Windows VM. 
</p>
<br />

<h2>Key Takeaways</h2>

The purpose of this activity was to observe the utilization of protocols and ports in a network between devices. This experience is crucial for skill development, particularly in troubleshooting. The goal is to become proficient with various tools to cultivate a deeper understanding of network operations.
