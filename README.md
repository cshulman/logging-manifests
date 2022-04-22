#!/bin/bash

# Patch openshift-operators-redhat namespace for elasticsearch
# by adding label 'openshift.io/cluster-monitoring=true'
oc apply -f elastic-namespace.yaml


# Manually add label to openshift-logging namespace
oc label namespace openshift-logging openshift.io/cluster-monitoring=true
# Or run the script which will run the command
./logging-namespace.sh

# Deploy elasticsearch operator
oc apply -f elastic-subscription.yaml

# Deploy logging operator
oc apply -f logging-subscription.yaml

# Create logging instance
oc apply -f logging-instance.yaml

# After logging instance is succesfully created, access Kibana UI 
# & define neccessary index patterns to view logs
# https://docs.openshift.com/container-platform/4.10/logging/cluster-logging-deploying.html#cluster-logging-visualizer-indices_cluster-logging-deploying


# Audit logs are not store by default & can be configured by following 
# https://docs.openshift.com/container-platform/4.10/logging/config/cluster-logging-log-store.html


