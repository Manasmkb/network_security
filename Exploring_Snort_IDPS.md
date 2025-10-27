# Exploring Snort (Intrusion Detection and Prevention System) — Assignment 6

This lab focuses on installing, configuring, and running **Snort** on Ubuntu to understand the fundamentals of an **Intrusion Detection and Prevention System (IDPS)**.  
Snort monitors network packets, matches them against defined rules, and alerts when suspicious activity is detected.

---

## ✅ Step 1 – Update the System

```bash
sudo apt update
sudo apt upgrade -y
```

**Screenshot:** `![update_system](screenshots/01_update_system.png)`  
**Explanation:**  
Updating ensures your package lists, dependencies, and security patches are current, reducing vulnerabilities before installing Snort.

---

## ✅ Step 2 – Install Snort

```bash
sudo apt install snort -y
```

During installation you’ll be prompted for:  
- **Network Interface:** enter your interface name (`enX0`, `eth0`, `ens33`, etc.).  
  Find it with: `ip a`  
- **HOME_NET:** enter your network range (e.g., `192.168.1.0/24`).

**Screenshot:** `![install_snort](screenshots/02_install_snort.png)`  
**Explanation:**  
This installs Snort and its default configs under `/etc/snort/`.

---

## ✅ Step 3 – Configure Snort

Open the configuration file:

```bash
sudo nano /etc/snort/snort.conf
```

Locate and verify:
```bash
ipvar HOME_NET 192.168.1.0/24
```

**Screenshot:** `![snort_conf](screenshots/03_snort_conf.png)`  
**Explanation:**  
`HOME_NET` defines your protected network. Matching this to your VM’s range ensures Snort analyzes relevant traffic.

---

## ✅ Step 4 – Update and Manage Rules

### (a) Download community rules
```bash
sudo wget https://www.snort.org/downloads/community/community-rules.tar.gz
sudo tar -xvzf community-rules.tar.gz
sudo cp community-rules/* /etc/snort/rules/
```

### (b) Add a custom rule
```bash
sudo nano /etc/snort/rules/local.rules
```
Insert:
```bash
alert icmp any any -> any any (msg:"ICMP detected"; sid:1000001; rev:1;)
```

**Screenshot:** `![local_rules](screenshots/04_local_rules.png)`  
**Explanation:**  
Rules define how Snort detects malicious behavior.  
This rule triggers an alert whenever ICMP traffic (ping) is seen.

**Answer:**  
Rules act as the *signature definitions* that tell Snort **what to look for and how to respond**, forming the detection logic of the IDS engine.

---

## ✅ Step 5 – Test Configuration

```bash
sudo snort -T -c /etc/snort/snort.conf
```

Expected message:
```
Snort successfully validated the configuration!
```

**Screenshot:** `![test_config](screenshots/05_test_config.png)`  
**Explanation:**  
`-T` performs a dry-run syntax check to ensure the configuration and rule files are valid.

---

## ✅ Step 6 – Run Snort in IDS Mode

```bash
sudo snort -c /etc/snort/snort.conf -i enX0
```
*(replace `enX0` with your actual interface)*  

**Screenshot:** `![snort_ids_mode](screenshots/06_snort_ids_mode.png)`  
**Explanation:**  
This runs Snort in **Intrusion Detection System** mode, reading traffic on the interface and alerting when rules match.  
Press **Ctrl + C** to stop.

---

## ✅ Step 7 – View Snort Logs

Logs are saved in:
```
/var/log/snort/
```

List them:
```bash
ls -lh /var/log/snort/
```

**Screenshot:** `![snort_logs](screenshots/07_snort_logs.png)`  
**Answer:**  
You’ll typically find `alert`, `snort.log.<timestamp>`, and possibly empty files if no traffic matched your rules.  
If `alert` is empty, no rule-triggering events occurred yet.

---

## ✅ Step 8 – Run Snort as a Daemon (Background Process)

Start Snort in the background:
```bash
sudo snort -D -c /etc/snort/snort.conf -i enX0
```

Check running processes:
```bash
top
```

**Screenshot:** `![snort_daemon](screenshots/08_snort_daemon.png)`  

**To stop Snort:**
1. Find its PID:
   ```bash
   ps aux | grep snort
   ```
2. Terminate it:
   ```bash
   sudo kill <PID>
   ```

**Explanation:**  
Running as a daemon allows continuous monitoring even after you log out.

---

## 🧠 Discussion & Answers

### Q 1 – Purpose of Rules
Rules instruct Snort **what patterns, ports, or payloads to inspect**, forming the logic that detects intrusions or suspicious traffic.

### Q 2 – Why might log files be empty?
If there’s no network activity matching any rule, Snort will not log alerts. Generating ICMP (ping) traffic helps trigger your custom rule.

### Q 3 – How to trigger alerts manually?
From another terminal or host:
```bash
ping <your_VM_IP>
```
Then check:
```bash
sudo cat /var/log/snort/alert
```
You should see your custom `"ICMP detected"` message.

---

## ⚙️ Useful Commands Reference

| Purpose | Command |
|----------|----------|
| Validate configuration | `sudo snort -T -c /etc/snort/snort.conf` |
| Run Snort in IDS mode | `sudo snort -c /etc/snort/snort.conf -i <interface>` |
| Run as background daemon | `sudo snort -D -c /etc/snort/snort.conf -i <interface>` |
| View logs | `sudo tail -f /var/log/snort/alert` |
| Stop Snort process | `sudo kill <PID>` |

---

## ✅ Submission Checklist

- [ ] Screenshots for every major step.  
- [ ] Answers added under each question.  
- [ ] Pushed to GitHub.  

Commit example:
```bash
git add .
git commit -m "Assignment 6: Snort IDPS installation and configuration"
git push
```

---

## 🧩 Reflection

Snort demonstrates how an **IDPS inspects network packets**, compares them against **rules and signatures**, and provides actionable alerts for suspicious activity.  
This hands-on lab shows the value of layered defense—visibility, rule tuning, and real-time detection.
