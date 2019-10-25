.. title: Resolvconf on two interfaces with discrete networks
.. date: 2018-03-23 11:03:12 UTC-04:00
.. category: general
.. previewimage: /images/resolvconf.jpg

Resolvconf on two interfaces with discrete networks
Getting resolvconf to resolve dns entries on two interfaces with discrete networks and dns servers.

Why you would want this: - see at the end my notes on why you don't want to do this as well.

You have two discrete networks that can't be joined and can't share DNS information across their network boundaries.
You have been somehow allowed to join two interfaces to a server connecting to each of these networks.
What you might expect
You would be tempted to think you can use your usual utilities for this. In my case I specifically rely on /etc/network/interfaces on my Ubuntu machines. It is old and reliable for me as systemd-network gets it act together.

You might configure your /etc/network/interfaces files to look like this

    auto eth0 eth1

    iface eth0 inet static 
        address x.x.x.32 
        gateway x.x.x.0 
        network x.x.x.x 
        netmask x.x.x.x 
        dns-nameservers x.x.x.1 x.x.x.2 
        dns-search sub.contoso.com

    iface eth1 inet static 
        (same entries as above with different subnet) 
        dns-nameservers y.y.y.1 y.y.y.2 
        dns-search otherdomain.net 
If I have these dns entries in both interfaces on a reboot it will turn my /etc/resolv.conf into

    nameserver y.y.y.1 
    nameserver y.y.y.2 
    nameserver x.x.x.1 
    search sub.contoso.com otherdomain.net
Which results in not being able to resolve anything on sub.contoso.com but being able to resolve otherdomain.net.

What the heck!!?

Why wouldn't the nameserver correlate to the interface stanza I setup?

Why it doesn't work
Default entry limit
I found here that you can only add up to 3 entries by default in resolv.conf.

You can of course as usual in linux tinker deeper into the stack and increase this limit in > /usr/include/resolv.h with the lines.

/*
* Global defines and variables for resolver stub.
*/
# define MAXNS                  3       /* max # name servers we'll track */
But we're just trying to query two networks so there's no reason to open another element to cause bugs/issues. Moving on…

The process that /etc/network/interfaces appears to use is it rotates nameservers as it parses the config. The last dns-nameserver entries you have is what goes into resolv.conf. So when you are putting dns info in your interfaces file it's mostly a convenience to save you from writing to your resolv/conf and messing with resolvconf and its database. The order you place them in the interfaces file, the stanza, or even the corresponding dns-search means nothing.

If you have 8 dns entries in your interfaces file it will be cut to the last 3 entries and your dns-search string concatenated into a very long search line in your resolv.conf.

Timeout not Rotation
Default behavior for multiple nameservers is to only trigger the secondary server if there is a timeout in the request a.k.a the server is down or there are network problems.

This is a failover mechanism which is what you want for most situations. Good idea. Definitely best practice, but doesn't accomplish our goal at the moment. What we need is for our resolver to try all of the configured servers for the entry we are looking for before giving up. So..

Instead of trying more servers on TIMEOUT we try more servers on SERVFAIL.

The fix.
Configure your interfaces file as normal but omit nameservers until they fit in your resolv.conf or adjust your nameserver limit as shown above. Realize though that for every additional nameserver you are adding more servers to rotate through before you can query your end-server.

Modify the resolvconf database Open up /etc/resolvconf/resolv.conf.d/base and create the following

options rotate
wq! and/or your editors save and close equivalent

It works! Why you shouldn't do this
First of all you now have to specify by FQDN for every query so anything coded to just a hostname lookup is going to fail.
Secondly you're rotating through your dns-servers for these queries when really you should have 1 authoritative DNS server
Thirdly this type of customization makes your setup a snowflake and you shouldn't be engineering your network to be a snowflake (that's another blog post)
If your argument is 'but how else will I contact the devices?'

In my opinion this is a cop out. Someone isn't doing networking. Your computer should be able to talk to what it needs from its primary interface. We have routers, switches, and firewalls for connecting users and applications. Subscribing a computer as a client on two different networks is unnecessary complexity that will confuse future admins.

But perfection is the root of all procrastination so when you need to get nitty gritty don't let me stop you from doing what you have to do. You'll just find your answer elsewhere or heaven forbid write your own solution in node.js.

Sources:
Russel Ballestrini | Set-Dns-Resolver-Options Note: His item on the rotate setting here didn't actually work for me but showed me a way of editing resolvconf easily that will stay when generated by resolvconf.

Into the Void | resolv.conf options and discovery of ISP DNS Issue Note: I noticed the proper options setting for resolv.conf in here which worked for my server. Resolv.conf Manpage