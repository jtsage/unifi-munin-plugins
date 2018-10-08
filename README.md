UniFi Munin Plugin
===================
A munin plugin for UniFi gear - All written in 100% perl 

 * standard pre-requisites, no 3rd party API client.
 * does not yet support WebRTC requests - needs direct access to the controller.
 * most plugins support munin-multigraph
 * all plugins support [munin-dirtyconfig](http://guide.munin-monitoring.org/en/latest/plugin/protocol-dirtyconfig.html) (see performance below.)

## Available Graphs:


### unifi\_clients\_by\_device _(multigraph)_
 
 Display a count of clients connected per unifi device (physical device)
 
<p align="center"><img src="https://raw.githubusercontent.com/jtsage/unifi-munin-plugins/master/example-graphs/unifi_clients_by_device.png" /></p>


### unifi\_clients\_by\_type _(multigraph)_
 
 Display a count of clients grouped by how they are connected (wired/wireless, user/guest, SSID)

<p align="center"><img src="https://raw.githubusercontent.com/jtsage/unifi-munin-plugins/master/example-graphs/unifi_clients_by_network.png" /></p>

 

### unifi\_device\_cpu _(provided by unifi_device_stats multigraph)_
 
 Display CPU usage on each unifi device
 
<p align="center"><img src="https://raw.githubusercontent.com/jtsage/unifi-munin-plugins/master/example-graphs/unifi_device_cpu.png" /></p>



### unifi\_device\_load _(provided by unifi_device_stats multigraph)_

 Display the load average on each unifi device
 
<p align="center"><img src="https://raw.githubusercontent.com/jtsage/unifi-munin-plugins/master/example-graphs/unifi_device_load.png" /></p>



### unifi\_device\_mem _(provided by unifi_device_stats multigraph)_
 
 Display the memory usage on each unifi device

<p align="center"><img src="https://raw.githubusercontent.com/jtsage/unifi-munin-plugins/master/example-graphs/unifi_device_mem.png" /></p>



### unifi\_device\_uptime _(provided by unifi_device_stats multigraph)_
 
 Display the uptime of each unifi device, in days

<p align="center"><img src="https://raw.githubusercontent.com/jtsage/unifi-munin-plugins/master/example-graphs/unifi_device_uptime.png" /></p>



### unifi\_xfer\_by\_device _(multigraph)_
 
 Display transfer statistics on a per unifi device basis

<p align="center"><img src="https://raw.githubusercontent.com/jtsage/unifi-munin-plugins/master/example-graphs/unifi_xfer_by_device.png" /></p>



### unifi\_xfer\_by\_network _(multigraph)_
 
 Display transfer statistics per named network (VLAN, LAN, etc) (Requires a USG) - if you have more 
 than one firewall device on a "site", this plugin will almost certainly shit the bed.  I don't know 
 if this is a use case that can/should exist, but I didn't plan for it.

<p align="center"><img src="https://raw.githubusercontent.com/jtsage/unifi-munin-plugins/master/example-graphs/unifi_xfer_by_network.png" /></p>



### unifi\_xfer\_by\_port _(multigraph)_
 
 Display transfer statistics per physical network port (Switches only)

<p align="center"><img src="https://raw.githubusercontent.com/jtsage/unifi-munin-plugins/master/example-graphs/unifi_xfer_by_port.png" /></p>



### unifi\_xfer\_by\_radio _(multigraph)_
 
 Display transfer statistics per physical radio (APs only)

<p align="center"><img src="https://raw.githubusercontent.com/jtsage/unifi-munin-plugins/master/example-graphs/unifi_xfer_by_radio.png" /></p>



### unifi\_xfer\_by\_uplink
 
 Displays transfer statistics for your network uplink (This is not robust - I have a single UFW, and 
 it does not have a wan failover port - at a minimum, I'd need dumps from the /stat/device API point 
 for a bunch of configurations to make this not choke if your configuration doesn't match mine.  If 
 you do feel the need to send, there probably is sanitzation to be done in there.) (Also, should the 
 plugin run while speedtest is running, it'll report zero - by default, munin runs every 5 min, 
 on :00, :05, etc - plan accordingly)

<p align="center"><img src="https://raw.githubusercontent.com/jtsage/unifi-munin-plugins/master/example-graphs/unifi_xfer_by_uplink.png" /></p>




## Configuration:

In your munin-node plugin configruation file, you'll need to add:

```ini
[unifi_api]
  env.user Controller_Username
  # default is ubnt

  env.pass Controller_Password
  # default is ubnt

  env.api_url https://unifi.fqdn.com:8443
  # default is https://localhost:8443

  env.ssl_verify_host no 
  # Check That SSL host is valid, default is yes

  env.ssl_verify_peer no 
  # Check That SSL peer is valid, default is yes

  env.name Site Name
  # A pretty name for the unifi site - used in graph titles.

  env.site site_string 
  # default is "default" - found when you connect to the web interface - it's the term
  # in the URL - /manage/site/site_string/dashboard
```
It is probably a wise idea to add a read-only admin to your site for this purpose.  The munin-node 
configuration file is not a terribly secure place to store login details.

Additionally, you can configure:

```ini
[unifi_api]
  env.enable_device_cpu yes
  # Show device CPU utilization

  env.enable_device_mem yes
  # Show device memory usage

  env.enable_device_load yes
  # Show device load average (switches and APs only)

  env.enable_device_uptime yes
  # Show device uptime

  env.enable_clients_device yes
  # Show number of clients connected to each device
  env.enable_detail_clients_device yes
  # Show detailed graphs for each device

  env.enable_clients_type yes
  # Show number of clients connected to each network type
  env.enable_detail_clients_type yes
  # Show detailed graphs for each client type
  env.show_authorized_clients_type yes
  # Show unauthorized / authorized client list - if you are not using the guest portal, this is useless

  env.enable_xfer_port yes
  # Show transfer statistics on switch ports
  env.enable_detail_xfer_port yes
  # Show detailed graphs per switch port
  env.hide_empty_xfer_port yes
  # Hide ports that have no link

  env.enable_xfer_device yes
  # Show transfer statistics per device
  env.enable_detail_xfer_device yes
  # Show detailed graphs for each device

  env.enable_xfer_network yes
  # Show transfer statistics per named network
  env.enable_detail_xfer_network yes
  # Show detailed graphs for each named network

  env.enable_xfer_radio yes
  # Show transfer statistics per radio
  env.enable_detail_xfer_radio yes
  # Show detailed graphs for each radio
```

## Prerequisites:

All scripts require 

 * [Perl : WWW::Curl::Easy](https://metacpan.org/pod/WWW::Curl)
   * WWW::Curl is a Perl extension interface for libcurl.
 * [Perl : JSON](https://metacpan.org/pod/JSON)
   * JSON (JavaScript Object Notation) encoder/decoder

## Performance:

Largely unknown.  My use case is querying a controller that is not on the munin-node host, and the 
link is not stellar - that said, ~~running every plugin in this collection, my munin-update time went 
up by about 30 seconds. Every plugin is going to hit the API twice - just the nature of the beast - 
from there, it's on the local machine.  I suppose local cacheing could be a thing, but IIRC, munin 
is asyncronous, so it's only going to save 1 * number\_of\_plugins API calls.  I suppose I could 
dump all of this in a single multigraph, but then there is no easy pick-and-choose method.~~

This is what adding all of these did to my munin processing time:

<p align="center"><img src="https://raw.githubusercontent.com/jtsage/unifi-munin-plugins/master/example-graphs/perf_concern.png" /></p>

After a week of this, I refactored every plugin to use dirtyconfig - A way of returning the config 
and the data at the same time.  That saved a good bit of processing time:

<p align="center"><img src="https://raw.githubusercontent.com/jtsage/unifi-munin-plugins/master/example-graphs/perf-update.png" /></p>

Stay tuned for the change to a monolithic single file.  The savings are extream.

## Pull Requests, etc.

Eventually this will all get dropped into the munin contrib repository.  But I figured the community 
would want to test and make changes first.

If you have anything else you want added, drop a new one here, or open an issue with the suggestion 
(pointing out the API node you want added would be hugely helpful - it's not well docuemted)

## Special Thanks

Thanks to the following:

 * [hhansen06](https://github.com/hhansen06) - [Forum Post](https://community.ubnt.com/t5/UniFi-Wireless/Unifi-Munin-Plugin-V3-only/m-p/454653) - Did an initial PHP plugin that sparked the idea.
 
 * [Art-of-WiFi](https://github.com/Art-of-WiFi/) - PHP API implementation that was borrowed from heavily to figure out appropriate endpoints.
 
