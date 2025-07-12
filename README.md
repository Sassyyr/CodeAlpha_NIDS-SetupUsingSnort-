# 🛡️ Network Intrusion Detection System (NIDS) using Snort

This project outlines the setup of a **Network Intrusion Detection System (NIDS)** using **Snort**, a powerful open-source tool for real-time traffic analysis and threat detection.

---

## 📌 Objectives

- Deploy a Snort-based NIDS
- Detect and log suspicious network activity
- Create and test custom intrusion detection rules
- Monitor network traffic in real time

---

## 🛠️ Tools Used

| Tool            | Purpose                          |
|-----------------|----------------------------------|
| **Snort**       | Intrusion detection engine       |
| **Ubuntu Linux**| Deployment environment           |

---

## ⚙️ Installation Steps

### 1. System Update
```bash
sudo apt update && sudo apt upgrade -y
1.2 Install Required Packages
bash
Copy
Edit
sudo apt install -y build-essential libpcap-dev libpcre3-dev libdumbnet-dev bison flex zlib1g-dev liblzma-dev openssl libssl-dev
1.3 Download and Install DAQ (Data Acquisition Library)
bash
Copy
Edit
wget https://www.snort.org/downloads/snort/daq-2.0.7.tar.gz
tar -xvzf daq-2.0.7.tar.gz
cd daq-2.0.7
./configure && make && sudo make install
1.4 Download and Install Snort
bash
Copy
Edit
cd ~
wget https://www.snort.org/downloads/snort/snort-2.9.20.tar.gz
tar -xvzf snort-2.9.20.tar.gz
cd snort-2.9.20
./configure --enable-sourcefire && make && sudo make install
1.5 Configure Snort Environment
bash
Copy
Edit
sudo ldconfig
sudo ln -s /usr/local/bin/snort /usr/sbin/snort
Step 2: Configure Snort
2.1 Create Snort Directory Structure
bash
Copy
Edit
sudo mkdir -p /etc/snort/rules
sudo mkdir /var/log/snort
sudo mkdir /usr/local/lib/snort_dynamicrules
2.2 Create Empty Rule Files
bash
Copy
Edit
sudo touch /etc/snort/rules/local.rules
sudo touch /etc/snort/rules/white_list.rules
sudo touch /etc/snort/rules/black_list.rules
2.3 Configure snort.conf
Copy default snort.conf and modify:

bash
Copy
Edit
cp ~/snort-2.9.20/etc/snort.conf /etc/snort/
sudo nano /etc/snort/snort.conf
Update:

conf
Copy
Edit
var RULE_PATH /etc/snort/rules
include $RULE_PATH/local.rules
Step 3: Create and Test Custom Rules
Example Rule (ICMP Ping Detection)
In /etc/snort/rules/local.rules:

python
Copy
Edit
alert icmp any any -> any any (msg:"ICMP Packet Detected"; sid:1000001; rev:1;)
Test Rule with Ping
Run Snort:

bash
Copy
Edit
sudo snort -A console -q -c /etc/snort/snort.conf -i eth0
Then, ping from another device:

bash
Copy
Edit
ping <your_snort_machine_ip>
You should see an alert in the console.

Step 4: Monitor Network Traffic
Continuous Monitoring Mode
bash
Copy
Edit
sudo snort -A fast -c /etc/snort/snort.conf -i eth0 -l /var/log/snort/
Alerts will be saved to:

bash
Copy
Edit
/var/log/snort/alert.fast
Step 5: Implement Response Mechanisms
While Snort is primarily an IDS (not IPS), you can:

Trigger automated scripts upon alerts using tools like Swatch or OSSEC

Set up email alerts using log monitoring tools

Example: Use inotify or logwatch to scan alert logs and trigger a response.
