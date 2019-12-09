# elk-hole

## Pi-hole data visualization using Elasticsearch, Logstash and Kibana

elk-hole provides the relevant files and configuration to easily visualize pi-holes/dnsmasq statistics via the popular elasticstack.

### Show, search, filter and customize pi-hole statistics ... the elk way


### Requirements:
## Working Installation of:
1. [logstash](https://www.elastic.co/products/logstash) (currently tested up to version "7.3")
2. [elasticsearch](https://www.elastic.co/products/elasticsearch)(currently tested up to version with "7.3")
3. [kibana](https://www.elastic.co/products/kibana)(currently tested up to version "7.3")
4. [filebeat](https://www.elastic.co/products/beats/filebeat) on pi-hole (tested with "1.3.1", "7.1.1" & "7.3")


For official installation guides of the elk stack - refer to [Elastic](https://www.elastic.co/ for details) 

For a quick setup, check out [easyELK](https://github.com/josh-thurston/easyELK)


Elk-hole provides the relevant files and configuration for sending the pi-hole logs via filebeat directly to logstash/elasticsearch. We will then visualize the logs in kibana with a custom dashboard.

The result will look like this:

![alt text](https://github.com/nin9s/elk-hole/blob/master/dash.PNG)
#### Alternative:
![alt text](https://github.com/nin9s/elk-hole/blob/master/dash_enhanced.PNG)
  
# HOW TO USE 
 
### LOGSTASH HOST 

1. Download the files from Elk-hole repo 
2. From the downloaded files, copy ```20-dns-syslog.conf``` to ```/etc/logstash/conf.d/``` and ```/patterns``` to  ```/etc/logstash/``` to your logstash system.  

Your files should be like this:

```/etc/logstash/conf.d/20-dns-syslog.conf```

```/etc/logstash/patterns/dns```

If you have other files in this folder make sure to properly edit the input/output/filter sections to avoid matching our filebeat dns logs in these files which may be processed earlier. For testing purposes you can name your conf files like so:

```
/conf.d/20-dns-syslog.conf
/conf.d/30-other1.conf
/conf.d/40-other2.conf
```

This makes sure that ```/conf.d/20-dns-syslog.conf``` is processed at the beginning.

3. Using vim or nano, open/edit ```20-dns-syslog.conf```.  Scroll down to the Output section and change  ```ELASTICSEARCHHOST:PORT``` to match your environment.  If elasticsearch is running on the same system as logstash, then ```127.0.0.1:9200``` should work.  
4. Restart logstash -  ```systemctl restart logstash.service```

### PI-HOLE Host

5. From the downloaded files, copy ```filebeat.yml``` to your ```/etc/filebeat/``` and copy ```99-pihole-log-facility.conf``` to ```/etc/dnsmasq.d/```
6. Using vim or nano, open/edit the ```hosts:``` line and enter the IP address of the logstash system ```LOGSTASH IP:5141```
7. Restart filebeat ```systemctl restart filebeat.service``` 
8. *Important:* Restart pi-hole and ensure filebeat is sending logs to logstash before proceeding further. ```pihole restartdns```
9. You can verify this filebeat is running properly with the following two steps
10. ```service filebeat status``` The output should show a couple key message.  Active: active (running) & Connection established 
11. ```sudo filebeat test output``` should show:

```
Logstash: <Logstash IP>:5141...
  Connection..
    Parse hosts... OK
    Dns lookup...  OK
    Addresses: <Logstash IP>
    Dial up... OK
  TLS... WARN secure connection disabled
  Talk to server... OK
```

The following steps on the Kibana Host will not work correctly if sending data to logstash is not successfull!

### KIBANA HOST (CAN BE THE SAME AS LOGSTASH AND ELASTICSEARCH)

12. Browse to the Kibana management interface using a web browser ```http://Kibana IP:5601```
13. Go to Management --> Kibana --> Index Patterns and click Create the index pattern
14. Type ```logstash-syslog-dns*``` - It should find one index
15. Click next step and select ```@timezone``` 
16. Create index pattern
17. Once the index is created, verify that 79 fields are listed
18. Click the curved arrows on the top right to refresh the index fields.  This is important because this will not automatically happen.
19. Browse to Management --> Kibana --> Saved Objects
20. Select Import (You will repeat this step)
21. From the downloaded files, locate the ```json``` (or ```ndjson``` if you are using a recent version of elk) folder and import the following files depending on your software version (1.3.1 or 7.x)

```elk-hole - vis.json```

```elk-hole - vis_enhanced.json```

```elk-hole - vis_enhanced_fix.json```

```elk-hole - dash.json```

```elk-hole - dash_enhanced.json``` 

*Note:* When you import these files, you could possibly see a message "Index Pattern Conflicts".  This is ok.  Below that message you may see one or two rows of data.  On each row click on the drop down menu and select "logstash-syslog-dns*"

22. Browse to Dev Tools (wrench on left navigation)
23. When Dev Tools comes up, there will be two columns when you are in the section with "Console" underlined.
24. Delete any existing data in the left column
25. From the downloaded files, locate ```logstash-syslog-dns-index.template_ELK7.x.json```
26. Open that file in a text editor on your system
27. Copy the entire contents of the file
28. Paste the content of: ```logstash-syslog-dns-index.template_ELK7.x.json``` into kibanas dev tools console
29. Click the green triangle in the upper right of the pasted content (first line). Output should be:

```
{
  "acknowledged" : true 
}
```

30. As a precaution restart the whole elk stack

```
systemctl restart logstash.service 
systemctl restart elasticsearch.service
systemctl restart kibana.service
```

You should then be able to see your new dashboard and visualizations.
