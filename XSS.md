# XSS (Cross-Site Scripting)

First let's do some basic XSS attacks, with payloads like: `<script>alert(1)</script>`, `<img src=x onerror=alert(1)>`, etc.

<img width="1918" height="985" alt="image" src="https://github.com/user-attachments/assets/39f5866f-06fc-409b-be61-d28af1f29e60" />

Now let's see if it's captured by our ELK stack:

<img width="1873" height="984" alt="image" src="https://github.com/user-attachments/assets/a65e4c4b-8e8e-4a28-9a03-a881cfaee5ff" />

Now let's create a rule to catch that.

The query:
```
url : "/DVWA/vulnerabilities/xss_d*"
AND (
  url : "*<script*" OR
  url : "*%3cscript*" OR
  url : "*javascript:*" OR
  url : "*onerror=*" OR
  url : "*onload=*" OR
  url : "*<img*" OR
  url : "*<svg*" OR
  url : "*%3c*" 
)
```


<img width="1644" height="653" alt="image" src="https://github.com/user-attachments/assets/b40539e4-c26d-4024-9982-d6bde1b94aee" />


Let's test that alert and see if it's working:


<img width="1874" height="904" alt="image" src="https://github.com/user-attachments/assets/addaee2a-aad3-4f78-bb89-0c412faebbfc" />


We verified that it's working, now let's make visualizations to make detection smoother and simpler.


<img width="1874" height="974" alt="image" src="https://github.com/user-attachments/assets/e37a1ed3-215a-4a6f-87e9-35c7e5a55dbc" />
