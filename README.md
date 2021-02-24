# solar-monitoring-TIG-Shelly
telegraf to ping Shelly devices, store in influxdb and visualize with grafana, all inside a raspberry pi 3 B+

Access Grafana via HTTPS from outside home network

generated SSL certificates with let's encrypt, following their instructions with snap

copied the * .pem files into /etc/ssl/grafana and edit the grafana.ini config file in the [https] line (search with Ctrl + W) to point to location of certificate file and private key

the file need their permissions altered for grafana to read them:
  sudo chown grafana:grafana /etc/ssl/grafana
  (changes permission for all files in directory)
  
  then allow the grafana group to read the files
  sudo chmod 640 /etc/ssl/grafana
 
## Small title ##

normal text

**bold text** 
```
http://SHELLY_IP/emeter/0
```

```
code
```
