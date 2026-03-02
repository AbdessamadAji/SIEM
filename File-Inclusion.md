

# File Inclusion

First I wanted to ensure we got the file inclusion attempt on Kibana.

KQL: `url : "/DVWA/vulnerabilities/fi/*"`

<img width="1867" height="947" alt="image" src="https://github.com/user-attachments/assets/fb50c092-05b9-4e11-b632-7c20b49ac9ac" />

Now let's make the rule. For the rule's query I used the famous PayloadsAllTheThings repository to make a good LFI detection.

KQL:
```
url : "/DVWA/vulnerabilities/fi/*"
AND (
  
  /* Directory traversal patterns */
  url : "*../*" OR
  url : "*..\\*" OR
  url : "*%2e%2e*" OR
  url : "*%252e%252e*" OR
  url : "*%c0%ae*" OR
  url : "*..//*" OR
  url : "*....//*" OR
  
  /* Sensitive file access */
  url : "*/etc/passwd*" OR
  url : "*/etc/shadow*" OR
  
  /* Null byte injection */
  url : "*%00*" OR
  
  /* PHP wrapper abuse */
  url : "*php://*" OR
  url : "*data://*" OR
  url : "*file://*"
)
```

<img width="1600" height="763" alt="image" src="https://github.com/user-attachments/assets/fc65f64e-280e-4f8d-8f85-232043dd8115" />

Now let's trigger that alert.

And yes we got it:

<img width="1863" height="929" alt="image" src="https://github.com/user-attachments/assets/bf861ce6-9644-43bc-aa8e-d0c8dd1289c5" />

Now let's make visualizations for it:

<img width="1863" height="929" alt="image" src="https://github.com/user-attachments/assets/4769941a-c2d9-4f44-b3ff-3681b450cf05" />

