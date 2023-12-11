# Installation



## Error in installation

### Domain Verification Failed - Error 1107!

This happens when CapRover cannot verify that yourcustomdomain.com points to the IP address of CapRover. This can be caused by several factors:

* DNS changes take up to 24 hrs to propagate, specially if your server had cached them before. So wait for 24hrs and retry again. If it doesn't work, proceed to the next step:
* To confirm, go to [https://mxtoolbox.com/DNSLookup.aspx](https://mxtoolbox.com/DNSLookup.aspx) and enter `yourcustomdomain.com`. Make sure it points to the server IP. If you're using a proxy service like CloudFlare, this may cause a problem. Disable their proxy in your DNS on CloudFlare and have A record directly point to the IP address of your CapRover server.
* If you tested all above, and when you visit `something.domain.com` you see the CapRover page, then you can say your domain is working fine, but CapRover is unable to verify it because the loopback test doesn't work. In this case, you can choose to skip domain verification dome by CapRover:

```
echo  "{\"skipVerifyingDomains\":\"true\"}" >  /captain/data/config-override.json
docker service update captain-captain --force
```

* If none of the above works, please open an issue on Github.
