# Splunk-Dashboard-for-SSH-Authentication-Monitoring
This project focuses on analyzing SSH authentication logs using Splunk to detect suspicious activities such as failed logins, brute-force attempts, and invalid user access. The dashboard provides a quick overview of authentication events and helps visualize login trends and potential attacks in real time.

**Task 0: Setting up Time Range**

**Goal:** Create a reusable time filter for all panels.

**Steps:**

1. Click Add Input → Time → Pencil Icon
2. Set:
--Label: Time Range
--Token: time_range
3. Again, click Add Input → Submit
4. Use this time_range for all future panels for consistent filtering.

**Task 1: Authentication Overview Panels**

**Goal:** Summarize SSH activity at a glance.
<img width="1865" height="923" alt="1" src="https://github.com/user-attachments/assets/9ee93a7c-8251-45d8-9d6d-9dab862654e7" />

**Panel 1: Total SSH Events**
index="ssh_logs_new" source="ssh_logs_new.json" sourcetype="_json" | stats count AS "Total SSH Events"
<img width="1112" height="696" alt="2" src="https://github.com/user-attachments/assets/efd15e11-74bf-4930-a427-b0e231b9b0fa" />


**Panel 2: Successful Logins**
index="ssh_logs_new" source="ssh_logs_new.json" sourcetype="_json" event_type="Successful SSH Logins" | stats counts AS "Successfull Logins"
<img width="1016" height="587" alt="3" src="https://github.com/user-attachments/assets/99ed8bec-d726-41db-96e4-5f2d99d29312" />


**Panel 3: Failed Logins**
index="ssh_logs_new" source="ssh_logs_.json" earliest=0 latest=now
| spath
| search event_type="Failed SSH Login"
| stats count AS "Failed SSH Logins"
<img width="1018" height="660" alt="4" src="https://github.com/user-attachments/assets/b87ceff5-8526-4e5e-b634-6ecfd847e066" />


**Panel 4: Invalid User Attempts**
index="ssh_logs_new" source="ssh_logs_new.json" sourcetype="_json" event_type="ConnectionWithout Authentication" | stats count AS "Connection Without Authentication"
<img width="1031" height="586" alt="5" src="https://github.com/user-attachments/assets/f8e50a55-21e2-4080-8ef6-2c4818915716" />


**Task 2: Login Activity Trends**

**Goal:** Visualize login behavior and detect abnormal spikes.

**Panel 1: Failed Logins by Username (Bar Chart)**
index="ssh_logs_new" sourcetype="_json"
| spath
| search event_type="Failed SSH Login"
| top username
<img width="1015" height="610" alt="6" src="https://github.com/user-attachments/assets/055bfaba-3bfe-4ffb-aae2-a5306b382337" />


**Panel 2: Possible Brute Force by IP Address (Table)**
index="ssh_logs_new" sourcetype="json"
| spath
| search event_type="Multiple Failed Authentication Attempts"
| top id.orig_h
<img width="1008" height="600" alt="7" src="https://github.com/user-attachments/assets/1d2706f9-9be4-475a-9ef2-cf426c2ac1c3" />


**Task 3: Brute Force Attack — Geo Visualization**

**Goal:** Identify the geographic origin of brute-force attacks.

index="ssh_logs_new" sourcetype="_json" event_type="Multiple Failed Authentication Attempts"
| iplocation id.orig_h
| stats count by Country
| geom geo_countries featureIdField=Country

<img width="1012" height="637" alt="8" src="https://github.com/user-attachments/assets/1b3036a0-11f9-4c9f-b8f2-7dfae02b5453" />









<img width="1911" height="701" alt="9" src="https://github.com/user-attachments/assets/8acef078-9f66-40ed-9fc3-31edb319107f" />




