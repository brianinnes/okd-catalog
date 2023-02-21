# Catalog Operators

<!--- cSpell:ignore Tekton Istio Knative minikube devworkspace gitops -->

The RedHat catalog on OpenShift Container Platform contains over 90 operators.  This experiment will concentrate on a small set of operators, primarily related to development activities:

|Operator               | Git repo                                         | Description                               |
|-----------------------|--------------------------------------------------|-------------------------------------------|
| [DevWorkspace Operator](operators/devworkspace.md) | [https://github.com/devfile/devworkspace-operator](https://github.com/devfile/devworkspace-operator){:target=_blank} | Dependency for the Che workspaces operator |
| [Data Foundation](operators/data-foundation.md) | [https://github.com/red-hat-storage/odf-operator](https://github.com/red-hat-storage/odf-operator){:target=_blank} | Storage operator to provide storage services, such as object storage |
| [Pipelines (Tekton)](operators/pipelines.md) | [https://github.com/tektoncd/operator](https://github.com/tektoncd/operator){:target=_blank} | Tekton pipelines operator |
| [GitOps (ArgoCD)](operators/gitops.md) | [https://github.com/redhat-developer/gitops-operator](https://github.com/redhat-developer/gitops-operator){:target=_blank} | ArgoCD operator |
| [Service Mesh (Istio)](operators/service-mesh.md) | [https://github.com/maistra/istio-operator](https://github.com/maistra/istio-operator){:target=_blank} | Istio Service Mesh operator |
| [Serverless (Knative serving)](operators/serverless.md) | [https://github.com/openshift-knative/serverless-operator](https://github.com/openshift-knative/serverless-operator){:target=_blank} | Serverless Knative serving operator |

## Work to do for each operator

For each of the operators in the table above we need to:

1. Determine if pre-built containers are available in the public domain, or if we will need to build the containers
2. Determine if the catalog data is available (operator bundles, channels and update details) in the repo or if we will need to create it
3. Verify if pre-built containers are valid, or if they contain links to unavailable containers
4. If we need to build any content, then what commands/tooling are needed.  Will the [operator pipeline](https://github.com/okd-project/okd-operator-pipeline){:target=_blank} be able to build it?

## Work to do to create an official OKD catalog

Getting a catalog working is the first stage, but to create a catalog to be **officially** released for OKD requires additional work:

- is it possible to find out how OCP creates the RedHat catalog, is there anything we can reuse for the OKD catalog?
    - is the config in public domain?
    - can we validate we have the correct source repos identified?
    - can we convert the Prow automation to the pipeline technology we choose to use?
- we need to create automation, driven off a GitHub repo to build the operators as required then update the catalog to include new operators
    - should allow use on public cloud or in private environments, on and off OKD (e.g. minikube) to allow customization of the OKD catalog, or the catalog to be used as a basis for custom catalogs
- should we use pre-built containers, or is this unsafe and would be better for us to build all the containers from source?
    - is there any reason to possible rebase the containers on UBI/Fedora/CentOS Stream base images?
- what is the governance we need to put in place to control the catalog?
- what testing / validation do we need to have in place to validate operators/catalog before release?
- what naming convention are we going to use?
    - if we use different names from the RedHat catalog, then we break reuse of automation (Ansible / Terraform) between OKD and OCP
    - are there any legal limitations regarding use of RedHat/OpenShift names within the catalog?
