UniFi Munin Plugins
===================
A couple of munin plugins for UniFi gear - All written in 100% perl (standard pre-requisites, no 3rd party API client)

## Available Plugins:

 * archive/

 Should you reall wish to, roll back in the history here for the original PHP versions of these.

 * unifi\_clients\_by\_device _(multigraph)_

 Display a count of clients connected per unifi device (physical device)

 * unifi\_clients\_by\_type _(multigraph)_

 Display a count of clients grouped by how they are connected (wired/wireless, user/guest, SSID)

 * unifi\_device\_cpu

 Display CPU usage on each unifi device

 * unifi\_device\_load

 Display the load average on each unifi device

 * unifi\_device\_mem

 Display the memory usage on each unifi device

 * unifi\_device\_uptime

 Display the uptime of each unifi device, in days

 * unifi\_xfer\_by\_device _(multigraph)_

 Display transfer statistics on a per unifi device basis

 * unifi\_xfer\_by\_network _(multigraph)_

 Display transfer statistics per named network (VLAN, LAN, etc) (Requires a USG) - if you have more than one firewall device on a "site", this 
 plugin will almost certainly shit the bed.  I don't know if this is a use case that can/should exist, but
 I didn't plan for it.

 * unifi\_xfer\_by\_port _(multigraph)_

 Display transfer statistics per physical network port (Switches only)

 * unifi\_xfer\_by\_radio _(multigraph)_

 Display transfer statistics per physical radio (APs only)

 * unifi\_xfer\_by\_uplink

 Displays transfer statistics for your network uplink (This is not robust - I have a single UFW, and it does not have a wan failover port - at a minimum, I'd need dumps from the /stat/device API point for a bunch of configurations to make this not choke if your configuration doesn't match mine.  If you do feel the need to send, there probably is sanitzation to be done in there.)


## Configuration:

In your munin-node plugin configruation file, you'll need to add:

    [unifi_*]
      env.user Controller_Username
      # default is ubnt
      env.pass Controller_Password
      # default is ubnt
      env.host https://unifi.fqdn.com:8443
      # default is localhost:8443
      env.sslh no 
      # Check That SSL host is valid, default is yes
      env.sslp no 
      # Check That SSL peer is valid, default is yes
      env.name Site Name
      # A pretty name for the unifi site - used in graph titles.
      env.site site_string 
      # default is "default" - found when you connect to the web interface - it's the term
      # in the URL - /manage/site/site_string/dashboard

## Prerequisites:

All scripts require 

 * Perl : WWW::Curl::Easy
 * Perl : JSON

## Performance:

Largely unknown.  My use case is querying a controller that is not on the munin-node host, and the link is not stellar - that said, running every
plugin in this collection, my munin-update time went up by about 30 seconds. Every plugin is going to hit the API twice - just the nature of the 
beast - from there, it's on the local machine.  I suppose local cacheing could be a thing, but IIRC, munin is asyncronous, so it's only going
to save 1 * number\_of\_plugins API calls.  I suppose I could dump all of this in a single multigraph, but then there is no easy pick-and-choose method.

## Pull Requests, etc.

Eventually this will all get dropped into the munin contrib repository.  But I figured the community would want to test and make changes first.

If you have anything else you want added, drop a new one here, or open an issue with the suggestion (pointing out the API node you want added 
would be hugely helpful - it's not well docuemted)


