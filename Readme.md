# Openshift Cluster Config
Setting up an OpenShift cluster using Kustomize and ArgoCD. More info coming soon.

HEAVILY borrowed from [Christianh814's OpenShift-cluster-config repo](https://github.com/christianh814/openshift-cluster-config):

## Installing the OpenShift GitOps Operator

> :warning: This is based on the OpenShift GitOps operator using an "Automatic" update strategy on OpenShift 4.7. As of 4.7, the operator is still `Tech Preview`

You can install the operator using this repo by running the following `OC` command:

```
until oc apply -k https://github.com/beelandc/openshift-gitops-demo/gitops-operator/install; do sleep 2; done
```

This will start the installation of the GitOps operator in the `openshift-operators` namespace. As part of the operator install, a default instance of argocd will be created in the `openshift-gitops` namespace.

To get your argocd route (where you can login)

```
oc get route argocd-cluster-server -n openshift-gitops -o jsonpath='{.spec.host}{"\n"}'
```

