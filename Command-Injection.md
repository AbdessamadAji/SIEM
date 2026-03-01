# Command injection  

Verify That Command Injection Traffic Occurs

Filter: `url : "/DVWA/vulnerabilities/exec/*"`


<img width="1868" height="943" alt="image" src="https://github.com/user-attachments/assets/16d3283e-a024-40b5-a902-34bc8b715428" />


Now I'm facing a problem, the payload isn't included in the URL, it's in the request's body, and I'm not logging the request bodies right now.

To fix this, I enabled `mod_dumpio`, it's a module that logs raw HTTP traffic handled by Apache.

Those logs will be written to `/var/log/apache2/error.log`, so I added that path to Filebeat.

But when starting I found DumpIO is not ideal. Sometimes body missing and it gives noisy logs.

So I will force GET-based injections for our lab.

To fix this issue, I decided to edit the DVWA command injection file to log user input.


<img width="869" height="70" alt="image" src="https://github.com/user-attachments/assets/1712a274-e11e-402a-8148-fbc66e1b0657" />


And I added that path to Filebeat. Now we can detect command injection in Kibana.

And I edited Logstash Pipeline, since in the logs we have something like:
```
2026-03-01T03:30:00 | SRC=192.168.56.1 | IP_PARAM=127.0.0.1 | cat /etc/passwd
```

We need to parse the source IP and the command into separated fields.

I added the rule:


<img width="1862" height="925" alt="image" src="https://github.com/user-attachments/assets/8b16181b-a71a-4f68-aca0-9612591da283" />


I tested it and it's working:


<img width="1862" height="925" alt="image" src="https://github.com/user-attachments/assets/99bb4978-d9f4-4913-ad51-8817f9531b6a" />


Now let's create visualizations.


<img width="1862" height="925" alt="image" src="https://github.com/user-attachments/assets/d4414de7-c564-418e-a047-c769d1e5ba98" />
