# AMQ Interconnect multi-zone demo on OpenShift

Setting up:

- 3 externally-facing AMQ Interconnect routers, 1 per "zone"

- 3 interior routers, 1 per "zone"

- 3 AMQ brokers which share the same configuration, but separate storage

## How to deploy

To deploy this demo:

1.  Edit the `inter-router`, `intra-router` and `broker` YAMLs and specify the label key and value that you wish to use to ensure that the Pods are scheduled into separate "zones", using affinity. I've used the Node name, for example.

2.  Apply all of the provided YAMLs into a namespace, e.g. `oc apply -f ...`

You should get a nice console like this:

<img src="irconsole.png" width="500">


