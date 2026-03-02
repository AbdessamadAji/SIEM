# SQL Injection

First let's do a SQL injection attempt, something like `1' OR '1'='1`

And let's see if Kibana recorded that:

KQL: `url : "/DVWA/vulnerabilities/sqli/*"`

<img width="1868" height="800" alt="image" src="https://github.com/user-attachments/assets/9728b4a0-e616-4147-be12-d7bb1b3f6e05" />


Now let's create a rule to catch all SQL injection attempts.

To verify we're covering majority of techniques, I will use PayloadsAllTheThings for making a good query.

And since KQL is case-insensitive so we don't need case variation.

The query:
```
url : "/DVWA/vulnerabilities/sqli/*"
AND (
  
  /* Quote breaking */
  url : "*'*" OR
  url : "*%27*" OR
  url : "*\"*" OR
  url : "*%22*" OR
  
  /* Boolean logic manipulation */
  url : "* or *" OR
  url : "* and *" OR
  
  /* UNION-based extraction */
  url : "*union*" OR
  url : "*select*" OR
  
  /* Comment truncation */
  url : "*--*" OR
  url : "*%2d%2d*" OR
  url : "*#*" OR
  
  /* Time-based injection */
  url : "*sleep(*" OR
  url : "*benchmark(*" OR
  
  /* Stacked queries / execution */
  url : "*;*" OR
  url : "*exec*" OR
  url : "*||*" OR
  url : "*&&*" OR
  
  /* Metadata enumeration */
  url : "*@@*" OR
  
  /* Advanced filtering tricks */
  url : "*having*"
)
```

But I think this rule is too complex for Elasticsearch to process.

<img width="1642" height="285" alt="image" src="https://github.com/user-attachments/assets/7f8b3d56-b1f8-49e9-8077-90071961c280" />

In fact I should dedicate 3 rules, one for normal SQL injection, one for time based SQL injection, and last one for stacked/execution SQL injection.

But I prefer using just one, and simplifying that rule a little bit.

The query:
```
url : "/DVWA/vulnerabilities/sqli/*"
AND (
  url : "*'*" OR
  url : "*%27*" OR
  url : "* or *" OR
  url : "* and *" OR
  (url : "*union*" AND url : "*select*") OR
  url : "*--*" OR
  url : "*#*" OR
  url : "*sleep(*" OR
  url : "*benchmark(*" OR
  url : "*;*" OR
  url : "*@@*"
)
```

Now it's working:

<img width="1642" height="782" alt="image" src="https://github.com/user-attachments/assets/d9f67d09-6beb-4224-9e58-eaa3e63cba40" />


Now let's make visualizations for making the detection simple, faster, and efficient.

<img width="1867" height="942" alt="image" src="https://github.com/user-attachments/assets/4ee30fb0-5608-4cb4-81ee-a2d7ff3601ae" />
