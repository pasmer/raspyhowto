# SSL Cert

## Certificate with CERBOT and Let’s Encrypt&#x20;

Link: [https://mhagemann.medium.com/how-to-secure-nginx-with-certbot-on-ubuntu-16-10-4a66956cd49c](https://mhagemann.medium.com/how-to-secure-nginx-with-certbot-on-ubuntu-16-10-4a66956cd49c) &#x20;

#### INSTALL - Ensure that your version of snapd is up to date&#x20;

<pre><code>sudo apt install snapd
<strong>sudo snap install core; sudo snap refresh core
</strong></code></pre>

#### Remove certbot-auto and any Certbot OS packages:&#x20;

```
sudo apt-get remove certbot 
```

Install Certbot, Run this command on the command line on the machine to install Certbot.&#x20;

```
sudo snap install --classic certbot 
```

Prepare the Certbot command. Execute the following instruction on the command line on the machine to ensure that the certbot command can be run.&#x20;

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot 
```

&#x20;

### Get new certificate&#x20;

```
sudo certbot certonly --nginx 
```

Delete a certificate

```
sudo certbot delete
```

Certificate in this folder:&#x20;

```
/etc/letsencrypt/live
```

### RENEW CERTIFICATO&#x20;

Note that these are short-term certificates which will expiry in 90 days.&#x20;

Fare login in root con sudo su&#x20;

Run:&#x20;

```
 /usr/bin/certbot renew
```



NB: every once in a while to renew those certificates &#x20;

CRON JOB:&#x20;

[https://serverfault.com/questions/790772/cron-job-for-lets-encrypt-renewal](https://serverfault.com/questions/790772/cron-job-for-lets-encrypt-renewal) &#x20;
