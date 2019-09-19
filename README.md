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
#### alternative:
![alt text](https://github.com/nin9s/elk-hole/blob/master/dash_enhanced.PNG)
  
# HOW TO USE 
 
### LOGSTASH HOST 
1. copy ```/conf.d/20-dns-syslog.conf``` to your logstash folder (usually ```/etc/logstash/```)
If you have other files in this folder make sure to properly edit the input/output/filter sections to avoid matching our filebeat dns logs in these files which may be processed earlier. For testing purposes you can name your conf files like so:

```
/conf.d/20-dns-syslog.conf
/conf.d/30-other1.conf
/conf.d/40-other2.conf
```

This makes sure that ```/conf.d/20-dns-syslog.conf``` is beeing processed at the beginning.

2. customize ```ELASTICSEARCHHOST:PORT``` in the output section at the bottom of the file
3. copy ```dns``` to:
```/etc/logstash/patterns/``` create the folder if it does not exist

4. restart logstash

### PI-HOLE
5. copy ```/etc/filebeat/filebeat.yml``` to your filebeat installation at the pi-hole instance
6. customize ```LOGSTASHHOST:5141``` to match your logstash hostname/ip
7. restart filebeat
8. copy ```99-pihole-log-facility.conf to /etc/dnsmasq.d/```
9. this is very important: restart pi-hole and ensure filebeat is sending logs to logstash before proceeding
10. You can verify this by:
11. at your filebeat instance: 
```filebeat test output```
it should say ```ok``` on every step.
12. again: the following steps will not work correctly if sending data to logstash here is not successfull!

### KIBANA HOST (CAN BE THE SAME AS LOGSTASH AND ELASTICSEARCH)

13. create the index pattern:
```Management -> Index patterns -> Create index pattern```
14. type ```logstash-syslog-dns``` - it shound find one index
15. click next step and select ```@timezone``` 
16. Create index pattern
17. Once the index is created, verify that 79 fields are listed
18. click the curved arrows on the top left
19. import suitable ```json/elk-hole *.json``` for your version into kibana: ```management - saved objects - import```
20. optionally select the correct index pattern: ```logstash-syslog-dns*```
21. delete any existing template matching our index name: 
```DELETE /_template/logstash-syslog-dns*```
22. import the template: paste the content of: ```logstash-syslog-dns-index.template_ELK7.x.json``` into kibanas dev tools console
23. click the green triangle in the upper right of the pasted content (first line). Output should be:
```
{
  "acknowledged" : true 
}
```
24. as a precaution restart the whole elk stack
```
systemctl restart logstash 
systemctl restart elasticsearch
systemctl restart kibana
```

You should then be able to see your new dashboard and visualizations.
