## Verify DVWA Logs

On Kibana:
```
url : "/DVWA/vulnerabilities/brute/*" AND url : "*username=*"
```

<img width="1874" height="905" alt="Screenshot from 2026-02-28 00-10-00" src="https://github.com/user-attachments/assets/456bfea5-336a-4ee7-b8b6-75c24b08dbb8" />

## Make Brute Force Alert

<img width="960" height="792" alt="Screenshot from 2026-02-28 00-12-16" src="https://github.com/user-attachments/assets/c093aea1-9b87-4fc0-ac0f-cef3a735c912" />

## Generate Traffic

Do a random brute force with Burp Suite to trigger an alert.

<img width="1920" height="791" alt="Screenshot from 2026-02-28 00-30-26" src="https://github.com/user-attachments/assets/6a7da89f-69f3-400e-af4b-5e47a9c3b09b" />

## Alert Triggered

<img width="1869" height="961" alt="Screenshot from 2026-02-28 00-28-23" src="https://github.com/user-attachments/assets/8f650237-1b15-4836-b2da-e5def286e1ac" />


- **Threshold result count**: 51, that means 51 tries in 5 minutes
- **Grouped by**: 192.168.56.1, that means the attacker's IP
- **Severity**: High

## Two Panels to Make Detection Easier

Brute Force Attempts (1-min interval) and Top Attacking IP – Brute Force.

<img width="1871" height="841" alt="Screenshot from 2026-02-28 01-08-00" src="https://github.com/user-attachments/assets/3e65f391-9a30-41ba-85eb-d18337fbd26e" />
