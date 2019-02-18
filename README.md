# elk-hole

## elasticsearch, logstash and kibana configuration for pi-hole visualization

### show, search, filter and customize pi-hole statistics ... the elk way
### a huge "thank you" to [skaldenhoven](https://discourse.pi-hole.net/u/skaldenhoven/summary) who contributed quiet some nice details to the configuration and parsing logic as well as troubleshooting and testing!

requirements:

logstash
elasticsearch
kibana

-> working installation of the elk stack - refer to https://www.elastic.co/ for details.


this repo provides the relevant files and configuration for sending the pi-hole logs via filebeat directly to logstash/elasticsearch. We will then visualize the logs in kibana with a custom dashboard.

The result will look like this:

<SCREENSHOT TODO>
  
# HOW TO USE 
 
