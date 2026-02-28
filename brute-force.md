#verify dvwa logs 
on kibana : url : "/DVWA/vulnerabilities/brute/*" AND url : "*username=*"
*pic here

#make brite force alert 
*pic here

#generate traffic 
do a random brute force with burp suite to trigger an alert
*pic here

#alert triggered
* pic here

Threshold result count: 51 , that means 51 try in 5 minutes
Grouped by: 192.168.56.1 , that means the attacker's ip
Severity: High


#two panels to make detection easier
Brute Force Attempts (1-min interval) and Top Attacking IP – Brute Force.
*pic here

