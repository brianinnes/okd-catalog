# DevWorkspace

<!--- cSpell:ignore kube openshift kubebuilder devfile brianinnesuk devworkspace -->

Git repository : [https://github.com/devfile/devworkspace-operator](https://github.com/devfile/devworkspace-operator){:target=_blank}

This operator repo contains the data OPM data in the [olm-catalog](https://github.com/devfile/devworkspace-operator/tree/main/olm-catalog){:target=_blank} folder.  However, there is an issue with the data which is discussed below.

This operator publishes containers to [quay.io](https://quay.io/organization/devfile){:target=_blank}, so no containers need to be built to use this on OKD

## Catalog data

The repo contains the data for the Operator Lifecycle Manager, but in the required repository section it lists the **kube_rbac_proxy** being located at **registry.redhat.io/openshift4/ose-kube-rbac-proxy:v4.8**, which requires an access token to access the RedHat registry.  I think this is an error, as the operator uses the Google image located at **gcr.io/kubebuilder/kube-rbac-proxy:v0.13.1**

To add the operator to the catalog, the opportunity to explore the new veneer feature was chosen.  The veneer is defined in file [okd-catalog/devworkspace-operator/devworkspace-operator-veneer.yaml](https://github.com/brianinnes/okd-catalog/blob/main/okd-catalog/devworkspace-operator/devworkspace-operator-veneer.yaml){:target=_blank}:

```yaml
Schema: olm.semver
GenerateMajorChannels: true
GenerateMinorChannels: false
fast:
  Bundles:
  - Image: quay.io/devfile/devworkspace-operator-bundle:v0.19.0
  - Image: quay.io/devfile/devworkspace-operator-bundle:v0.18.1
  - Image: quay.io/devfile/devworkspace-operator-bundle:v0.18.0
  - Image: quay.io/devfile/devworkspace-operator-bundle:v0.17.0
  - Image: quay.io/devfile/devworkspace-operator-bundle:v0.16.0
  - Image: quay.io/devfile/devworkspace-operator-bundle:v0.15.3
  - Image: quay.io/devfile/devworkspace-operator-bundle:v0.15.2
```

This veneer can be converted into the **catalog.yaml** file needed by the Operator Lifecycle Manager using the following command in the devworkspace-operator folder:

```bash
opm alpha render-veneer semver -o yaml devworkspace-operator-veneer.yaml > catalog.yaml
```

Verify the catalog is valid by running the following command in the root directory of the project:

```bash
opm validate okd-catalog
```

once the validation passes you can build the catalog images:

```bash
podman build . -f okd-catalog.Dockerfile -t quay.io/brianinnesuk/okd-catalog:latest
podman push quay.io/brianinnesuk/okd-catalog:latest
```
