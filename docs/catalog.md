# Create Catalog

<!--- cSpell:ignore mkdir brianinnesuk openshift -->

It is easy to create a new catalog using instructions found in the [Operator Lifecycle Manager documentation](https://olm.operatorframework.io){target=_blank}.  You also need to have the [operator development tooling](https://sdk.operatorframework.io/docs/installation/){target=_blank} installed.

!!! Todo
    Windows commands, or assume Linux Subsystem for Windows can be used?

1. create a directory for the operator catalog

    ```sh
    mkdir okd-catalog
    ```

2. generate the container file

    ```shell
    opm generate dockerfile okd-catalog
    ```

## Add operators

Once the catalog structure is created and the container file has been created you can add operators to the catalog.

Operators are added by creating a directory in the okd-catalog director and adding details of the operator bundle.  This information can be added using **File Based Catalog** (FCB) or the new **Veneers** to define the operators.  Veneers are currently in *alpha* and subject to change, but provide a simplification over FCB.

Details of how to add operators will be covered in more detail in subsequent pages

## Building the catalog

Before building a catalog you should validate the catalog definitions:

```bash
opm validate okd-catalog
```

once the validation passes you can build the catalog images:

```bash
podman build . -f okd-catalog.Dockerfile -t quay.io/brianinnesuk/okd-catalog:latest
podman push quay.io/brianinnesuk/okd-catalog:latest
```

## Adding the catalog to an OKD cluster

!!! Warning
    This repo and the operator catalog created should be considered as **experimental** and work in progress so should not be installed on an important cluster

You can add the catalog to an OKD cluster using the admin console or command line to set a new CatalogSource CRD:

```yaml
apiVersion: operators.coreos.com/v1alpha1
kind: CatalogSource
metadata:
  name: okd-catalog
  namespace: openshift-marketplace
spec:
  displayName: OKD Catalog
  image: quay.io/brianinnesuk/okd-catalog
  publisher: OKD Community
  sourceType: grpc
  updateStrategy:
    registryPoll:
      interval: 10m0s
```
