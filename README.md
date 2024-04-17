<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>

<h1>Azure Siem HoneyPot Project</h1>

<h2>Description</h2>
<p>In this project i spun up a vulnerable virtual machine in Azure acting as a HoneyPot for attacks, Ingested logs of the Attempts and plotted it on a world map. After some observation I then Hardened the security of the environment.</p>

<h2>Tools and Utilities Used</h2>
<p>Azure Sentinal <br />
Windows Remote Desktop <br />
PowerShell <br />
Windows Firewall <br />
Azure Log Analytics</p>

<h2>Walk-through:</h2>

<p align="center">
  <p><b>Spun up a vulnerable virtual machine in Azure and made it discoverable and enticing.</b></p>
  <p><b>Create a resource group to house the virtual machine, when creating the VM, I created a new inbound rule that allows everything in for the sake of the honey pot test.</b></p>
  
  <a href="https://i.imgur.com/onL3WI1.png" target="_blank"><img src="https://i.imgur.com/onL3WI1.png" width="603" height="288" alt="Step 1 Image"></a>

  <p><b>Created a log repository in Azure called Log Analytic workspace which was used to ingest the logs from my virtual machine. Also Ingested the windows event logs and created custom logs with geo information to discover where the attacks were coming from.</b></p>

  <a href="https://i.imgur.com/o5nriay.jpeg" target="_blank"><img src="https://i.imgur.com/o5nriay.jpeg" width="606" height="286" alt="Step 2 Image"></a>

  <p><b>Set up Azure Sentinel within Azure, Microsoft’s cloud native SIEM which was connected to the log analytic workspace to create a map while tracking all the attacker data i.e country, latitude and longitude.</b></p>

  <a href="https://i.imgur.com/JQkMiKr.png" target="_blank"><img src="https://i.imgur.com/JQkMiKr.png" width="623" height="271" alt="Step 3 Image"></a>

  <p><b>Logged into my VM remotely via Windows Remote Desktop using the VM’s public IP address</b></p>

  <a href="https://i.imgur.com/KegntUb.png" target="_blank"><img src="https://i.imgur.com/KegntUb.png" width="269" height="289" alt="Step 4 Image"></a>

  <p><b>Turned off the firewall in Windows Defender for easier discovery and observed the event viewer logs to see the security events attempted logins.</b></p>
  <p><b>Made use of PowerShell to extract the IP addresses from the Windows logs sending it to a third-party API to derive the necessary details and send it back to the custom machine which created a custom log with geographic data inside.</b></p>

  <a href="https://i.imgur.com/tOGZmbH.png" target="_blank"><img src="https://i.imgur.com/tOGZmbH.png" width="623" height="372" alt="Step 5 Image"></a>

  <p><b>I inserted a custom API key (highlighted) into the readymade PowerShell script below in order to get the geo data.</b></p>

  <a href="https://i.imgur.com/AV4dMkU.png" target="_blank"><img src="https://i.imgur.com/AV4dMkU.png" width="624" height="360" alt="Step 6 Image"></a>

</p>

<h2>Attempts by Location</h2>

<table>
    <tr>
        <th>Locations</th>
        <th>Attempts</th>
    </tr>
    <tr>
        <td>Russia</td>
        <td>2600</td>
    </tr>
    <tr>
        <td>Austria</td>
        <td>634</td>
    </tr>
    <tr>
        <td>Indonesia</td>
        <td>16200</td>
    </tr>
    <tr>
        <td>Germany</td>
        <td>36</td>
    </tr>
    <tr>
        <td>China</td>
        <td>55</td>
    </tr>
    <tr>
        <td>United States</td>
        <td>16</td>
    </tr>
    <tr>
        <td>Unknown</td>
        <td>12</td>
    </tr>
    <tr>
        <td>United Kingdom</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Netherlands</td>
        <td>1</td>
    </tr>
</table>

<p><b>Conclusion</b></p>
<p><b>I enhanced the security of Azure resources by implementing several hardening measures and security controls:</b></p>
<ol>
    <li><p><b>Network Security Groups (NSGs):</b> NSGs were tightened by blocking all inbound and outbound traffic except for connections originating from authorized sources, such as my own public IP address. This ensured that only trusted traffic could access the virtual machines.</p></li>
    <li><p><b>Built-in Firewalls:</b> The built-in firewalls on the virtual machines were configured to impose strict access controls, safeguarding the resources from unauthorized connections. Fine-tuning of firewall rules was conducted to tailor protection to the specific needs of each VM, thereby reducing the potential attack surface.</p></li>
    <li><p><b>Private Endpoints:</b> Public endpoints were replaced with Private Endpoints to bolster the security of Azure resources. This measure restricted access to sensitive resources, such as storage accounts and databases, solely to the virtual network, shielding them from exposure to the public internet. Consequently, these resources were safeguarded against unauthorized access and potential attacks.</p></li>
</ol>

</body>
</html>
