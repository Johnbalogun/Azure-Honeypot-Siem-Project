0</head>
<body>
<h1>Azure Siem HoneyPot Project</h1>

<h1> 

<h2>Description</h2>
<h1> 

In this project i spun up a vulnerable virtual machine in Azure acting as a HoneyPot for attacks, Ingested logs of the Attempts and plotted it on a world map. After some observation I then Hardened the security of the environment.
<br />
<h2>Tools and Utilities Used</h2>
Azure Sentinal <br />
Windows Remote Desktop <br />
PowerShell <br />
Windows Firewall <br />
Azure Log Analytics <br />
<br />

<h1> </h1>

<h2> Walk-through:</h2>
<h1> 

<p align="center">

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<p><b>Spun up a vulnerable virtual machine in Azure and made it discoverable and enticing.</b></p>
<p><b>Create a resource group to house the virtual machine, when creating the VM, I created a new inbound rule that allows everything in for the sake of the honey pot test.</b></p>
  
<img src="https://i.imgur.com/onL3WI1.png" width="603" height="288">

<p><b>Created a log repository in Azure called Log Analytic workspace which was used to ingest the logs from my virtual machine. Also Ingested the windows event logs and created custom logs with geo information to discover where the attacks were coming from</b></p>

<img src="https://i.imgur.com/o5nriay.jpeg[/img]" alt="Log Analytics" width="606" height="286">

<p><b>Set up Azure Sentinel within Azure, Microsoft’s cloud native SIEM which was connected to the log analytic workspace to create a map while tracking all the attacker data i.e country, latitude and longitude.</b></p>

<img src="https://i.imgur.com/JQkMiKr.png[/img]" alt="Azure Sentinel" width="623" height="271">

<p><b>Logged into my VM remotely via Windows Remote Desktop using the VM’s public IP address</b></p>
<img src="https://i.imgur.com/KegntUb.png[/img]" alt="Windows Remote Desktop" width="269" height="289">

<p><b>Turned off the firewall in Windows Defender for easier discovery and observed the event viewer logs to see the security events attempted logins.</b></p>
<p><b>Made use of PowerShell to extract the IP addresses from the Windows logs sending it to a third-party API to derive the necessary details and send it back to the custom machine which created a custom log with geographic data inside.</b></p>

<img src="https://i.imgur.com/tOGZmbH.png[/img]" width="623" height="372">

<p><b>I inserted a custom API key (highlighted) into the readymade PowerShell script below in order to get the geo data</b></p>

<img src="https://i.imgur.com/AV4dMkU.png[/img]" alt="Custom API Key" width="624" height="360">

<p><b>I created custom fields and extract fields in log analytics for the data that’s being extracted. I then went on to set up the geo map in Sentinel and ran the query</b></p>
<pre>&lt;&lt; FAILED_RDP_WITH_GEO_CL | summarize event_count=count() by sourcehost_CF, latitude_CF, longitude_CF, country_CF, label_CF, destinationhost_CF | where destinationhost_CF != "samplehost" | where sourcehost_CF != "" &gt;&gt;</pre>

Several attacks were logged and plotted on different parts of the map

<img src="https://i.imgur.com/QMmdumj.png[/img]" alt="Geo Map" width="623" height="301">

<p><b>More attempted brute force attacks came in over time from different countries</b></p>

<img src="https://i.imgur.com/IzvAtTp.png[/img]" alt="Brute Force Attacks" width="621" height="298">


</head>
<body>

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

</body>
</html>

<p><b>Conclusion</b></p>
<p><b>I enhanced the security of Azure resources by implementing several hardening measures and security controls:</b></p>
<ol>
    <li><p><b>Network Security Groups (NSGs):</b> NSGs were tightened by blocking all inbound and outbound traffic except for connections originating from authorized sources, such as my own public IP address. This ensured that only trusted traffic could access the virtual machines.</p></li>
    <li><p><b>Built-in Firewalls:</b> The built-in firewalls on the virtual machines were configured to impose strict access controls, safeguarding the resources from unauthorized connections. Fine-tuning of firewall rules was conducted to tailor protection to the specific needs of each VM, thereby reducing the potential attack surface.</p></li>
    <li><p><b>Private Endpoints:</b> Public endpoints were replaced with Private Endpoints to bolster the security of Azure resources. This measure restricted access to sensitive resources, such as storage accounts and databases, solely to the virtual network, shielding them from exposure to the public internet. Consequently, these resources were safeguarded against unauthorized access and potential attacks.</p></li>
</ol>
<p><b




