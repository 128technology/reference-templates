## Part 3

Here, we see how to delete a config element depending on which config element type it falls under.

1. If the config element has a unique key identifier like "name" such as the router, use that as the value\
`{ "name": "{{ instance.routerName }}" }` as shown in the [delete_router](delete_router) template.

2. If the config element has a unique identifier described by their own sub elements like in the case of next-hops,
The value used would be\
`{ "nodeName": "{{ instance.nodeName }}", "interface": "wan1" }` as shown in the [delete_next_hop](delete_next_hop) template.
