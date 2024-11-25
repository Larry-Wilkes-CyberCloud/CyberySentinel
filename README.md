# CyberSentinel: Threat Hunting and Analysis

## Overview
CyberSentinel is a Splunk app designed for proactive threat hunting and analysis. It leverages custom dashboards, alerts, and reports to detect anomalies and threats in network activity.

This app provides:
- A centralized **dashboard** for monitoring suspicious activities.
- **Real-time alerts** for potential threats.
- Predefined **knowledge objects** for quicker investigations.

---

## App Home Page
The app includes an intuitive home page that provides an overview of its features and purpose.

![CyberSentinel home page](https://github.com/user-attachments/assets/363bd843-01da-4d0a-8aa5-a08fe3b3e1ae)


### Welcome XML Configuration
The app features a custom XML welcome page to guide users through its purpose and capabilities.

![welcome xml file for app home page](https://github.com/user-attachments/assets/2c07a7fd-db7a-4ce1-9332-b62f741a2bb0)


---

## Alerts
CyberSentinel provides real-time alerts to detect critical network activity and threats. The following alerts are included:

1. **Excessive DNS Queries to Rare Domains**  
   - **SPL**:
     ```spl
     index=threat_hunting_logs sourcetype=dns | stats count by query | where count = 1
     ```
   - **Trigger**: When the number of rare domain queries exceeds 10.

2. **High Volume of DNS Queries from a Single Source**  
   - **SPL**:
     ```spl
     index=threat_hunting_logs sourcetype=dns | stats count by id_orig_h | where count > 100
     ```
   - **Trigger**: When a single source IP makes more than 100 DNS queries in 10 minutes.

3. **Rare Domain Access**  
   - **SPL**:
     ```spl
     index=threat_hunting_logs sourcetype=http | stats count by extracted_host | where count < 3
     ```
   - **Trigger**: When a domain is accessed fewer than 3 times within an hour.

4. **SSL Self-Signed Certificate Usage**  
   - **SPL**:
     ```spl
     index=threat_hunting_logs sourcetype=ssl validation_status="self signed certificate" | stats count by server_name
     ```
   - **Trigger**: When more than 5 self-signed certificates are detected in 1 hour.

![saved alerts](https://github.com/user-attachments/assets/72508632-63d4-45d7-a0c3-8e7bde6f3b2e)


---

## Reports
The app includes predefined reports that provide detailed insights into network activities. These reports can be used to analyze trends and identify potential threats:

1. **Rare DNS Queries**  
   - **SPL**:
     ```spl
     index=threat_hunting_logs sourcetype=dns | stats count by query | where count = 1
     ```

2. **Rare HTTP URLs**  
   - **SPL**:
     ```spl
     index=threat_hunting_logs sourcetype=http | stats count by extracted_host | where count < 3
     ```

3. **SSL Self-Signed Certificates**  
   - **SPL**:
     ```spl
     index=threat_hunting_logs sourcetype=ssl validation_status="self signed certificate" | stats count by server_name
     ```

4. **Suspicious POST Requests**  
   - **SPL**:
     ```spl
     index=threat_hunting_logs sourcetype=http method="POST" | stats count by extracted_host
     ```

5. **Suspicious User Agents**  
   - **SPL**:
     ```spl
     index=threat_hunting_logs sourcetype=http | stats distinct_count(extracted_host) as Domains by user_agent | sort -Domains | head 5
     ```

6. **Top Downloaded Files**  
   - **SPL**:
     ```spl
     index=threat_hunting_logs sourcetype=files | top filename limit=10
     ```

7. **Top Queried Domains**  
   - **SPL**:
     ```spl
     index=threat_hunting_logs sourcetype=dns | top query limit=10
     ```

![saved reports](https://github.com/user-attachments/assets/7d937a4c-d99e-4db0-91ca-7c59de61055d)


---

## Dashboard
The CyberSentinel dashboard is designed to give you real-time visibility into network anomalies and threat indicators. Key panels include:
- **Top Queried Domains**: Bar chart displaying frequently queried domains.
- **SSL Self-Signed Certificates**: Pie chart highlighting certificate anomalies.
- **Suspicious User Agents**: Pie chart showing rare and suspicious user agents.
- **Rare DNS Queries**: Tabular format showing DNS queries with low occurrence.

![dasboard image](https://github.com/user-attachments/assets/07d2fdb7-214f-4641-826c-7874ee2b249b)


---


