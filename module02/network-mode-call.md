# Network Mode Call

**Course:** CIT 386 · Module 2 · Assignment 2.2
**Author:** Nabin Chaulagain
**Date:** September 24, 2026

**The situation:** a guest on my laptop must be reachable from another machine on the same physical network in the room, and no configuration may be done on the host.

## My paragraph

<!--
Write 4-5 sentences in your own words. Cover all three things below, then delete this comment.

1. The mode you would set  -> Bridged Adapter.
   Why it works: the guest gets its own address on the room's real network, so other
   machines can reach it directly, and nothing has to be set up on the host.

2. The mode you rejected   -> NAT (the default).
   Why it fails: the guest sits behind the host, so nobody outside can start a
   connection to it. It would need port forwarding configured on the host, and the
   requirement does not allow host configuration.

3. What bridged costs you  -> the guest is exposed on the real network instead of
   hidden behind the host, it depends on that network handing out an address, and some
   networks (campus or guest Wi-Fi) block it.

I would set the network adapter to Bridged.

This works because the VM gets its own IP address on the room’s network, so other machines on that network can connect to the VM directly. Nothing needs to be changed on the laptop itself.

I would reject NAT because it hides the VM behind the laptop’s network connection. Other machines cannot normally initiate connections to the VM through NAT unless port forwarding is configured on the host, which the requirement does not allow.

The downside of Bridged networking is that the VM is exposed directly to the real network. It needs to obtain its own network address, and some networks—especially managed or restricted ones—may block or limit bridged connections.
