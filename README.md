UniFi Munin Plugins
===================
A couple of PHP based munin plugins for UniFi gear.

## Available Plugins:

 * unifi_clients

 Display a count of clients connected per unifi device (physical device)

 * unifi_cpu

 Display the CPU usage of your unifi devices

 * unifi_loadavg

 Display the load average of your unifi devices

 * unifi_memory

 Display the memory usage of your unifi devices

 * unifi_userdetail

 Display a count of clients connected per network (Wireless networks and Wired, Totals for both)

## Configuration:

In your munin-node plugin configruation file, you'll need to add:

    [unifi_*]
    env.unifi_user Controller_Username
    env.unifi_pass Controller_Password
    env.unifi_host https://unifi.fqdn.com:8443
    env.unifi_site Site_Name (often "default" for home use)
    env.unifi_ver 5.8.28 (Controller software version)
    env.unifi_ssl false (true == Fail on bad SSL certificate)

## Prerequisites:

You need a copy of ArtOfWifi's UniFi API Client - available here: https://github.com/Art-of-WiFi/UniFi-API-client

These plugins assume it is installed in a unifi folder off of your PHP search path. (i.e. /usr/share/php/unifi )

Also be aware that your PHP needs to be configured to populate the $_ENV variable, or you need to hardcode your login details in the script.

## Pull Requests, etc.

Eventually this will all get dropped into the munin contrib repository.  But I figured the community would want to test and make changes first.

If you have anything else you want added, drop a new one here, or open an issue with the suggestion (pointing out the API node you want added would be hugely helpful - it's not well docuemted)

Also, if anyone wants to port all of this over to something not-PHP, I'm all for it.  Not a huge fan, but the API client was already written.


