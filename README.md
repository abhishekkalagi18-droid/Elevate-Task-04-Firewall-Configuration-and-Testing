# Elevate Task 04 — Firewall Configuration and Testing

## 📌 Overview

This task demonstrates the configuration and basic testing of a **host-based firewall using UFW (Uncomplicated Firewall)** on Kali Linux.

The objective was to understand how firewall rules control inbound network traffic, allow required services such as SSH, temporarily block Telnet traffic, verify firewall configuration, and restore the system to its original state.

---

## 🎯 Objectives

- Configure UFW on Linux.
- Enable and verify the firewall.
- Allow inbound SSH traffic on TCP port `22`.
- Temporarily block Telnet traffic on TCP port `23`.
- Verify firewall rules.
- Check whether a service is listening on port `23`.
- Remove the temporary Telnet rule.
- Understand inbound and outbound traffic filtering.

---

## 🛠️ Tools and Environment

| Component | Details |
|---|---|
| Operating System | Kali Linux |
| Firewall | UFW |
| Protocol | TCP |
| SSH Port | 22 |
| Telnet Port | 23 |
| Testing Tool | `ss` |
| Privileges | sudo/root |

---

# 🔍 What is a Firewall?

A **firewall** is a security mechanism that monitors and filters network traffic according to predefined rules.

A host-based firewall protects an individual system by controlling which network connections are allowed or denied.

### Basic Concept

```text
Incoming Traffic
       │
       ▼
   ┌─────────┐
   │ Firewall│
   └─────────┘
       │
   ┌───┴────┐
   │        │
 Allow     Deny
   │        │
   ▼        X
 Service   Blocked
```

For example, a firewall can allow SSH traffic on port `22` while denying Telnet traffic on port `23`.

---

# 🧪 Practical Implementation

## 1. Allow SSH Traffic

SSH normally uses TCP port `22`.

Before enabling UFW, SSH traffic was explicitly allowed:

```bash
sudo ufw allow 22/tcp
```

### Expected Result

```text
Rules updated
Rules updated (v6)
```

This creates rules allowing SSH connections over IPv4 and IPv6.

---

## 2. Enable UFW

The firewall was enabled using:

```bash
sudo ufw enable
```

### Expected Result

```text
Firewall is active and enabled on system startup
```

This confirms that UFW is active and configured to start automatically with the system.

---

## 3. Verify Firewall Rules

The active firewall rules were checked using:

```bash
sudo ufw status numbered
```

### Expected Result

```text
[ 1] 22/tcp       ALLOW IN    Anywhere
[ 2] 22/tcp (v6)  ALLOW IN    Anywhere (v6)
```

This confirms that inbound SSH traffic is allowed.

---

## 4. Block Telnet Traffic

Telnet commonly uses TCP port `23`.

A temporary deny rule was created:

```bash
sudo ufw deny 23/tcp
```

### Expected Result

```text
Rule added
Rule added (v6)
```

The resulting firewall configuration was:

```text
[ 1] 22/tcp       ALLOW IN    Anywhere
[ 2] 23/tcp       DENY IN     Anywhere
[ 3] 22/tcp (v6)  ALLOW IN    Anywhere (v6)
[ 4] 23/tcp (v6)  DENY IN     Anywhere (v6)
```

This demonstrates explicit port-based traffic filtering.

---

## 5. Check Whether Port 23 Is Listening

Before attempting a Telnet connection, the system was checked to determine whether a service was listening on TCP port `23`.

```bash
sudo ss -lntp | grep ':23'
```

### Result

No output was returned.

### Interpretation

No service was listening on TCP port `23` on the Kali Linux system.

Therefore, an actual Telnet connection test could not be performed.

However, the firewall rule itself was successfully created and verified.

> **Important:** A firewall rule blocking a port and a service actually listening on that port are two different things. A firewall can deny traffic to a port even when no application is currently using it.

---

## 6. Review Complete Firewall Configuration

The complete UFW configuration was checked using:

```bash
sudo ufw status verbose
```

### Expected Result

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
```

Configured rules included:

```text
22/tcp  ALLOW IN  Anywhere
23/tcp  DENY IN   Anywhere
```

### Security Observation

The default policy:

```text
deny (incoming), allow (outgoing)
```

means unsolicited inbound connections are denied unless an explicit rule permits them.

Outbound connections are allowed by default.

This follows a common security principle:

> **Allow only what is required and deny unnecessary inbound access.**

---

## 7. Remove the Temporary Telnet Rule

After completing the test, the temporary Telnet rule was removed:

```bash
sudo ufw delete deny 23/tcp
```

### Expected Result

```text
Rule deleted
Rule deleted (v6)
```

The firewall was then verified again:

```bash
sudo ufw status numbered
```

### Final Result

```text
[ 1] 22/tcp       ALLOW IN    Anywhere
[ 2] 22/tcp (v6)  ALLOW IN    Anywhere (v6)
```

The temporary Telnet rule was successfully removed.

---

# 📸 Evidence

The practical evidence can be stored in the following structure:

```text
screenshots/
├── 01-ufw-enabled.png
├── 02-telnet-blocked.png
├── 03-firewall-verbose.png
└── 04-final-firewall-status.png
```

### Evidence Demonstrates

- UFW successfully enabled.
- SSH port `22` allowed.
- Telnet port `23` denied.
- Firewall default policies.
- Port `23` verification.
- Temporary rule removal.
- Final firewall configuration.

---

# 📚 Key Concepts

## Inbound Traffic

Traffic entering a system.

```text
Remote Computer → Kali Linux
```

Firewall inbound rules determine whether this traffic should be allowed or denied.

---

## Outbound Traffic

Traffic leaving the system.

```text
Kali Linux → Internet
```

Outbound rules determine whether the system can initiate connections to external systems.

---

## Stateful Firewall

A stateful firewall tracks active connections and uses connection state when making filtering decisions.

For example, it can recognize traffic belonging to an already-established connection.

---

## Stateless Firewall

A stateless firewall evaluates packets individually according to predefined rules such as:

- Source IP
- Destination IP
- Source port
- Destination port
- Protocol

---

## UFW

**UFW (Uncomplicated Firewall)** is a simplified command-line interface for managing firewall rules on Linux systems.

Common commands include:

```bash
sudo ufw enable
sudo ufw disable
sudo ufw status
sudo ufw status numbered
sudo ufw allow <port>/tcp
sudo ufw deny <port>/tcp
sudo ufw delete deny <port>/tcp
```

---

## Port 23 — Telnet

TCP port `23` is traditionally associated with Telnet.

Telnet is considered insecure for remote administration because it does not provide the encryption expected from modern secure remote-access protocols.

**SSH**, normally using TCP port `22`, is preferred for secure remote administration.

---

# 🔐 Security Best Practices

When configuring a host-based firewall:

1. **Allow only required services.**
2. **Avoid unnecessary open ports.**
3. **Use SSH instead of Telnet.**
4. **Restrict administrative services to trusted networks where possible.**
5. **Review firewall rules regularly.**
6. **Remove temporary testing rules after use.**
7. **Consider both IPv4 and IPv6 rules.**
8. **Enable appropriate firewall logging.**
9. **Follow the principle of least privilege.**
10. **Document important firewall changes.**

---

# 🧠 What I Learned

Through this practical task, I learned:

- How host-based firewalls protect Linux systems.
- How to configure UFW.
- How to allow and deny traffic based on TCP ports.
- How SSH and Telnet differ from a security perspective.
- How to verify firewall rules.
- How to identify listening services using `ss`.
- How default inbound and outbound policies work.
- Why temporary firewall rules should be removed after testing.
- How firewall configuration contributes to reducing the attack surface.

---

# ✅ Outcome

The firewall configuration and testing exercise was successfully completed.

### Completed Tasks

- ✅ UFW available on Kali Linux
- ✅ Firewall enabled
- ✅ SSH port `22` allowed
- ✅ Telnet port `23` temporarily blocked
- ✅ Firewall rules verified
- ✅ Port `23` checked using `ss`
- ✅ Temporary Telnet rule removed
- ✅ Final firewall configuration verified
- ✅ Inbound and outbound filtering concepts understood

This exercise provided practical experience with **Linux firewall administration, port-based filtering, UFW configuration, inbound traffic control, service verification, and basic network security concepts**.

---

# ⚠️ Disclaimer

All firewall configuration and testing documented in this repository was performed on an **authorized personal/lab Linux environment**.

No unauthorized systems, networks, or services were targeted.

This repository is intended for **educational cybersecurity and defensive security learning purposes only**.

---

## 📖 Recommended Next Steps

After completing this task, the next useful topics to study are:

- Linux firewall architecture
- UFW logging and troubleshooting
- `iptables` and `nftables`
- Stateful packet filtering
- Network segmentation
- Firewall rule optimization
- Network security monitoring
- IDS vs IPS
- Security hardening
- Port scanning and firewall behavior

---

## 🔗 Useful Learning Resources

- **UFW Documentation:** https://help.ubuntu.com/community/UFW
- **Ubuntu Server Documentation:** https://documentation.ubuntu.com/server/
- **Linux Manual Pages:** https://man7.org/linux/man-pages/
- **Nmap Documentation:** https://nmap.org/docs.html
- **OWASP:** https://owasp.org/

---

## ⭐ Project Status

**Status:** Completed ✅

**Task:** Elevate Task 04 — Firewall Configuration and Testing

**Environment:** Kali Linux

**Focus:** Host-Based Firewall / Network Security / UFW
