## Part 2

In here, we'll introduce the usage of conditional and looping statements like:
1. `if-else` to dynamically assign **pci addresses** (as opposed to a static config for pci in this [template](../Part_1/template)).\
The **pci-addresses** are assigned depending on the **Hardware Type**

2. `for loops` to shrink the template by looping through repetitive config elements.

### 1. If-Else

The hardware type for a given branch is stored in the variable tab / file and then compared to the entire list of possible hardware types in the if-else logic at the beginning of the template.\
When a match is found, the respective pci-address variables are set for the given branch.

If the **hwType** on the variable is set as **Silicom**, the following pci addresses are assigned to the respective interface.

| interface | variabes  | pci address  |
| --------- | --------- | ------------ |
| mpls1     | mpls1_pci | 0000:00:04.0 |
| lan1      | lan1_pci  | 0000:00:03.0 |
| wan1      | wan1_pci  | 0000:00:05.0 |


For every other hardware, the following pci addresses are assinged to the respective interface

| interface | variabes  | pci address  |
| --------- | --------- | ------------ |
| mpls1     | mpls1_pci | 0000:00:05.0 |
| lan1      | lan1_pci  | 0000:00:04.0 |
| wan1      | wan1_pci  | 0000:00:06.0 |


And assigned to the interface like so

```
"pciAddress": {{ mpls1_pci }}
"pciAddress": {{ lan1_pci }}
"pciAddress": {{ wan1_pci }}
```

> NOTE: In this sample, we assume there are only 2 hw types Silicom and Lanner and hence an else)
If there are more hw types, you could chain them through an `elsif` loop

### 2. Shrinking repetitive config using for loops

Looking at the service routes in [template](../Part_1/template), the service routes

**svr-testserver2**, \
**svr-dnat-testservice3**, \
**svr-lb-testservice**

have similar config and can be merged to self generate as seen in [template](template) and shown below.

First, we compile the list of service routes we want self generated.

``{% assign service_routes = "testservice2, dnat-testservice3, lb-testservice" | split: ", " %}``

> NOTE: The (**split: ", "**) is using the ", " as delimiter and compiling a list of just the service names

If there's a need to add another service route with a similar format, all that's needed then is to add an entry to the above list.

Next, we loop through them by using a `for loop`

```
{% for service_name in service_routes %}
          {
            "name": "svr-{{ service_name }}",
            "serviceName": "{{ service_name }}",
            "peer": "{{ instance.routerName }}"
          }{% if forloop.last == false %},{% endif %}
{% endfor %}

```

Where the `for service_name in service_routes` loads each of the service routes one after the other.

The `{% if forloop.last == false %},{% endif %}` adds a comma for all segments but the last one.

This form of reusability helps managing the service routes a lot easier by listing out all of them in a single place at the top of the file.
