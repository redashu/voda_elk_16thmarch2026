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

### apache.conf 

```
cd /etc/logstash/conf.d/
  295  ls
  296  vim apache.conf

===>

input {

 beats {
   port => 5044
 }
}

filter {
  # Keep empty for now (we’ll add parsing later)
}

output {
  elasticsearch {
    hosts => ["https://localhost:9200"]
    user => "elastic"
    password => "Redhat@12345"
    ssl_certificate_authorities => "/etc/elasticsearch/certs/http_ca.crt"
    index => "apache-logstash-%{+YYYY.MM.dd}"
  }

  stdout {
    codec => rubydebug
  }
}
```