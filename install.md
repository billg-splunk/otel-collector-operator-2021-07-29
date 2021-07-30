0. If you don't have Openshift deployed, you can follow the instructions here:
* [CodeReady Containers](https://crc.dev/crc/#installation_gsg)
* Login as an admin
```
oc login -u kubeadmin https://api.crc.testing:6443
```
* Install cert-manager
```
oc create namespace cert-manager
oc apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v0.15.1/cert-manager.yaml
```

1. Switch to `openshift-operators` namespace/project.
```
oc project openshift-operators
```

2. Create new role for the OpenTelemetry collector. This will not be required after an upcoming version of the operator.
```
oc apply -f role.yaml
```

3. Store your Splunk APM Ingest API token as a secret in the `openshift-operators` namespace.
```
oc create secret generic splunk-otel-agent --from-literal=access-token=SPLUNK_TOKEN_HERE
```

4. Install OpenTelemetry Operator
```
oc apply -f https://github.com/open-telemetry/opentelemetry-operator/releases/latest/download/opentelemetry-operator.yaml
```

5. Deploy OpenTelemetry collector as a daemonset/agent
* First modify the cluster name in operator.yaml
```
oc apply -f operator.yaml
```

6. Verify the installation
* Verify that each node is succesfully running one copy of ```splunk-otel-agent-collector```
* For example:
```
➜ oc get po
NAME                                READY   STATUS    RESTARTS   AGE
splunk-otel-agent-collector-5t4f6   1/1     Running   0          3m39s
```
* Verify that metrics/traces are showing up in Splunk APM dashboard.
  * Login to Splunk Observability Cloud
  * Go to Infrastructure
  * Click on Kubernetes
  * You should see your cluster, named as you did in the previous step