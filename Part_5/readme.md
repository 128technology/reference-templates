## Part 5


In this part, we'll see how to leverage a built-in filter called `network_hosts` to create a list of all IPs within a given network.

This helps overcome the need to have multiple IPs defined in the variables tab / file, ease of user management.

First, we source the network IP from the variables tab and store the network address within the in-template variable `network` like so

`{% capture network %}{{ instance.rxLanNetwork }}/26{% endcapture %}`

Next, we load the list of IPs in that subnet into the in-template variable `ip_addresses`
`{%- assign ip_addresses = network | network_hosts %}`

Now all the IPs available within that network are accessible as **offsets** from the base IP.

Meaning a `network = 192.168.0.0/30` would make

```
ip_addresses[1] = 192.168.0.0
ip_addresses[2] = 192.168.0.1
ip_addresses[3] = 192.168.0.2
ip_addresses[4] = 192.168.0.3
```
