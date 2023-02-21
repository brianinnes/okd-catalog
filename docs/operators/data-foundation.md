# Data Foundation

<!--- cSpell:ignore kube kubebuilder openshift brianinnesuk -->

Git Repository : [https://github.com/red-hat-storage/odf-operator](https://github.com/red-hat-storage/odf-operator){:target=_blank}

This operator repo contains the data OPM data in the [catalog](https://github.com/red-hat-storage/odf-operator/tree/main/catalog){:target=_blank} folder.

This operator publishes containers to [quay.io](https://quay.io/organization/ocs-dev){:target=_blank}, so it looks like no containers need to be built to use this on OKD - this turns out not to be the case!

Copying the bundle.yaml and index.yaml files from the catalog directory of odf-operator repository into the [okd-catalog/odf-operator](https://github.com/brianinnes/okd-catalog/tree/main/okd-catalog/odf-operator){:target=_blank} directory will add the operator to the catalog.

Verify the catalog is valid by running the following command in the root directory of the project:

```bash
opm validate okd-catalog
```

once the validation passes you can build the catalog images:

```bash
podman build . -f okd-catalog.Dockerfile -t quay.io/brianinnesuk/okd-catalog:latest
podman push quay.io/brianinnesuk/okd-catalog:latest
```

## Operator imports container images from protected registry

The operator appears in the catalog and can be selected to be installed on an OKD cluster, however, the install will not complete as the operator tries to pulls the **ose-kube-rbac-proxy** image from the RedHat registry which is protected by a pull secret.  This suggests that we will need to build the operator containers using an alternate kube-rbac-proxy image

## Kube RBAC Proxy

A Kube RBAC Proxy is needed by many operators.  There are a number of containers available:

- registry.redhat.io/openshift4/ose-kube-rbac-proxy
- gcr.io/kubebuilder/kube-rbac-proxy

The OpenShift version seems to be sourced in repo [https://github.com/openshift-auth/kube-rbac-proxy-downstream](https://github.com/openshift-auth/kube-rbac-proxy-downstream){:target=_blank} or [https://github.com/openshift/kube-rbac-proxy](https://github.com/openshift/kube-rbac-proxy){:target=_blank}.

!!!Todo
    Determine which is correct git repo and see if there are any built containers in a public repo.  If not we need to decide if the google implementation can be used or if we need to build the containers
