<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

</head>
<body>
<h1>Azure Siem HoneyPot Project</h1>
<p><b>Spun up a vulnerable virtual machine in Azure and make it discoverable and enticing.</b></p>
<p><b>Create a resource group to house the virtual machine, when creating the VM, I created a new inbound rule that allows everything in for the sake of the honey pot test.</b></p>
  
<img src="1a53c23686e12ecf107d6f38d2cf11fe_html_2ac67257.jpg" alt="Azure VM" width="603" height="288">

<p><b>Created a log repository in Azure called Log Analytic workspace which was used to ingest the logs from my virtual machine. Also Ingested the windows event logs and created custom logs with geo information to discover where the attacks were coming from</b></p>

<img src="1a53c23686e12ecf107d6f38d2cf11fe_html_8992b7df.jpg" alt="Log Analytics" width="606" height="286">

<p><b>Set up Azure Sentinel within Azure, Microsoft’s cloud native SIEM which was connected to the log analytic workspace to create a map while tracking all the attacker data i.e country, latitude and longitude.</b></p>

<img src="1a53c23686e12ecf107d6f38d2cf11fe_html_c89d5e62.png" alt="Azure Sentinel" width="623" height="271">

<p><b>Logged into my VM remotely via Windows Remote Desktop using the VM’s public IP address</b></p>
<img src="1a53c23686e12ecf107d6f38d2cf11fe_html_76e8f2ed.png" alt="Windows Remote Desktop" width="269" height="289">

<p><b>Turned off the firewall in Windows Defender for easier discovery and observed the event viewer logs to see the security events attempted logins.</b></p>
<p><b>Made use of PowerShell to extract the IP addresses from the Windows logs sending it to a third-party API to derive the necessary details and send it back to the custom machine which created a custom log with geographic data inside.</b></p>

<img src="1a53c23686e12ecf107d6f38d2cf11fe_html_1053951b.png" alt="PowerShell" width="623" height="372">

<p><b>I inserted a custom API key (highlighted) into the readymade PowerShell script below in order to get the geo data</b></p>

<img src="1a53c23686e12ecf107d6f38d2cf11fe_html_e3d79c46.png" alt="Custom API Key" width="624" height="360">

<p><b>I created custom fields and extract fields in log analytics for the data that’s being extracted. I then went on to set up the geo map in Sentinel and ran the query</b></p>
<pre>&lt;&lt; FAILED_RDP_WITH_GEO_CL | summarize event_count=count() by sourcehost_CF, latitude_CF, longitude_CF, country_CF, label_CF, destinationhost_CF | where destinationhost_CF != "samplehost" | where sourcehost_CF != "" &gt;&gt;</pre>

<img src="1a53c23686e12ecf107d6f38d2cf11fe_html_b13cbdb5.png" alt="Geo Map" width="623" height="301">

<p><b>More attempted brute force attacks came in over time from different countries</b></p>

<img src="1a53c23686e12ecf107d6f38d2cf11fe_html_366a9c20.png" alt="Brute Force Attacks" width="621" height="298">

<table width="262" cellpadding="7" cellspacing="0">
    <col width="116"/>
    <col width="116"/>
    <tr valign="top">
        <td width="116" height="1" style="border: 1px solid #000000; padding: 0in 0.08in"><p align="left" style="orphans: 2; widows: 2"><font face="Calibri, serif"><font size="2" style="font-size: 11pt"><b>Locations</b></font></font></p></td>
        <td width="116" style="border: 1px solid #000000; padding: 0in 0.08in"><p align="left" style="orphans: 2; widows: 2"><font face="Calibri, serif"><font size="2" style="font-size: 11pt"><b>Attempts</b></font></font></p></td>
    </tr>
    <tr valign="top">
        <td width="116" height="2" style="border: 1px solid #000000; padding: 0in 0.08in"><p align="left" style="orphans: 2; widows: 2"><font face="Calibri, serif"><font size="2" style="font-size: 11pt"><b>Russia</b></font></font></p></td>
        <td width="116" style="border: 1px solid #000000; padding: 0in 0.08in"><p align="left" style="orphans: 2; widows: 2"><font face="Calibri, serif"><font size="2" style="font-size: 11pt"><b>2600</b></font></font></p></td>
    </tr>
    <!-- Additional rows for other countries -->
</table>
<p><b>Conclusion</b></p>
<p><b>I enhanced the security of Azure resources by implementing several hardening measures and security controls:</b></p>
<ol>
    <li><p><b>Network Security Groups (NSGs):</b> NSGs were tightened by blocking all inbound and outbound traffic except for connections originating from authorized sources, such as my own public IP address. This ensured that only trusted traffic could access the virtual machines.</p></li>
    <li><p><b>Built-in Firewalls:</b> The built-in firewalls on the virtual machines were configured to impose strict access controls, safeguarding the resources from unauthorized connections. Fine-tuning of firewall rules was conducted to tailor protection to the specific needs of each VM, thereby reducing the potential attack surface.</p></li>
    <li><p><b>Private Endpoints:</b> Public endpoints were replaced with Private Endpoints to bolster the security of Azure resources. This measure restricted access to sensitive resources, such as storage accounts and databases, solely to the virtual network, shielding them from exposure to the public internet. Consequently, these resources were safeguarded against unauthorized access and potential attacks.</p></li>
</ol>
<p><b

