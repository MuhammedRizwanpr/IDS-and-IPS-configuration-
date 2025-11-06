# Understanding IPS and Suricata

## What is an IPS (Intrusion Prevention System)?

An IPS is a security system that not only detects suspicious activity (like an IDS) but can also actively block or prevent dangerous traffic. Think of it as a "security guard" that both spots and stops trouble on your network.

## Why Use an IPS?

- Blocks harmful or unauthorized traffic in real-time
- Protects your system from attacks before damage is done
- Provides automatic, proactive security for your network

---

# Setting Up Suricata as an IPS

Suricata can work as an IPS when placed in "inline" mode. Here’s how to set it up simply:

## 1. Install Suricata

On Ubuntu/Debian:
```sh
sudo apt update
sudo apt install suricata
```

## 2. Add a Blocking Rule

1. Open your rules file:
    ```sh
    sudo nano /etc/suricata/rules/local.rules
    ```
2. Add a “drop” rule to block ICMP (ping) traffic:
    ```
    drop icmp any any -> any any (msg:"ICMP Ping Blocked"; sid:2000001; rev:1;)
    ```

## 3. Set Suricata to Inline Mode

Start Suricata in NFQUEUE (inline) mode:
```sh
sudo suricata -c /etc/suricata/suricata.yaml -q 0
```

## 4. Configure NFQUEUE with IPTables

Add a firewall rule to send traffic to Suricata:
```sh
sudo iptables -I INPUT -j NFQUEUE --queue-num 0
```
(You can remove it later with `sudo iptables -D INPUT -j NFQUEUE --queue-num 0`)

---

# Using Suricata as an IPS

## Step 1: Start Suricata in Inline Mode

Use the command above to launch Suricata for active prevention.

## Step 2: Test the Blocking Rule

From another system (for example, a Kali Linux VM), try to ping your protected machine:
```sh
ping <your system’s IP address>
```

## Step 3: Observe the Blocked Traffic

No ping response should be received. You can check Suricata’s logs for the drop alerts:
```sh
sudo tail -f /var/log/suricata/fast.log
```

---

## Try it Yourself!

**You can simply download my code and run it!**

---

### 1. IPS Setup

![IPS Setup](images/1.png)

---

### 2. Check Your IP

![Your IP Address](images/2.png)

---

### 3. Suricata IPS Running

![Suricata IPS Running](images/3.png)

---

### 4. Ping Attempt from Kali Linux VM

![Sending Ping](images/4.png)

---

### 5. Block Alert Shown

![Block Alert Details](images/5.png)

---

**That’s it! Now you have an IPS running using Suricata, actively protecting your system.**