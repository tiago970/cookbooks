# Kubernetes Operations (kOps) Instance Group (IG)

<!--
c5.2xlarge 10 gb
-->

## Tips

### Delete Node

```sh
#
export KOPS_STATE_STORE='s3://k8s-kops-state-store'

# Get first cluster description
kops get cluster \
  -o json | \
    jq

# Good pattern [cluster-name]-[region].k8s.local
export KOPS_CLUSTER_NAME='dev01-us-east-1.k8s.local' # prod01, stg01, uat01

#
kubectl drain [name] \
  --ignore-daemonsets \
  --delete-local-data

#
kubectl delete node [name]
```

### Edit Instance Group (IG)

```sh
#
export KOPS_STATE_STORE='s3://k8s-kops-state-store'

# Get first cluster description
kops get cluster \
  -o json | \
    jq

# Good pattern [cluster-name]-[region].k8s.local
export KOPS_CLUSTER_NAME='dev01-us-east-1.k8s.local' # prod01, stg01, uat01

#
kops get ig

#
export KOPS_NODE_NAME='nodes-us-east-1a'

#
kops get ig \
  -o yaml \
  "$KOPS_NODE_NAME"

#
kops edit ig "$KOPS_NODE_NAME"

#
kops update cluster \
  --admin \
  --yes

#
kops rolling-update cluster \
  --yes

#
kops validate cluster
```

<!--
c5a.4xlarge
c5a.2xlarge

t3.large
-->

<!-- ## Issues

###

```log
Machine	i-04e6420c3615e1b15		machine "i-04e6420c3615e1b15" has not yet joined cluster
Node	ip-10-96-88-13.ec2.internal	node "ip-10-96-88-13.ec2.internal" of role "node" is not ready
Validation failed: cluster not yet healthy
```

TODO -->
