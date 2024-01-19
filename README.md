# honeypot-project #
Project Overview:<br />
Created and deployed a VM via Azure, exposed the machine as a honeypot example, and monitored resulting traffic in Microsoft Sentinel.

--- 

Here's an overview of the setup of the VM, running Windows 10, in Azure. After deploying the machine, I created a custom NIC configuration to expose it and allow all traffic. Then, I created a Log Analytics Workspace and linked it to the VM, with Microsoft Defender for Cloud configured to store all security events via Event Viewer.<br /><br />

![01](https://github.com/astrowstorer/honeypot-project/assets/156803402/05e1ee04-aed7-4ac4-aa2d-71d972ee6df7)

---

After connecting to the VM via Windows Remote Desktop and configuring basic controls, I inspected the security logs as a baseline.<br /><br />
![02](https://github.com/astrowstorer/honeypot-project/assets/156803402/86b9e27d-cc7f-4b57-86cb-d01aceb51c25)

---

Trying to ping the VM from my personal machine to establish external traffic was constantly timing out…<br /><br />
![03](https://github.com/astrowstorer/honeypot-project/assets/156803402/8f41bfb0-c2ee-43d8-993e-449bb7c8663d)<br /><br />

To allow traffic, I disabled all firewall controls...<br /><br />
![04](https://github.com/astrowstorer/honeypot-project/assets/156803402/49e62d04-0475-4a59-8a90-a3f85fce8840)<br /><br />

Allowing me to establish traffic via the ping.<br /><br />
![05](https://github.com/astrowstorer/honeypot-project/assets/156803402/775cb26f-4ebd-43e5-aa03-780227cffb15)

---

I implemented a custom PowerShell script (from: [source](https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1)) with an integrated IP geolocation API to collect data from failed RDP login attempts. I was hoping these attempts would start coming in over the next few hours now that the machine was exposed to the internet.<br /><br />

![06](https://github.com/astrowstorer/honeypot-project/assets/156803402/872d2a41-6a70-4471-9810-3eca88bd3627)<br /><br />

The PowerShell script contains some sample outputs for training field detection in Microsoft Sentinel, but I tested it with some intentionally failed attempts from my own machine to demonstrate malicious logins.

---

I ingested the log data from the PowerShell script into Azure as a new table in the LAW and used that to create a new workbook in Sentinel, allowing me to visualize the data as a dashboard map. Surprisingly, in the few hours I left this project live until the next morning, no other failed RDP login attempts came in, but I decided to take it down after it served its purpose.<br /><br />

![07](https://github.com/astrowstorer/honeypot-project/assets/156803402/5d0b3763-8a9b-46b9-be83-3a6906b12060)<br /><br />
Here's a look at the visualization of the data in Sentinel, excluding the training data but including my own simulated malicious failed login attempts via RDP.
