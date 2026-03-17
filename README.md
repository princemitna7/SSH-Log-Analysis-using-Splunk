# 🔐 SSH Log Analysis using Splunk

## 📌 Project Overview

This project focuses on analyzing SSH authentication logs using Splunk to identify security events and potential threats. It simulates real-world SOC (Security Operations Center) use cases such as detecting brute-force attacks, monitoring login activity, and identifying suspicious connections.

---

## 🎯 Objective

The goal of this project is to analyze SSH logs to detect:

* ✅ Successful logins (who connected and from where)
* ❌ Failed login attempts (possible brute-force or password spraying)
* 🔁 Multiple failed authentication attempts (brute-force indicators)
* ⚠️ Connections without authentication (possible scanning or incomplete sessions)

---

## 🛠️ Lab Setup & Prerequisites

Before starting:

* Install and configure Splunk (Enterprise or Free version)
* Download the provided `ssh_log.json` file

---

## ⚙️ Data Ingestion Steps

1. Log in to your Splunk instance
2. Navigate to: **Apps → Search & Reporting**
3. Click: **Add Data → Upload**
4. Upload the `ssh_log.json` file
5. Set:

   * **Sourcetype**: `_json`
   * **Index**: `ssh_logs`
6. Review settings and click **Start Searching**

---

## 📊 Task 1: Ingest and Parse Logs

Ensure the following fields are extracted:

* `event_type`
* `auth_success`
* `auth_attempts`
* `id.orig_h` (Source IP)
* `id.resp_h` (Destination host)

### ✅ Validation Query

```spl
index=ssh_logs | stats count by event_type
```

---

## 🚫 Task 2: Analyze Failed Login Attempts

### 🔍 Query

```spl
index=ssh_logs event_type="Failed SSH Login"
| stats count by id.orig_h
```

### 📈 Visualization

* Create a **bar chart**
* Show **Top 10 source IPs** generating failed login attempts

---

## 🔐 Task 3: Detect Brute-Force Attempts

### 🔍 Query

```spl
index=ssh_logs event_type="Multiple Failed Authentication Attempts"
| stats count by id.orig_h, id.resp_h
```

### 🚨 Detection Logic

* Identify IPs with **more than 5 failed attempts**

### ⏰ Alert Configuration

* Trigger alert when:

  * An IP performs **>5 login attempts within 10 minutes**

---

## ✅ Task 4: Track Successful Logins

### 🔍 Query

```spl
index=ssh_logs event_type="Successful SSH Login"
| stats count by id.orig_h, id.resp_h
```

### 🧠 Analysis

* Compare successful logins with prior failed attempts
* Detect potential compromised accounts

### 📊 Dashboard

* Create panel showing:

  * Top source IPs for successful logins

---

## ⚠️ Task 5: Identify Suspicious Unauthenticated Connections

### 🔍 Query

```spl
index=ssh_logs event_type="Connection Without Authentication"
| stats count by id.orig_h
```

### 📈 Time-Based Monitoring

```spl
index=ssh_logs event_type="Connection Without Authentication"
| timechart count by id.orig_h
```

### 🧠 Insight

* Repeated unauthenticated attempts may indicate:

  * Port scanning
  * SSH probing activity

---

## 📊 Dashboard Overview

Build a Splunk dashboard including:

* Failed login attempts (Top IPs)
* Successful logins
* Brute-force detection
* Unauthenticated connections over time

---

## 🚨 Alerts Configured

* Brute-force detection alert:

  * Condition: More than 5 login attempts within 10 minutes
  * Action: Notify via email or trigger webhook

---

## 🎓 Key Learnings

* Log ingestion and field extraction in Splunk
* Writing SPL (Search Processing Language) queries
* Detecting security threats from logs
* Creating dashboards and visualizations
* Configuring alerts for real-time monitoring

---

## 🏁 Conclusion

This project provides hands-on experience in:

* Monitoring SSH activity
* Detecting brute-force and suspicious login attempts
* Building SOC-level dashboards and alerts

---

## 📂 Project Structure

```
ssh-log-analysis-splunk/
│
├── ssh_log.json
├── README.md
└── dashboards/
```

---

## 🚀 Future Enhancements

* Integrate with threat intelligence feeds
* Add geo-location enrichment for IP addresses
* Automate response using SOAR tools
* Extend to Kubernetes or cloud-based SSH logs

---