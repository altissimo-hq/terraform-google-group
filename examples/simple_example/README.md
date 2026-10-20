# Simple Example

This example illustrates how to use the `group` module.

<!-- BEGIN_TF_DOCS -->
## Requirements

No requirements.

## Providers

| Name | Version |
| ---- | ------- |
| <a name="provider_google"></a> [google](#provider\_google) | n/a |

## Modules

| Name | Source | Version |
| ---- | ------ | ------- |
| <a name="module_group"></a> [group](#module\_group) | terraform-google-modules/group/google | ~> 0.6 |
| <a name="module_inner_group"></a> [inner\_group](#module\_inner\_group) | terraform-google-modules/group/google | ~> 0.6 |

## Resources

| Name | Type |
| ---- | ---- |
| [google_service_account.manager](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/service_account) | resource |
| [google_service_account.member](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/service_account) | resource |

## Inputs

| Name | Description | Type | Default | Required |
| ---- | ----------- | ---- | ------- | :------: |
| <a name="input_domain"></a> [domain](#input\_domain) | Domain of the organization to create the group in | `string` | n/a | yes |
| <a name="input_project_id"></a> [project\_id](#input\_project\_id) | The ID of the project in which to provision resources and used for billing | `string` | n/a | yes |
| <a name="input_suffix"></a> [suffix](#input\_suffix) | Suffix of the groups to create | `string` | n/a | yes |

## Outputs

| Name | Description |
| ---- | ----------- |
| <a name="output_group_id"></a> [group\_id](#output\_group\_id) | n/a |
| <a name="output_group_name"></a> [group\_name](#output\_group\_name) | n/a |
<!-- END_TF_DOCS -->

To provision this example, run the following from within this directory:
- `terraform init` to get the plugins
- `terraform plan` to see the infrastructure plan
- `terraform apply` to apply the infrastructure build
- `terraform destroy` to destroy the built infrastructure
