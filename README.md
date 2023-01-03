# Create a managed cluster

```
region=eu-west-2
```

# Enable observability

Create a bucket in hub cluster region.

1. Create namesapce, pull secret

```
$ oc create namespace open-cluster-management-observability

$ DOCKER_CONFIG_JSON=`oc extract secret/pull-secret -n openshift-config --to=-`

$ oc create secret generic multiclusterhub-operator-pull-secret \
    -n open-cluster-management-observability \
    --from-literal=.dockerconfigjson="$DOCKER_CONFIG_JSON" \
    --type=kubernetes.io/dockerconfigjson
```

2. Edit *thanos-object-storage.yaml* with bucket name, access key and secret key. Create secret.

```
$ oc create -f thanos-object-storage.yaml -n open-cluster-management-observability
```

3. Create the MultiClusterObservability custom resource YAML file named multiclusterobservability_cr.yaml.
```
$ oc apply -f multiclusterobservability_cr.yaml
```

# Deploy applications

Go to Applications on hub cluster and deploy Subscription for namespace *org1* and *org2*.

# Add custom rules
TODO: Add rules for *org1* and *org2* apps  
-> Soit deployer stress pod  
-> Soit règles basiques types nb objet dans un namespace
```
$ oc create -f thanos-ruler-custom-rules.yaml
```

# Add alert manager
TODO: Add alert for *org1* and *org2* apps
```
$ oc -n open-cluster-management-observability create secret generic alertmanager-config --from-file=alertmanager.yaml --dry-run -o=yaml |  oc -n open-cluster-management-observability replace secret --filename=-
```

# TODO: Add custom metrics
TODO