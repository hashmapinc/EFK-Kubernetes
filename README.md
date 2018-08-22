## Elasticsearch

These manifests install Elasticsearch itself as a StatefulSet of two pods that will allocate a persistent volume of 20 GB per pod (make sure to pick a size that make sense for your workload). A Service is created in front of the StatefulSet pods to load balance them.
Elasticsearch is also configured to run under the service account elasticsearch-logging, which gets bound to the role of the same name in order for it to have the right permissions.

* es-statefulset.yaml
* es-service.yaml

## Elasticsearch Curator

Alongside Elasticsearch itself we deploy a service called Elasticsearch Curator, which does automatic maintenance of your Elasticsearch cluster. In our case we make it delete indices older than three days. If you want to tweak this configuration, Base64 decode the values for action_file.yaml and/or config.yaml in es-curator-secret.yaml (Kubernetes requires secret values to be Base64 encoded), make your changes and re-insert the Base64 encoded contents of respective files.

* es-curator.yaml
* es-curator-secret.yaml

## Fluentd

Fluentd is installed as a DaemonSet, which means that a corresponding pod will run on every Kubernetes worker node in order to collect its logs (and send them to Elasticsearch). Furthermore, the pods run as the service account fluentd-es which is bound to the cluster role with the same name in order to have the necessary permissions.

* fluentd-es-configmap.yaml
* fluentd-es-ds.yaml

## Kibana

Installs a Deployment, which ensures that one pod is always running, and a Service in front of it (which is capable of load balancing in case there should be several pods in parallel).

* kibana-deployment.yaml
* kibana-service.yaml
