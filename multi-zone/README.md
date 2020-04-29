# AMQ Interconnect multi-zone demo on OpenShift

Setting up:

- 3 externally-facing AMQ Interconnect routers, 1 per "zone"

- 3 interior routers, 1 per "zone"

- 3 AMQ brokers which share the same configuration, but separate storage

## How to deploy

To deploy this demo, apply all of the provided YAMLs into a namespace, e.g. `oc apply -f ...`

```
oc apply -f broker-shared-secrets.yml

oc apply -f router-shared-config.yml

oc apply -f router-shared-secrets.yml

oc process -f broker-template.yml -p ZONE_NAME=aztec -p NODE_SELECTOR_VALUE=node-0.example.com | oc apply -f -

oc process -f broker-template.yml -p ZONE_NAME=medieval -p NODE_SELECTOR_VALUE=node-1.example.com | oc apply -f -

oc process -f broker-template.yml -p ZONE_NAME=industrial -p NODE_SELECTOR_VALUE=node-2.example.com | oc apply -f -

oc process -f intra-router-template.yml -p ZONE_NAME=aztec -p NODE_SELECTOR_KEY=kubernetes.io/hostname -p NODE_SELECTOR_VALUE=node-0.example.com | oc apply -f -

oc process -f intra-router-template.yml -p ZONE_NAME=medieval -p NODE_SELECTOR_KEY=kubernetes.io/hostname -p NODE_SELECTOR_VALUE=node-1.example.com | oc apply -f -

oc process -f intra-router-template.yml -p ZONE_NAME=industrial -p NODE_SELECTOR_KEY=kubernetes.io/hostname -p NODE_SELECTOR_VALUE=node-2.example.com | oc apply -f -

oc process -f inter-router-template.yml -p ZONE_NAME=aztec -p NODE_SELECTOR_KEY=kubernetes.io/hostname -p NODE_SELECTOR_VALUE=node-0.example.com | oc apply -f -

oc process -f inter-router-template.yml -p ZONE_NAME=medieval -p NODE_SELECTOR_KEY=kubernetes.io/hostname -p NODE_SELECTOR_VALUE=node-1.example.com | oc apply -f -

oc process -f inter-router-template.yml -p ZONE_NAME=industrial -p NODE_SELECTOR_KEY=kubernetes.io/hostname -p NODE_SELECTOR_VALUE=node-2.example.com | oc apply -f -
```

You should get a nice console like this:

![](irconsole.png)
