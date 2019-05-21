# elk-hole

## elasticsearch, logstash and kibana configuration for pi-hole visualization

elk-hole provides the relevant files and configuration to easily visualize pi-holes/dnsmasq statistics via the popular elasticstack.

### show, search, filter and customize pi-hole statistics ... the elk way


### requirements:
## working installation of:
1. logstash (tested with "6.5.0")
2. elasticsearch (tested with "6.5.0")
3. kibana (tested with "6.5.0")
4. filebeat on pi-hole (tested with "1.3.1")

-> installation of the elk stack - refer to https://wiki.kaldenhoven.org/display/LIN/Elastic+Stack+on+Ubuntu+16.04+with+AdoptOpenJDK or https://www.elastic.co/ for details.


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
9. copy 99-pihole-log-facility.conf to /etc/dnsmasq.d/
11. restart pi-hole

### KIBANA HOST (CAN BE THE SAME AS LOGSTASH AND ELASTICSEARCH)
12. import "elk-hole *.json" into kibana: management - saved objects - import
13. optionally reload kibanas field list


You should then be able to see your new dashboard and visualizations.




#### a huge "thank you" to [skaldenhoven](https://github.com/skaldenhoven) who contributed quiet some nice details to the configuration and parsing logic as well as troubleshooting and testing!
