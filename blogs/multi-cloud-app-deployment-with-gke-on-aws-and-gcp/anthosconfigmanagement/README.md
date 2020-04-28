# Purpose
This repository holds all of the manifests required for Anthos Config Management to apply consistent configuraitons across each cluster. 

## Folder Layout

The layout of this directory is as follows: 

```shell
├── cluster
│   ├── rbac-manager.yaml
│   └── team-a-rbac.yaml
├── clusterregistry
├── namespaces
│   ├── default
│   │   └── namespace.yaml
│   ├── kube-system
│   │   └── namespace.yaml
│   ├── np-test-1
│   │   └── namespace.yml
│   └── rbac-manager
│       ├── deployment.yaml
│       └── namespace.yaml
├── README.md
└── system
    ├── config-management.yaml
    └── resourcequota-hierarchy.yaml
```

**Note** Please update this layout as required. 

## Sample ConfigManagement ConfigMap

The following Anthos Config Management manifest will be unique across each cluster and auto generated & applied with Terraform. 

```shell
apiVersion: addons.sigs.k8s.io/v1alpha1
kind: ConfigManagement
metadata:
  name: config-management
  namespace: config-management-system
spec:
  # clusterName is required and must be unique among all managed clusters
  clusterName: my-cluster
  git:
    syncRepo: git@github.com:ArctiqTeam/community-source.git
    syncBranch: "blog/multi-cloud-app-deployment-with-gke-on-aws-and-gcp"
    secretType: none
    policyDir: "blogs/multi-cloud-app-deployment-with-gke-on-aws-and-gcp/anthosconfigmanagement"
  # If true, namespaces in an abstract namespace share inherited
  # ResourceQuotas in aggregate.
  enableAggregateNamespaceQuotas: false
  ```

## Troubleshooting 
The following commands are helpful in troubleshooting ACM: 

- Viewing import logs
```shell
kubectl logs -f $(kubectl get pods -n config-management-system | grep git-import | awk '{print $1}') -c importer -n config-management-system
```

## Resources
- [Anthos Config Management](https://cloud.google.com/anthos-config-management)
