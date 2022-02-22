## Part 4

In here, we'll see when a dictionary can be used and how to emulate a dictionary within liquid templating.

Taking the following config as an example, we see that there's a repetitive pattern it.


```
config

    authority

        router  bostonsite1
            name           bostonsite1

            service-route  service_1_local_route
                name          service_1_local_route
                service-name  service_1
                nat-target    192.168.10.1
            exit

            service-route  service_2_local_route
                name          service_2_local_route
                service-name  service_2
                nat-target    192.168.10.1
            exit

            service-route  service_3_local_route
                name          service_3_local_route
                service-name  service_3
                nat-target    192.168.30.3
            exit

            service-route  service_4_local_route
                name          service_4_local_route
                service-name  service_4
                nat-target    192.168.30.3
            exit

            service-route  service_5_local_route
                name          service_5_local_route
                service-name  service_5
                nat-target    192.168.40.4
            exit

            service-route  service_6_local_route
                name          service_6_local_route
                service-name  service_6
                nat-target    192.168.70.7
            exit

            service-route  service_7_local_route
                name          service_7_local_route
                service-name  service_7
                nat-target    192.168.70.7
            exit

            service-route  service_8_local_route
                name          service_8_local_route
                service-name  service_8
                nat-target    192.168.80.8
            exit

            service-route  service_9_local_route
                name          service_9_local_route
                service-name  service_9
                nat-target    192.168.80.8
            exit

            service-route  service_10_local_route
                name          service_10_local_route
                service-name  service_10
                nat-target    192.168.90.9
            exit

            service-route  datacenter1-route
                name          datacenter1-route
                service-name  testservice
                peer          bostonsite1
            exit
        exit
    exit
exit

```

The same structure shown below is repeated.

```
service-route  service_1_local_route
    name          service_1_local_route
    service-name  service_1
    nat-target    192.168.10.1
exit
```

Where, each **service-name** within the **service-route** has an associated **nat-target IP**.
Like a dictionary map of ip to service names.

### How to templatize it?

- Create a dictionary mapping IPs and services.

```
{%- assign service_dict = "
  192.168.10.1:service_1,service_2#
  192.168.30.3:service_3,service_4#
  192.168.40.4:service_5,service_5#
  192.168.70.7:service_6,service_7#
  192.168.80.8:service_8,service_9#
  192.168.90.9:service_10" | strip | split: "#"
%}
```

- The IPs can be accessed by
```
{% assign dictKeyVal = item | split: ":" %}
{% assign ip = dictKeyVal[0] %}
```

- Followed by it's list of services:
```
{% assign dictValue = dictKeyVal[1] | strip %}
{% assign service_list = dictValue | split: "," %}
```

- Resulting config template.

```
{% for item in service_dict %}
          {% assign dictKeyVal = item | split: ":" %}
          {% assign ip = dictKeyVal[0] %}
          {% assign dictValue = dictKeyVal[1] | strip %}
          {% assign service_list = dictValue | split: "," %}
    {% for service in service_list %}
          {
            "name": "{{ service | strip }}_local_route",
            "serviceName": "{{ service | remove: "'" }}",
            "natTarget": "{{ ip | strip | remove: "'" }}"
          },
    {% endfor %}
{% endfor %}
```

We put this all together in the template [template](template)
