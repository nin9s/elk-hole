# elk-hole

## elasticsearch, logstash and kibana configuration for pi-hole visualization

### show, search, filter and customize pi-hole statistics ... the elk way
### a huge "thank you" to [skaldenhoven](https://github.com/skaldenhoven) who contributed quiet some nice details to the configuration and parsing logic as well as troubleshooting and testing!



please note, this is still work in progress, so please let me know if I've left anything unclear/incorrect which definitely could be the case!

### requirements:
## working installation of:
1. logstash
2. elasticsearch
3. kibana
4. filebeat on pi-hole

-> installation of the elk stack - refer to https://www.elastic.co/ for details.


this repo provides the relevant files and configuration for sending the pi-hole logs via filebeat directly to logstash/elasticsearch. We will then visualize the logs in kibana with a custom dashboard.

The result will look like this:

![alt text](https://github.com/nin9s/elk-hole/blob/master/dash.PNG)

  
# HOW TO USE 
 
### LOGSTASH HOST 
1. copy "/conf.d/20-dns-syslog.conf" to your logstash folder (usually /etc/logstash)
2. customize "ELASTICSEARCHHOST:PORT" in the output section at the bottom of the file
3. copy "dns" to "/etc/logstash/patterns/"
4. restart logstash

### PI-HOLE
5. copy "/etc/filebeat/filebeat.yml" to your filebeat installation at the pi-hole instance
6. customize "LOGSTASHHOST:5141" to match your logstash hostname/ip
7. restart filebeat

### KIBANA HOST (CAN BE THE SAME AS LOGSTASH AND ELK)
8. import "elk-hole.json" into kibana: management - saved objects - import



You should then be able to see your new dashboard and visualizations.

