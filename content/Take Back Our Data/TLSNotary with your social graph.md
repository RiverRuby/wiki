---
date: 2024-08-26
draft: false
---
TLSNotary is very powerful, but limited in its expressivity. In particular, TLSNotary allows a notary to sign off on API requests while maintaining privacy over the included data. However, this signature can only be trusted if you trust the notary involved didn't collude in the creation of these API requests.

However, I believe TLSNotary data becomes much more [[VIP]] when it's being done over a social graph. In particular, if someone who is one degree away from you signs off on different TLS sessions (maybe even all of your TLS sessions?) then everyone who's two degrees away from you through that friend can trust + use these signatures as needed. Or even folks further away, based on how important the data is.

I believe this is a very compelling (and conceptually beautiful) way we can make TLSNotary into a general source of data, not methods using proxies or EigenLayer AVS. Those have other attacks in the general case, or are just too complicated and custom built to credibly gain wide adoption.

### Examples
- What if your friends signed off on your internet reputation?
	- Let's say your friends A and B signed off on your Twitter follower count being >800
	- Friends of friends through A/B can have faith this fact is true
	- Can require multiple attestations from multiple friends
- What if your friends signed off on all of your internet traffic?
	- Let's say your friends A and B signed off on all of your internet traffic (one form factor: [[VPN that signs web traffic]])