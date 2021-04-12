# Openshift Cluster Config
Setting up an OpenShift cluster using Kustomize and ArgoCD. More info coming soon.

Content heavily borrowed from [Christianh814's OpenShift-cluster-config repo](https://github.com/christianh814/openshift-cluster-config):

## Installing the OpenShift GitOps Operator

> :warning: This is based on the OpenShift GitOps operator using an "Automatic" update strategy on OpenShift 4.7. As of 4.7, the operator is still `Tech Preview`

You can install the operator using this repo by running the following `OC` command:

```
until oc apply -k https://github.com/beelandc/openshift4-gitops-demo/gitops-operator/install; do sleep 2; done
```

This will start the installation of the GitOps operator in the `openshift-operators` namespace. As part of the operator install, a default instance of argocd will be created in the `openshift-gitops` namespace.

To get your argocd route (where you can login)

```
oc get route argocd-cluster-server -n openshift-gitops -o jsonpath='{.spec.host}{"\n"}'
```

## Deploying this Repo

To configure your cluster to this repo run

```
oc apply -k https://github.com/beelandc/openshift4-gitops-demo/gitops-config/config/overlays/default
```

This will configure your server with the following.

Cluster Configurations:
* machineconfigs applied -- Example Hardening configuration
* Two Groups created
  * `admins`
    * `ocp-admin` is part of `admins`
  * `developer`
    * `ocp-developer` is part of `developer`
* ClusterRole/Role Bindings setup
  * `admins` group has `cluster-admin` on OpenShift
  * The `developer` group has `edit` on the `pricelist` namespace on OpenShift
* Compliance Operator
* Container Security Operator installed

Application Deployments:
* Deploy Pricelist in an ArgoCD project called `pricelist`
  * One `application` Consisting of...
    * Frontend Web Application
    * Backend Database store
    * Job that creates database tables and the such

ArgoCD Configurations
* The `cluster-config` ArgoCD project has all "cluster wide" configurations
  * Can only be seen/synced by ArgoCD admins
* The `pricelist` ArgoCD project has all application components to run the Pricelist application
  * Can be seen/synced by ArgoCD admins or ArgoCD users
* Autosync is turned on
