# Terraform Kubernetes Engine ASM Submodule

This module installs [Anthos Service Mesh](https://cloud.google.com/service-mesh/docs) (ASM) in a Kubernetes cluster.

Specifically, this module automates the following steps for [installing ASM](https://cloud.google.com/service-mesh/docs/install):

1. Installing the ASM Istio Operator on your cluster.
2. Optionally registering your cluster with GKE Hub.

## Usage

There is a [full example](../../examples/simple_zonal_with_asm) provided. Simple usage is as follows:

```tf
module "asm" {
  source           = "terraform-google-modules/kubernetes-engine/google//modules/asm"

  project_id       = "my-project-id"
  cluster_name     = "my-cluster-name"
  location         = module.gke.location
  cluster_endpoint = module.gke.endpoint
}
```

To deploy this config:
1. Run `terraform apply`

## Requirements

- Anthos Service Mesh [requires](https://cloud.google.com/service-mesh/docs/gke-install-existing-cluster#requirements) an active Anthos license.
- GKE cluster must have minimum four nodes.
- Minimum machine type is `e2-standard-4`.
- GKE cluster must be enrolled in a release channel. ASM does not support static version.
- ASM on a private GKE cluster requires adding a firewall rule to open port 15017 if you want to use [automatic sidecar injection](https://cloud.google.com/service-mesh/docs/proxy-injection).
- Only one ASM per Google Cloud project is supported.


 <!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| asm\_dir | Name of directory to keep ASM resource config files. | string | `"asm-dir"` | no |
| asm\_version | ASM version to deploy. Available versions are documented in https://github.com/GoogleCloudPlatform/anthos-service-mesh-packages | string | `"release-1.6-asm"` | no |
| cluster\_endpoint | The GKE cluster endpoint. | string | n/a | yes |
| cluster\_name | The unique name to identify the cluster in ASM. | string | n/a | yes |
| enable\_gke\_hub\_registration | Enables GKE Hub Registration when set to true | bool | `"true"` | no |
| gcloud\_sdk\_version | The gcloud sdk version to use. Minimum required version is 293.0.0 | string | `"296.0.1"` | no |
| gke\_hub\_membership\_name | Memebership name that uniquely represents the cluster being registered on the Hub | string | `"gke-asm-membership"` | no |
| gke\_hub\_sa\_name | Name for the GKE Hub SA stored as a secret `creds-gcp` in the `gke-connect` namespace. | string | `"gke-hub-sa"` | no |
| internal\_ip | Use internal ip for the cluster endpoint. | bool | `"false"` | no |
| location | The location (zone or region) this cluster has been created in. | string | n/a | yes |
| project\_id | The project in which the resource belongs. | string | n/a | yes |
| service\_account\_key\_file | Path to service account key file to auth as for running `gcloud container clusters get-credentials`. | string | `""` | no |
| skip\_gcloud\_download | Whether to skip downloading gcloud (assumes gcloud and kubectl already available outside the module) | bool | `"true"` | no |

## Outputs

| Name | Description |
|------|-------------|
| asm\_wait | An output to use when you want to depend on ASM finishing |
| hub\_wait | An output to use when you want to depend on GKE hub finishing |

 <!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
