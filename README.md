# elk-hole

## elasticsearch, logstash and kibana configuration for pi-hole visualization

elk-hole provides the relevant files and configuration to easily visualize pi-holes/dnsmasq statistics via the popular elasticstack.

### show, search, filter and customize pi-hole statistics ... the elk way


### requirements:
## working installation of:
1. logstash (currently tested up to version "7.1.0")
2. elasticsearch (currently tested up to version with "7.1.0")
3. kibana (currently tested up to version "7.1.0")
4. filebeat on pi-hole (tested with "1.3.1" & "7.1.1")

-> installation of the elk stack - refer to https://www.elastic.co/ for details.


this repo provides the relevant files and configuration for sending the pi-hole logs via filebeat directly to logstash/elasticsearch. We will then visualize the logs in kibana with a custom dashboard.

The result will look like this:

![alt text](https://github.com/nin9s/elk-hole/blob/master/dash.PNG)

  
# HOW TO USE 
 
### LOGSTASH HOST 
1. copy "/conf.d/20-dns-syslog.conf" to your logstash folder (usually /etc/logstash/)
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
12. import suitable "json/elk-hole *.json" for your version into kibana: management - saved objects - import
13. delete any existing template matching our index name: DELETE /_template/logstash-syslog-dns*
14. import the template: paste the content of "logstash-syslog-dns-index.template_ELK7.x.json" into kibanas dev tools console
15. optionally reload kibanas field list


You should then be able to see your new dashboard and visualizations.
