---
subcategory: "Cloud Platform"
layout: "google"
page_title: "Google: google_project_default_service_accounts"
sidebar_current: "docs-google-project-default-service-accounts-x"
description: |-
  Allows management of Google Cloud Platform project default service accounts.
---

# google_project_default_service_accounts

Allows management of Google Cloud Platform project default service accounts.

When certain service APIs are enabled, Google Cloud Platform automatically creates service accounts to help get started, but
this is not recommended for production environments as per [Google's documentation](https://cloud.google.com/iam/docs/service-accounts#default).
See the [Organization documentation](https://cloud.google.com/resource-manager/docs/quickstarts) for more details.
~> This resource works on a best-effort basis, as no API formally describes the default service accounts. If the default service accounts change their name or additional service accounts are added, this resource will need to be updated.

## Example Usage

```hcl
resource "google_project_default_service_accounts" "my_project" {
  project = "my-project-id"
  action = "DELETE"
}
```

To enable the default service accounts on the resource destroy:

```hcl
resource "google_project_default_service_accounts" "my_project" {
  project = "my-project-id"
  action = "DISABLE"
  restore_policy = "REVERT"
}

```

## Argument Reference

The following arguments are supported:

- `project` - (Required) The project ID where service accounts are created.

- `action` - (Required) The action to be performed in the default service accounts. Valid values are: `DEPRIVILEGE`, `DELETE`, `DISABLE`. Note that `DEPRIVILEGE` action will ignore the REVERT configuration in the restore_policy

- `restore_policy` - (Optional) The action to be performed in the default service accounts on the resource destroy. Valid values are `NONE` and `REVERT`. If set to `REVERT` it will attempt to restore all default SAs but in the `DEPRIVILEGE` action.

## Attributes Reference

In addition to the arguments listed above, the following computed attributes are
exported:

- `id` - an identifier for the resource with format `projects/{{project}}`
- `service_accounts` - The Service Accounts changed by this resource. It is used for `REVERT` the `action` on the destroy.

## Timeouts

This resource provides the following
[Timeouts](/docs/configuration/resources.html#timeouts) configuration options:

- `create` - Default is 10 minutes.
- `update` - Default is 10 minutes.
- `delete` - Default is 10 minutes.

## Import

This resource does not support import
