# solar-monitoring-TIG-Shelly
telegraf to ping Shelly devices, store in influxdb and visualize with grafana, all inside a raspberry pi 3 B+

Access Grafana via HTTPS from outside home network

generated SSL certificates with let's encrypt, following their instructions with snap

copied the * .pem files into /etc/ssl/grafana and edit the grafana.ini config file in the [https] line (search with Ctrl + W) to point to location of certificate file and private key

```
[server]
# Protocol (http, https, h2, socket)
protocol = https
```
uncomment the line by removing ``` ; ```

add the location of the certificates you copied from letsencrypt/live/yourwebsitename/
```
# enable gzip
;enable_gzip = false

# https certs & key file
cert_file = /etc/ssl/grafana/fullchain.pem
cert_key = /etc/ssl/grafana/privkey.pem

# Unix socket path
;socket =
```

and just your traffic from outside gets routed through port 443 directly change the ```http_port = 3000``` to:
```
# The ip address to bind to, empty will bind to all interfaces
;http_addr =

# The http port  to use
https_port = 443

# The public facing domain name used to access grafana from a browser
```

the file need their permissions altered for grafana to read them, by changing permission for all files in directory (more instructions at https://medium.com/grafana-tutorials/adding-ssl-to-grafana-eb4ab634e43f):
```
sudo chown grafana:grafana /etc/ssl/grafana
```
then allow the grafana group to read the cert files:
```
sudo chmod 640 /etc/ssl/grafana/cert.pem
sudo chmod 640 /etc/ssl/grafana/privkey.pem
```

then restart the grafana service and check that is running alright:
```
sudo systemctl restart grafana.service
sudo systemctl status grafana.service

● grafana-server.service - Grafana instance
   Loaded: loaded (/lib/systemd/system/grafana-server.service; enabled; vendor p
   Active: active (running) since Wed 2021-02-24 22:59:49 GMT; 6min ago
     Docs: http://docs.grafana.org
 Main PID: 4320 (grafana-server)
    Tasks: 10 (limit: 2062)
   CGroup: /system.slice/grafana-server.service
           └─4320 /usr/sbin/grafana-server --config=/etc/grafana/grafana.ini --p

Feb 24 22:59:49 raspberrypi grafana-server[4320]: t=2021-02-24T22:59:49+0000 lvl
```

to check the grafana log file in case something goes wrong:
```
cat /var/log/grafana/grafana.log | grep "narrow search here"
```
in my case as I was having trouble with the pem file permissions:
```
cat /var/log/grafana/grafana.log | grep ".pem"
```

## Small title ##

normal text

**bold text** 
```
http://SHELLY_IP/emeter/0
```

```
code
```

Generating SSL certificate to encrypt communication between grafana in host and client from outside the home network by using Let's Encrypt and Certbot
Followed direction in letsencrypt website and used snap to install it in the raspberry pi.
````
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/fishermanshouse.duckdns.org/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/fishermanshouse.duckdns.org/privkey.pem
   Your certificate will expire on 2021-05-23. To obtain a new or
   tweaked version of this certificate in the future, simply run
   certbot again. To non-interactively renew *all* of your
   certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
 ````
```
