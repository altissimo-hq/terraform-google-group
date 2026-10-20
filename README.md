# terraform-google-group

This module manages Cloud Identity Groups and Memberships using the
[Cloud Identity Group API](https://cloud.google.com/identity/docs/groups).

## Usage

Basic usage of this module is as follows:

```hcl
# Required if using User ADCs (Application Default Credentials) for Cloud Identity API.
provider "google-beta" {
  user_project_override = true
  billing_project       = "<PROJECT_ID>"
}

module "group" {
  source  = "terraform-google-modules/group/google"
  version = "~> 0.6"

  id           = "example-group@example.com"
  display_name = "example-group"
  description  = "Example group"
  domain       = "example.com"
  owners       = ["foo@example.com"]
  managers     = ["example-sa@my-project.iam.gserviceaccount.com"]
  members      = ["another-group@example.com"]
}
```

Functional examples are included in the [examples](./examples/) directory.

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
| ---- | ------- |
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.13 |
| <a name="requirement_google"></a> [google](#requirement\_google) | >= 6.0, < 9 |
| <a name="requirement_google-beta"></a> [google-beta](#requirement\_google-beta) | >= 6.0, < 9 |

## Providers

| Name | Version |
| ---- | ------- |
| <a name="provider_google"></a> [google](#provider\_google) | >= 6.0, < 9 |
| <a name="provider_google-beta"></a> [google-beta](#provider\_google-beta) | >= 6.0, < 9 |

## Modules

No modules.

## Resources

| Name | Type |
| ---- | ---- |
| [google-beta_google_cloud_identity_group.group](https://registry.terraform.io/providers/hashicorp/google-beta/latest/docs/resources/google_cloud_identity_group) | resource |
| [google-beta_google_cloud_identity_group_membership.managers](https://registry.terraform.io/providers/hashicorp/google-beta/latest/docs/resources/google_cloud_identity_group_membership) | resource |
| [google-beta_google_cloud_identity_group_membership.members](https://registry.terraform.io/providers/hashicorp/google-beta/latest/docs/resources/google_cloud_identity_group_membership) | resource |
| [google-beta_google_cloud_identity_group_membership.owners](https://registry.terraform.io/providers/hashicorp/google-beta/latest/docs/resources/google_cloud_identity_group_membership) | resource |
| [google_organization.org](https://registry.terraform.io/providers/hashicorp/google/latest/docs/data-sources/organization) | data source |

## Inputs

| Name | Description | Type | Default | Required |
| ---- | ----------- | ---- | ------- | :------: |
| <a name="input_customer_id"></a> [customer\_id](#input\_customer\_id) | Customer ID of the organization to create the group in. One of domain or customer\_id must be specified | `string` | `""` | no |
| <a name="input_description"></a> [description](#input\_description) | Description of the group | `string` | `""` | no |
| <a name="input_display_name"></a> [display\_name](#input\_display\_name) | Display name of the group | `string` | `""` | no |
| <a name="input_domain"></a> [domain](#input\_domain) | Domain of the organization to create the group in. One of domain or customer\_id must be specified | `string` | `""` | no |
| <a name="input_id"></a> [id](#input\_id) | ID of the group. For Google-managed entities, the ID must be the email address the group | `string` | n/a | yes |
| <a name="input_initial_group_config"></a> [initial\_group\_config](#input\_initial\_group\_config) | The initial configuration options for creating a Group. See the API reference for possible values. Possible values are INITIAL\_GROUP\_CONFIG\_UNSPECIFIED, WITH\_INITIAL\_OWNER, and EMPTY. | `string` | `"EMPTY"` | no |
| <a name="input_managers"></a> [managers](#input\_managers) | Managers of the group. Each entry is the ID of an entity. For Google-managed entities, the ID must be the email address of an existing group, user or service account | `list(string)` | `[]` | no |
| <a name="input_members"></a> [members](#input\_members) | Members of the group. Each entry is the ID of an entity. For Google-managed entities, the ID must be the email address of an existing group, user or service account | `list(string)` | `[]` | no |
| <a name="input_owners"></a> [owners](#input\_owners) | Owners of the group. Each entry is the ID of an entity. For Google-managed entities, the ID must be the email address of an existing group, user or service account | `list(string)` | `[]` | no |
| <a name="input_types"></a> [types](#input\_types) | The type of the group to be created. More info: https://cloud.google.com/identity/docs/groups#group_properties | `list(string)` | <pre>[<br/>  "default"<br/>]</pre> | no |

## Outputs

| Name | Description |
| ---- | ----------- |
| <a name="output_id"></a> [id](#output\_id) | ID of the group. For Google-managed entities, the ID is the email address the group |
| <a name="output_name"></a> [name](#output\_name) | Name of the group with the domain removed. For Google-managed entities, the ID is the email address the group |
| <a name="output_resource_name"></a> [resource\_name](#output\_resource\_name) | Resource name of the group in the format: groups/{group\_id}, where group\_id is the unique ID assigned to the group. |
<!-- END_TF_DOCS -->

## Limitations

The provider is still under development, the following are known issues or
limitations:

* Updating a `google_cloud_identity_group_membership` to remove a role fails
    with an error
    ([link](https://github.com/hashicorp/terraform-provider-google/issues/7616)).

* Updating a `google_cloud_identity_group_membership` to change the role of a
    member fails with the following error due to Terraform trying to create the
    new role assignment before/at the same time as the old one is removed.
    Rerunning the same deployment twice might resolve the issue.

    ```bash
    Error: Error creating GroupMembership: googleapi: Error 409: Error(4003): Cannot create membership 'user@example.com' in 'groups/xxx' because it already exists.
    Details:
    [
      {
        "@type": "type.googleapis.com/google.rpc.ResourceInfo",
        "description": "Error(4003): Cannot create membership 'user@example.com' in 'groups/xxx' because it already exists.",
        "owner": "domain:cloudidentity.googleapis.com",
        "resourceType": "cloudidentity.googleapis.com/Membership"
      },
      {
        "@type": "type.googleapis.com/google.rpc.DebugInfo",
        "detail": "[ORIGINAL ERROR] generic::already_exists: Error(4003): Cannot create membership 'user@example.com' in 'groups/xxx' because it already exists.\ncom.google.ccc.hosted.api.oneplatform.cloudidentity.error.exceptions.OpAlreadyExistsException: Error(4003): Cannot create membership 'user@example.com' in 'groups/xxx' because it already exists. [google.rpc.error_details_ext] { message: \"Error(4003): Cannot create membership \\'user@example.com\\' in \\'groups/xxx\\' because it already exists.\" details { [type.googleapis.com/google.rpc.ResourceInfo] { resource_type: \"cloudidentity.googleapis.com/Membership\" owner: \"domain:cloudidentity.googleapis.com\" description: \"Error(4003): Cannot create membership \\'user@example.com\\' in \\'groups/xxx\\' because it already exists.\" } } }"
      }
    ]
    ```

* Only
    [Google Groups](https://cloud.google.com/identity/docs/groups#group_properties)
    are supported.

* Last `OWNER` cannot be removed from a Google Group.

## Requirements

These sections describe requirements for using this module.

### Software

The following dependencies must be available:

* [Terraform][terraform] v0.13
* [Terraform Provider for GCP][terraform-provider-gcp] plugin v2.0

### Permissions

A service account or user account needs the following roles to provision the
resources of this module:

#### Google Cloud IAM roles

* Service Usage Consumer: `roles/serviceusage.serviceUsageConsumer` on the
    billing project
* Organization Viewer: `roles/resourcemanager.organizationViewer` if using
    `domain` instead of `customer_id`

The [Project Factory module][project-factory-module] and the
[IAM module][iam-module] may be used in combination to provision a service
account with the necessary roles applied.

#### Google Workspace (formerly known as G Suite) roles

* [Group Admin role](https://support.google.com/a/answer/2405986?hl=en)

To make the service account a Group Admin, you must have Google Workspace Super
Admin access for your domain. Follow
[Assigning an admin role to the service account](https://cloud.google.com/identity/docs/how-to/setup#assigning_an_admin_role_to_the_service_account)
for instructions.

To create groups as an end user, the caller is required to authenticate as a
member of the domain, i.e. you cannot use this module to create a group under
`bar.com` with a `foo.com` user identity.

After the groups have been created, the organization’s Super Admin, Group Admin
or any custom role with Groups privilege can always modify and delete the groups
and their memberships. In addition, the group’s OWNER and MANAGER can edit
membership, and OWNER can delete the group. Documentation around the three group
default roles (OWNER, MANAGER and MEMBER) can be found
[here](https://support.google.com/a/answer/167094?hl=en).

### APIs

A project with the following APIs enabled must be used to host the resources of
this module:

* Cloud Identity API: `cloudidentity.googleapis.com`

The [Project Factory module][project-factory-module] can be used to provision a
project with the necessary APIs enabled.

To use the Cloud Identity Groups API, you must have Google Groups for Business
enabled for your domain and allow end users to create groups.

## Contributing

Refer to the [contribution guidelines](./CONTRIBUTING.md) for information on
contributing to this module.

[iam-module]: https://registry.terraform.io/modules/terraform-google-modules/iam/google
[project-factory-module]: https://registry.terraform.io/modules/terraform-google-modules/project-factory/google
[terraform-provider-gcp]: https://www.terraform.io/docs/providers/google/index.html
[terraform]: https://www.terraform.io/downloads.html
