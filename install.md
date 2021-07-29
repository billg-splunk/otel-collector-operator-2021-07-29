1. Switch to `openshift-operators` namespace/project.
    `oc project openshift-operators`

2. Create new role for the OpenTelemetry collector. This will not be required after an upcoming version of the operator
    `oc apply -f role.yaml`

3. Store your Splunk APM Ingest API token as a secret in the `openshift-operators` namespace.
    `oc create secret generic splunk-otel-agent --from-literal=access-token=SPLUNK_TOKEN_HERE`

4. Install OpenTelemetry Operator
    oc apply -f https://github.com/open-telemetry/opentelemetry-operator/releases/latest/download/opentelemetry-operator.yaml

5. Deploy OpenTelemetry collector as a daemonset/agent
    Modify the cluster name in operator.yaml
    From command line
        `oc apply -f operator.yaml`

6. Verify the installable
    Verify that each node is succesfully running one copy of `splunk-otel-agent-collector`.
    Verify that metrics/traces are showing up in Splunk APM dashboard.