# 🧪 Assignment: Build Log Monitoring Pipeline with ELK

## 🎯 Objective

Design and implement a log monitoring solution using:

```
Nginx → Filebeat → Logstash → Elasticsearch → Kibana
```

The system should:

- Collect only error logs
- Process and store logs
- Visualize HTTP errors (300–500) in the last 5 minutes

## 🧩 Part 1: Environment Setup

### Q1. Provision an Ubuntu machine and install ELK stack
- Which components are required for this project?
- What commands will you use to install them?

### Q2. Install and configure Nginx
- How will you verify that Nginx is running successfully?
- Which log files does Nginx generate by default?

### Q3. Generate sample traffic on Nginx
- How will you generate valid and invalid HTTP requests?
- What type of logs do you expect in the error log file?

## 📥 Part 2: Filebeat Configuration

### Q4. Configure Filebeat to collect logs
- Which configuration file will you modify?
- How will you ensure Filebeat reads only Nginx error logs?

### Q5. Configure Filebeat output
- How will you send logs to Logstash instead of Elasticsearch?
- Which port will be used for communication?

### Q6. Validate Filebeat setup
- What command will you use to restart Filebeat?
- How will you confirm that logs are being shipped?

## 🔄 Part 3: Logstash Processing

### Q7. Create a Logstash pipeline configuration
- Where will you define pipeline configuration files?
- What are the three main sections of a Logstash pipeline?

### Q8. Configure Logstash input
- How will Logstash receive data from Filebeat?

### Q9. Parse log data using filters
- How will you extract HTTP status codes from raw logs?
- Which Logstash plugin will you use for parsing?

### Q10. Transform extracted data
- How will you ensure the HTTP status field is treated as a number?

### Q11. Configure Logstash output
- How will you send processed logs to Elasticsearch?
- What naming convention will you use for indices?

### Q12. Validate Logstash pipeline
- How will you check if Logstash is processing logs correctly?
- Which logs or commands will you use?

## 🗄️ Part 4: Elasticsearch Verification

### Q13. Verify index creation in Elasticsearch
- Which API or command will you use to list indices?
- What index pattern do you expect to see?

### Q14. Inspect stored data
- How will you verify that logs are correctly stored in Elasticsearch?

## 📊 Part 5: Kibana Visualization

### Q15. Create an index pattern in Kibana
- What index pattern will you use?
- Which field will be used as the time filter?

### Q16. Write KQL query to filter logs
- How will you filter logs where HTTP status is between 300 and 500?
- How will you limit results to the last 5 minutes?

### Q17. Create a visualization
- How will you display the count of errors?
- Which visualization type will you use?

### Q18. Create time-based visualization
- How will you show error trends over time?
- What interval will you choose?

### Q19. Create additional insights
- How will you display distribution of HTTP status codes?
- How will you identify top error messages?

### Q20. Build a dashboard
- How will you combine multiple visualizations into a dashboard?
- What name will you give your dashboard?

## 🚨 Part 6: Alerting (Optional)

### Q21. Create an alert rule in Kibana
- What condition will trigger the alert?
- What time window will you configure?

### Q22. Define alert action
- What type of notification or action will you configure?