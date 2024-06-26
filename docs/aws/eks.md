# EKS

# `eksclt`

https://eksctl.io/

https://eksctl.io/getting-started/

## Commands

> **WARNING**
>
> The `eksctl create cluster` execution will overwrite the `~/.kube/config` file to configure access

Create default cluster with:

- exciting auto-generated name
- two m5.large worker nodes
- use the official AWS EKS AMI
- us-west-2 region
- a dedicated VPC (check your quotas)

```shell
eksctl create cluster
```

For forcing a specific version on k8s:

```shell
eksctl create cluster --version=1.24
```

Listing existing clusters

```shell
aws eks list-clusters
```

Describe cluster shows endpoint and VPC and internet access exposure

```shell
aws eks describe-cluster --name $CLUSTER_NAME
```

Or for changing default behaviour a yaml config file can be used:

[medium2nodesCluster.yaml](./2_large_node_cluster.yaml):

```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: cluster-1
  region: eu-west-1
  version: '1.27'
nodeGroups:
  - name: ng-1
    instanceType: m5.large
    desiredCapacity: 2
    availabilityZones: [ "eu-west-1a", "eu-west-1b" ]
```

````shell
eksctl create cluster -f medium2nodesCluster.yaml
````

or use manage node groups instead:

```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: manual-created-1-29-ng
  region: ap-northeast-2
  version: '1.29'

managedNodeGroups:
  - name: ng-1-workers-amd
    labels: { role: workers }
    instanceType: m5.large
    desiredCapacity: 0
    volumeSize: 80
    privateNetworking: true
  - name: ng-2-workers-arm
    labels: { role: workers }
    instanceType: m7g.large
    desiredCapacity: 0
    volumeSize: 100
    privateNetworking: true
```

> **~~TODO~~**: getting an error using this file, I think related with permissions:
>
> **REASON**: m5.medium doesn't exist. The error describing the AZs is not related.
>
> ```shell
> 2023-11-22 16:04:00 [ℹ]  eksctl version 0.164.0
> 2023-11-22 16:04:00 [ℹ]  using region eu-west-1
> 2023-11-22 16:04:01 [ℹ]  skipping eu-west-1a from selection because it doesn't support the following instance type(s): m5.medium
> 2023-11-22 16:04:01 [ℹ]  skipping eu-west-1b from selection because it doesn't support the following instance type(s): m5.medium
> 2023-11-22 16:04:01 [ℹ]  skipping eu-west-1c from selection because it doesn't support the following instance type(s): m5.medium
> Error: getting availability zones: only 0 zones discovered [], at least 2 are required
> ```

Testing a config file using [`--dry-run`](https://eksctl.io/usage/dry-run/):

```shell
eksctl create cluster -f 2_large_node_cluster.yaml --dry-run 
```

Configure kubectl

```shell
aws eks update-kubeconfig --region $AWS_REGION --name $EKS_CLUSTER_NAME
```
