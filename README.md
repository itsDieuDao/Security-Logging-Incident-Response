<h1>Security Logging & Incident Response</h1>

 

**Objective**: Set up **security logging**, detect suspicious activity, and create an **incident response plan** in a home lab.  
 <br/>


<h2>🛠 Step 1: Set Up Logging for Key Security Events </h2>
  
  <br/>  
  
<h3>🔹 Decide What to Log </h3>    
  ✅ **User Logins & Authentication Failures** - Detect brute-force attacks  
  ✅ **File & System Changes** - Identify unauthorized modifications  
  ✅ **Network Traffic & Firewalls Logs** - Spot suspicious connections  
  ✅ **Security Software Alerts** - Monitor antivirus or IDS alerts  
  
  <br/>  

  
<h2>🔹 Step 1.1: Configure Security Logging in Windows & Linux </h2>
     
🖥  **Windows: Enable Security Logs**  

📌 **Enable Advanced Audit Policies:**  

  1. Open **Group Policy Editor (gpedit.msc)**
  2. Navigate to:
     `Computer Configuration > Windows Settings > Security Settings > Advanced Audit Policy Configuration > Audit Policies`
  3. Enable **Audit Logon Events, Audit Account Logon, and Audit Object Access.**  
<br/>   
  
📌 **Verify Security Logs in Event Viewer**  

     Get-EventLog -LogName Security -Newest 10  

<h3> 🐧 **Linux: Enable Logging for Security Events**  

📌 **Monitor SSH Login Attempts**  

    sudo cat /var/log/auth.log | grep "Failed password"  
  
📌 **Track File Changes in /etc/ **  

    sudo auditctl -w /etc/passwd -p wa -k passwd_changes  

📌 **List All Failed Login Attempts**  

    sudo lastb  

✅ **These logs help detect failed logins, privilege escalation, and file tampering. **  

 
<h2> 🛠 Step 2: Deploy an Intrustion Detection System (IDS) </h2>  

<h3> 🔹 Install & Configure IDS </h3>  

 **Windows: Enable Windows Defender Attack Surface Reduction Rules**  

      Set-MpPreference -AttackSurfaceReductionRules_Actions Enabled  

 **Linux: Install & Configure Fail2Ban for SSH Protection**  

      sudo apt install fail2ban -y
      sudo systemctl enable fail2ban  

 📌 **Review Fail2Ban logs for blocked IPs:**  

      sudo cat /var/log/fail2ban.log  

 ✅ **This blocks brute-force attacks on SSH logins.**  

   
<h2>🛠 Step 3: Detect & Respond to Security Incidents </h2>  
  
<h3> 🔹 Simulate an Incident & Investigate Logs </h3>  

  
<h2> Scenario 1: Unauthorized Login Attempt (Brute-Force Attack)  </h2> 

  📌 **Generate failed login attempts on purpose:**  

       ssh wronguser@yourserver  

  📌 **Check logs for repeated failures:**  

       sudo cat /var/log/auth.log | grep "Failed password"  
  
 📌 **Block the attacker's IP manually:**  

       sudo iptables -A INPUT -s <attacker_IP> -j DROP  
  

       
 <h2>Scenario 2: Unauthorized File Modification (Malware or Insider Threat) </h2>  
  
  🔧 Tasks:  
  1. **Run a Compliance Scan (NIST, CIS Benchmarks)**
  
         sudo apt install lynis -y
         sudo lynis audit system  
  - This will generate a **security report** based on NIST controls.
  
  2. **Set Up Basic Firewall Rules (UFW)**

         sudo ufw enable
         sudo ufw allow OpenSSH
         sudo ufw allow 443/tcp
         sudo ufw status verbose
  - This ensures only secure traffic is allowed.  

<h2>🛠 Step 5: Compliance Validation & Reporting  <br/>  
     
🔹 Generate a Compliance Report  </h2>  
  
 1. **Windows: Check compliance settings**  
  
        Get-GPOReport -All -ReportType HTML -Path C:\GPO_Report.html
    - This exports a **Group Policy** compliance report.
  
2. *Linux: Review Lynis Report**

       cat /var/log/lynis-report.dat
    - Look for **security misconfigurations**

3. **Analyze Security Logs with Splunk/Wazuh**
    - If you set up **Splunk**, create a **dashboard** to track compliance events.

<h2>🛠 Step 6: Write a Compliance Summary Report</h2>  

🎯 Your Deliverable: A **1-Page Compliance Report** summarizing:  

✅ Controls Implemented  
✅ Findings from Compliance Tools  
✅ Areas for Improvement  
✅ Next Steps  

<h3> 📄 Example Summary: </h3>  

**Security Compliance Report - NIST CSF Implementation in Home Lab**  
**Date:** \[Your Date]  
**Compliance Framework:** NIST Cybersecurity Framework (CSF) 

<h3> 🔹 Summary of Controls Implemented </h3>  

✅ Access Control (Windows & Linux GPOs) → PROTECT  
✅ Security Logging Enabled (Splunk/Wazuh) → DETECT  
✅ Firewall Rules (UFW, pfSense) → PROTECT  
✅ Audit Logs Reviewed → DETECT & RESPOND  

<h3> 🔹 Findings </h3>  

📌 **Windows Server Compliance Scan**: Passed 8/10 controls (Weak password policy detected)  
📌 **Linux Security Audit (Lynis)**: Hardening score 85/100 (Need SSH restriction improvement)  
📌 **Splunk Logs**: Detected 3 failed login attempts (Possible brute force)  
  
<h3> 🔹 Areas for Improvement </h3>  

  - Enforce **MFA** for **Windows Login**  
  - Implement **automatic remidiation scripts** for failed login detection

<h3> 🔹 Next Steps </h3>  

   1. Set up **Automated Compliance Monitoring** (e.g., Wazuh SIEM rules)
   2. Perform **Continuous Auditing** (Scheduled CIS Benchmark scans)

  


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
