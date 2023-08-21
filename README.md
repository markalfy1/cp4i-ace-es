# Cloud Pak for Integration
## Pre-requisites
### Set up GitOps
## GitOps Repository Structure
There are a total of 4 Git repositories involved with the GitOps workflow.

- [Main GitOps repository](https://github.com/cloud-native-toolkit/multi-tenancy-gitops) - Repository that the various [personas](#personas) will interact with to update the desired state of the OpenShift cluster.
- [Infrastructure repository](https://github.com/cloud-native-toolkit/multi-tenancy-gitops-infra) - Repository
- [Services repository](https://github.com/cloud-native-toolkit/multi-tenancy-gitops-services)
- [Application repository](https://github.com/cloud-native-toolkit-demos/multi-tenancy-gitops-apps)


### **GitOps**
- Main GitOps repository ([https://github.com/cloud-native-toolkit/multi-tenancy-gitops](https://github.com/cloud-native-toolkit/multi-tenancy-gitops)): This repository contains all the ArgoCD Applications for  the `infrastructure`, `services` and `application` layers.  Each ArgoCD Application will reference a specific K8s resource (yaml resides in a separate git repository), contain the configuration of the K8s resource, and determine where it will be deployed into the cluster.

- Directory structure for `single-cluster` or `multi-cluster` profiles:

    ```bash
    .
    ├── 1-infra
    ├── 2-services
    ├── 3-apps
    ├── bootstrap.yaml
    └── kustomization.yaml
    ```

The contents of the `kustomization.yaml` will determine which layer(s) will be deployed to the cluster.  This is based on whether the `resources` are commented out or not.  Each of the listed YAMLs contains an ArgoCD Application which in turn tracks all the K8s resources available to be deployed.  This follows the ArgoCD [app of apps pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/#app-of-apps-pattern).

```yaml
resources:
- 1-infra/1-infra.yaml
- 2-services/2-services.yaml
- 3-apps/3-apps.yaml
```


### **Infrastructure Layer**
- Infrastructure GitOps repository ([https://github.com/cloud-native-toolkit/multi-tenancy-gitops-infra](https://github.com/cloud-native-toolkit/multi-tenancy-gitops-infra)): Contains the YAMLs for cluster-wide and/or infrastructure related K8s resources managed by a cluster administrator.  This would include `namespaces`, `clusterroles`, `clusterrolebindings`, `machinesets` to name a few.

```bash
1-infra
├── 1-infra.yaml
├── argocd
│   ├── consolelink.yaml
│   ├── consolenotification.yaml
│   ├── infraconfig.yaml
│   ├── machinesets.yaml
│   ├── namespace-ci.yaml
│   ├── namespace-dev.yaml
│   ├── namespace-ibm-common-services.yaml
│   ├── namespace-istio-system.yaml
│   ├── namespace-openldap.yaml
│   ├── namespace-openshift-storage.yaml
│   ├── namespace-prod.yaml
│   ├── namespace-sealed-secrets.yaml
│   ├── namespace-staging.yaml
│   ├── namespace-tools.yaml
│   └── storage.yaml
└── kustomization.yaml
```

<details>
<summary> Contents of the `kustomization.yaml` will determine which resources are deployed to the cluster</summary>

```yaml
resources:
- argocd/consolelink.yaml
- argocd/consolenotification.yaml
- argocd/namespace-ibm-common-services.yaml
- argocd/namespace-ci.yaml
- argocd/namespace-dev.yaml
- argocd/namespace-staging.yaml
#- argocd/namespace-prod.yaml
#- argocd/namespace-istio-system.yaml
#- argocd/namespace-openldap.yaml
- argocd/namespace-sealed-secrets.yaml
- argocd/namespace-tools.yaml
#- argocd/namespace-openshift-storage.yaml
#- argocd/operator-ocs.yaml
#- argocd/refarch-infraconfig.yaml
#- argocd/refarch-machinesets.yaml
```

</details>


### **Services Layer**
- Services GitOps repository ([https://github.com/cloud-native-toolkit/multi-tenancy-gitops-services](https://github.com/cloud-native-toolkit/multi-tenancy-gitops-services)): Contains the YAMLs for K8s resources which will be used by the `application` layer.  This could include `subscriptions` for Operators, YAMLs of custom resources provided, or Helm Charts for tools provided by a third party.  These resource would usually be managed by the Administrator(s) and/or a DevOps team supporting application developers.

```bash
2-services
├── 2-services.yaml
├── argocd
│   ├── instances
│   │   ├── artifactory.yaml
│   │   ├── cert-manager-instance.yaml
│   │   ├── chartmuseum.yaml
│   │   ├── developer-dashboard.yaml
│   │   ├── grafana-instance.yaml
│   │   ├── ibm-foundational-services-instance.yaml
│   │   ├── ibm-platform-navigator-instance.yaml
│   │   ├── ibm-process-mining-instance.yaml
│   │   ├── openldap.yaml
│   │   ├── openshift-service-mesh-instance.yaml
│   │   ├── pact-broker.yaml
│   │   ├── sealed-secrets.yaml
│   │   ├── sonarqube.yaml
│   │   └── swaggereditor.yaml
│   └── operators
│       ├── cert-manager.yaml
│       ├── elasticsearch.yaml
│       ├── grafana-operator.yaml
│       ├── ibm-ace-operator.yaml
│       ├── ibm-apic-operator.yaml
│       ├── ibm-aspera-operator.yaml
│       ├── ibm-assetrepository-operator.yaml
│       ├── ibm-automation-foundation-core-operator.yaml
│       ├── ibm-catalogs.yaml
│       ├── ibm-cp4i-operators.yaml
│       ├── ibm-datapower-operator.yaml
│       ├── ibm-eventstreams-operator.yaml
│       ├── ibm-foundations.yaml
│       ├── ibm-mq-operator.yaml
│       ├── ibm-opsdashboard-operator.yaml
│       ├── ibm-platform-navigator.yaml
│       ├── ibm-process-mining-operator.yaml
│       ├── jaeger.yaml
│       ├── kiali.yaml
│       ├── openshift-gitops.yaml
│       ├── openshift-pipelines.yaml
│       └── openshift-service-mesh.yaml
└── kustomization.yaml
```


<details>
<summary> Contents of the `kustomization.yaml` will determine which resources are deployed to the cluster</summary>

```yaml
resources:
# IBM Software
- argocd/operators/ibm-ace-operator.yaml
#- argocd/operators/ibm-apic-operator.yaml
#- argocd/operators/ibm-aspera-operator.yaml
#- argocd/operators/ibm-assetrepository-operator.yaml
#- argocd/operators/ibm-cp4i-operators.yaml
#- argocd/operators/ibm-datapower-operator.yaml
#- argocd/operators/ibm-eventstreams-operator.yaml
#- argocd/operators/ibm-mq-operator.yaml
#- argocd/operators/ibm-opsdashboard-operator.yaml
#- argocd/operators/ibm-process-mining-operator.yaml
#- argocd/instances/ibm-process-mining-instance.yaml
- argocd/operators/ibm-platform-navigator.yaml
- argocd/instances/ibm-platform-navigator-instance.yaml

# IBM Foundations / Common Services
- argocd/operators/ibm-foundations.yaml
- argocd/instances/ibm-foundational-services-instance.yaml
- argocd/operators/ibm-automation-foundation-core-operator.yaml

# IBM Catalogs
- argocd/operators/ibm-catalogs.yaml

# Required for IBM MQ
#- argocd/instances/openldap.yaml
# Required for IBM ACE, IBM MQ
#- argocd/operators/cert-manager.yaml
#- argocd/instances/cert-manager-instance.yaml

# Sealed Secrets
- argocd/instances/sealed-secrets.yaml

# CICD
#- argocd/operators/grafana-operator.yaml
#- argocd/instances/grafana-instance.yaml
#- argocd/instances/artifactory.yaml
#- argocd/instances/chartmuseum.yaml
#- argocd/instances/developer-dashboard.yaml
#- argocd/instances/swaggereditor.yaml
#- argocd/instances/sonarqube.yaml
#- argocd/instances/pact-broker.yaml
# In OCP 4.7+ we need to install openshift-pipelines and possibly privileged scc to the pipeline serviceaccount
- argocd/operators/openshift-pipelines.yaml

# Service Mesh
#- argocd/operators/elasticsearch.yaml
#- argocd/operators/jaeger.yaml
#- argocd/operators/kiali.yaml
#- argocd/operators/openshift-service-mesh.yaml
```

</details>


