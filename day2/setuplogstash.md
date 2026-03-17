### Understanding more info 

<img src="appi.png">

### Install & verify 

```
 270  sudo apt install logstash 
  271  history 
root@ip-172-31-7-140:~# logstash --version 
logstash: command not found
root@ip-172-31-7-140:~# find / -type f -iname logstash 
/usr/share/logstash/bin/logstash
/etc/default/logstash
root@ip-172-31-7-140:~# /usr/share/logstash/bin/logstash --version 
Using bundled JDK: /usr/share/logstash/jdk
logstash 8.19.12

```