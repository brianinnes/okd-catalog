---
template: home.html
title: OKD Catalog
---

<!--- cSpell:ignore devfile Kustomize -->

This repo is my experimentation in creating an operator catalog for OKD to replace the redhat catalog in OCP.

## Creating a catalog

It is easy to create a new catalog using instructions found in the [Operator Lifecycle Manager documentation](https://olm.operatorframework.io){target=_blank}.  You also need to have the operator development tooling installed.

1. create a directory for the operator catalog

    ```sh
    mkdir okd-catalog
    ```

2. generate the container file

    ```shell
    opm generate dockerfile okd-catalog
    ```
