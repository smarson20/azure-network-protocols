<p align="center">
  <img src="https://learn.g2.com/hubfs/G2CM_FI634_Learn_Article_Images_%5BNetwork_traffic_analysis%5D_V1b.png" height="70%" width="70%" alt="Visual representation of network traffic" />
</p>

<h1>Exploring Network Security Groups (NSGs) and Network Traffic Inspection Between Azure Virtual Machines</h1>

<p>
  This tutorial provides a hands-on walkthrough for inspecting network traffic between virtual machines (VMs) within the Microsoft Azure ecosystem using Wireshark. Additionally, it explores how Network Security Groups (NSGs) can be configured to manage and control traffic flows. A foundational understanding of network behavior is essential for IT professionals in diagnosing performance bottlenecks, ensuring compliance, and protecting digital infrastructure from malicious activity.
</p>

<h2>Technologies and Environments Utilized</h2>
<ul>
  <li>Microsoft Azure (Virtual Machines)</li>
  <li>Remote Desktop Protocol (RDP)</li>
  <li>Command-Line Tools (PowerShell, SSH)</li>
  <li>Key Network Protocols: SSH, RDP, DNS, DHCP, ICMP</li>
  <li>Wireshark (Open-source Protocol Analyzer)</li>
</ul>

<h2>Operating Systems Involved</h2>
<ul>
  <li>Windows 10 Pro (21H2)</li>
  <li>Ubuntu Server 20.04 LTS</li>
</ul>

<h2>Outline of Key Tasks</h2>
<ul>
  <li>Provision resource group and virtual machines</li>
  <li>Ensure both VMs are configured within the same virtual network</li>
  <li>Connect to the Windows VM via RDP</li>
  <li>Install and configure Wireshark for packet inspection</li>
  <li>Capture and analyze network traffic for multiple protocols</li>
  <li>Implement NSG rules and observe impact on traffic</li>
  <li>Deallocate and clean up resources to prevent unnecessary billing</li>
</ul>

<h2>Step-by-Step Guide</h2>

<h3>Create a Resource Group</h3>
<p>
  - In Azure Portal, navigate to "Resource Groups"<br>
  - Click <strong>Create</strong>, input required information<br>
  - Complete creation and verify the resource group exists
</p>
<p>
  <img src="https://i.imgur.com/w5yKzNW.png" height="80%" width="80%" alt="Azure Resource Group creation interface" />
</p>

<h3>Create a Windows 10 Virtual Machine</h3>
<p>
  - Go to "Virtual Machines" and click <strong>Create</strong><br>
  - Under Resource Group, select the one created earlier (e.g., <em>RG-Network-Activities</em>)<br>
  - Choose "Windows 10 Pro" as the image<br>
  - Fill in remaining fields (size, region, credentials, etc.)<br>
  - Finalize by clicking <strong>Review + Create</strong>
</p>
<p>
  <img src="https://i.imgur.com/2DAgUaY.png" height="80%" width="80%" alt="Assigning VM to resource group" />
  <img src="https://i.imgur.com/JyjH7kb.png" height="80%" width="80%" alt="Selecting Windows 10 image" />
</p>

<h3>Create a Linux (Ubuntu) Virtual Machine</h3>
<p>
  - Repeat the VM creation steps, this time selecting Ubuntu Server (22.04 or 24.04 LTS)<br>
  - Set "Authentication type" to "Password" and specify credentials<br>
  - Ensure the virtual network matches the one used for the Windows VM (e.g., <em>Lab2-vnet</em>)
</p>
<p>
  <img src="https://i.imgur.com/yYDqGp5.png" height="80%" width="80%" alt="Selecting Ubuntu image" />
  <img src="https://i.imgur.com/qHy8fEb.png" height="80%" width="80%" alt="Setting authentication type for Linux VM" />
  <img src="https://i.imgur.com/NQXygVj.png" height="80%" width="80%" alt="Selecting shared virtual network" />
</p>

<h3>Verify Network Consistency</h3>
<p>
  - Open both VMs in the Azure portal<br>
  - Confirm both are on the same virtual network and subnet<br>
  - This ensures internal communication and accurate packet inspection
</p>
<p>
  <img src="https://i.imgur.com/kMZdeJn.png" height="80%" width="80%" alt="Verifying VM network configuration" />
</p>

<h3>Remote into the Windows VM</h3>
<p>
  - Use Microsoft Remote Desktop to connect<br>
  - Obtain the VM’s public IP from the Azure dashboard<br>
  - Input it into the RDP client and connect
</p>
<p>
  <img src="https://i.imgur.com/Z2iVbds.png" height="80%" width="80%" alt="Using RDP to connect to VM" />
</p>

<h3>Install and Launch Wireshark</h3>
<p>
  - From within the Windows VM, download Wireshark from the official website<br>
  - Install and launch the application
</p>

<h3>Start Capturing Packets</h3>
<p>
  - Open PowerShell in Windows<br>
  - In Wireshark, begin a packet capture session<br>
  - Click the blue fin icon to initiate capture on the active network interface
</p>
<p>
  <img src="https://i.imgur.com/vCQosqd.png" height="80%" width="80%" alt="Starting Wireshark capture" />
</p>

<h3>ICMP (Ping) Traffic</h3>
<p>
  - In Wireshark, filter by <code>icmp</code><br>
  - Ping the Linux VM from PowerShell using its private IP (e.g., <code>ping 10.0.0.5</code>)<br>
  - Observe echo requests and replies<br>
  - Try continuous ping using <code>-t</code>, then disable ICMP via NSG rule and see the timeout behavior<br>
  - Re-enable ICMP and confirm recovery
</p>
<p>
  <img src="https://i.imgur.com/z72ALTZ.png" height="80%" width="80%" alt="Filtered ICMP traffic in Wireshark" />
  <img src="https://i.imgur.com/uvHhIX1.png" height="80%" width="80%" alt="Blocking ICMP via NSG" />
  <img src="https://i.imgur.com/VnxToy1.png" height="80%" width="80%" alt="ICMP timeout after NSG block" />
</p>

<h3>SSH Traffic</h3>
<p>
  - In Wireshark, filter by <code>ssh</code><br>
  - Use PowerShell to SSH into the Linux VM using <code>ssh username@privateIP</code><br>
  - Run a few Linux commands and observe traffic<br>
  - Exit SSH session when done
</p>
<p>
  <img src="https://i.imgur.com/5pkwhHU.png" height="80%" width="80%" alt="Inspecting SSH session" />
</p>

<h3>DHCP Traffic</h3>
<p>
  - Filter Wireshark using <code>dhcp</code><br>
  - In PowerShell, renew the IP using <code>ipconfig /renew</code><br>
  - Watch DHCP discovery and request packets in real time
</p>
<p>
  <img src="https://i.imgur.com/3YPCr3h.png" height="80%" width="80%" alt="DHCP traffic inspection" />
</p>

<h3>DNS Traffic</h3>
<p>
  - Filter Wireshark using <code>dns</code><br>
  - Use <code>nslookup</code> to resolve domain names and observe the DNS queries and responses
</p>
<p>
  <img src="https://i.imgur.com/9lIeSAN.png" height="80%" width="80%" alt="DNS queries and responses" />
</p>

<h3>RDP Traffic</h3>
<p>
  - Filter using <code>tcp.port == 3389</code><br>
  - Observe persistent traffic due to the live session between the local machine and the Windows VM
</p>
<p>
  <img src="https://i.imgur.com/uFEwWqO.png" height="80%" width="80%" alt="RDP session traffic" />
</p>

<h3>Clean Up Your Azure Resources</h3>
<p>
  - Close all remote connections<br>
  - Delete the resource group(s) you created to avoid additional Azure charges<br>
  - Confirm deletion in the Azure portal
</p>
<p>
  <img src="https://i.imgur.com/BxzCTKp.png" height="80%" width="80%" alt="Resource cleanup confirmation" />
</p>

<h3>Conclusion</h3>
<p>
  
  Through a brief tutorial, we were able to deploy Virtual Machines in Azure, utilized Wireshark to monitor newtork activity, and configured NSGs to control traffic flow.

</p>
