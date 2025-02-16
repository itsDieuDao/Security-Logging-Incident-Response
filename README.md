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

<h2> 🐧 Linux: Enable Logging for Security Events  </h2>

📌 **Monitor SSH Login Attempts**  

    sudo cat /var/log/auth.log | grep "Failed password"  
  
📌 **Track File Changes in /etc/ **  

    sudo auditctl -w /etc/passwd -p wa -k passwd_changes  

📌 **List All Failed Login Attempts**  

    sudo lastb  

✅ **These logs help detect failed logins, privilege escalation, and file tampering. **  
</br>
 
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
</br>  

   
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
  
  📌 **Modify `/etc/passwd` file (simulate an attack):**  

       sudo echo "malicious_user:x:1002:1002::/home/malicious:/bin/bash" >> /etc/passwd  

  📌 **Detect unauthorized modification with audit logs: **  

       sudo ausearch -f /etc/passwd    
  
 📌 **Revert the change & investigate: **  

       sudo vi /etc/passwd  
  
✅ **These simulations help you practice forensic analysis!**  
</br>

  
<h2> 🛠 Step 4: Create an Incident Response Plan (IRP) </h2>  

 📌 Use the **SANS 6-Step Incident Response Process **  
  
 | Phase | Actions Taken | 
 | ----- | ------------- | 
 | 1. Preparation | Set up logging, IDS, and response tools | 
 | 2. Identification | Review logs for suspicious activity | 
 | 3. Containment | Block IPs, isolate infected systems | 
 | 4. Eradication | Remove malware, revoke compromised credentials | 
 | 5. Recovery | Restores systems, ensure no persistence | 
 | 6. Lessons Learned | Update security controls based on findings | 
  
</br>  
  
<h2> 📝 Sample Incident Response Report  </h2>  

**Incident Report - Unauthorized SSH Login Attempt**  
**Date:** \[2/16/2025]  
**Detected By:** Fail2Ban IDS  
**Incident Type:** Brute-force attack  
**Indicators of Compromise (IoC): **  
  - Repeated failed login attempts from IP `192.168.1.10`
  - SSH logs show `Failed password for root from 192.168.1.10`
  
<h3> 🔹 Actions Taken </h3>  

✅ **Blocked IP using firewall**  
✅ **Reviewed logs for further suspicious activity**  
✅ **Changed root password & enforced 2FA**  
  
<h3> 🔹 Next Steps </h3>   
  
   - Implement **automated threat detection**    
   - Enforce **account lockout policies**  
   - Conduct **team security awareness training**  

  


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
