# Pi-Hole Setup
Set static IP through DHCP reservation
- Pretty straightforward. May vary from router to router, but try looking in the *LAN* section in the GUI
- Your server needs a static IP address for Pi-Hole to function most effectively

Install Pi-Hole: `curl -sSL https://install.pi-hole.net | bash`
- Informational menu navigation in setup. First relevant option is Upstream DNS Provider. I chose OpenDNS for now
	* There is a ton of info in the Pi-Hole documentation about which one you can choose. I'd recommend reviewing that if you're interested in making a decision based on that : https://docs.pi-hole.net/guides/dns/upstream-dns-providers/
- Do you want to install Steven's block list? : *Yes* 
	- This block list is a good starting point
	- If you just want to set this up and not tinker with it much beyond the initial setup, this will probably be all you need
	- It's a list that's maintained regularly. Here's their [GitHub](https://github.com/StevenBlack/hosts)
- Do you want to enable query logging? : *Yes/No* 
- Select privacy mod for FTL: *Anonymous Mode*
	* Used in conjunction with query logging : This is how much will be logged if logging is enabled.
	- FTL privacy determines how much data is logged when logging is enabled
After this is completed, Pi-Hole is installed and will provide you with a generated password.
- You can and should change the password with `pihole setpassword`

Sometimes it can take a minute, but Pi-Hole will automatically update its `pihole.toml` configuration file if port 80 is already in use. Just make sure port 8080 is not in use, and it will automatically rollover to 8080. Otherwise, just configure it yourself in the `.toml` file

> Of note, it's very easy to add pre-made block-lists into your Pi-Hole. You just need a URL to paste in the address link
- So far I am only using Steven's domain list
*May be configured more later*


## Now to set up the Pi as the default DNS server
In router, navigate to DHCP server
- Update the DNS server to match the IP address of your Pi-Hole server

**At this point, everything should be completely functional.**

## Unbound Installation
**Got this part from Crosstalk Solutions on YouTube: https://www.crosstalksolutions.com/the-worlds-greatest-pi-hole-and-unbound-tutorial-2023/**
- And he found it here in the official [Pi-Hole docs](https://docs.pi-hole.net/guides/dns/unbound/)
	- Follow the steps in the official Pi-Hole documentation specified above, and it should work.
Assists Pi-Hole in looking up root DNS servers on the Internet. You can use this instead of OpenDNS or any other DNS service chosen previously

# Issues
Per the documentation, Pi-Hole will install on 80 by default, but if that's in use it will switch to 8080. If 8080 isn't available, it will not be available and you'll have to manually configure it.
- **Read the documentation - it will save you a lot of time**
- The documentation is ~~false~~ **true** ! *(it just takes a minute to re-do the ports automatically - but you should still edit the pihole.toml file yourself)* 

Don't add additional endpoints to your nginx redirect for Pi-Hole. If you simply redirect to `127.0.0.1:8080` it will work properly
- For instance, when I tried redirecting to `127.0.0.1:8080/admin/login/` it would break the UI in Chromium browsers and in Firefox the UI just wouldn't work.

Encountered an issue with `unbound-resolvconf.service`. Error message was `Failed to set DNS configuration:` and some info regarding a loopback device. 
- I found a GitHub Issue [here](https://github.com/NLnetLabs/unbound/issues/1161) that matched this exactly, and it's also in the Unbound documentation [here](https://unbound.docs.nlnetlabs.nl/en/latest/use-cases/local-stub.html). (In the docs, just search "resolvconf" and the relevant section will come up)
- I stopped, disabled, and masked the service 
	- After doing this, systemd doesn't automatically clear its cache, so it'll still look like you have an error. Just refresh systemd's records of services in the "failed" state and it should display properly
* It didn't seem to affect how Pi-Hole was performing, but the error message was annoying me when I saw it.
* Using the `dig` command helps to confirm Unbound is still working

