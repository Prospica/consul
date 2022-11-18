# Explanation to api.yaml and web.yaml
In this case, we have two pods, web and api. We want web pod can communicate with api pod using api service. There is some configuration (by adding annotations) we need to make this happen.

## web.yaml
In this pod, there is two annotations :
```
consul.hashicorp.com/connect-inject: 'true'
consul.hashicorp.com/transparent-proxy-exclude-inbound-ports: '9090'
```
The first one ``consul.hashicorp.com/connect-inject`` is to define that this pod will be injected with sidecar proxy and the service will be discovered by consul. We need to define this annotations because in [values.yaml](../consul/values.yaml) we define ``connectInject -> default:false``. We do this so not all pods in Openshift Cluster is injected (this will result many pods failure in master node).

The second one ``consul.hashicorp.com/transparent-proxy-exclude-inbound-ports`` is to exclude inbound service port to be redirected to sidecar proxy (this happen because we enable tranparent proxy more [info](../consul/)). We do this so we can access this web service via Openshift Route.

## api.yaml
In this pod, there is one annotations :
```
consul.hashicorp.com/connect-inject: 'true'
```
The first one ``consul.hashicorp.com/connect-inject`` is to define that this pod will be injected with sidecar proxy and the service will be discovered by consul. We need to define this annotations because in [values.yaml](../consul/values.yaml) we define ``connectInject -> default:false``. We do this so not all pods in Openshift Cluster is injected (this will result many pods failure in master node).