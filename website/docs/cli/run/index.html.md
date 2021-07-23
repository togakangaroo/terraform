---
layout: "docs"
page_title: "Provisioning Infrastructure - Terraform CLI"
description: "Learn about core provisioning tasks: `plan`, `apply`, and `destroy`. Terraform uses them to create, modify, and destroy infrastructure to match a configuration."
---

# Provisioning Infrastructure with Terraform

Terraform's primary function is to create, modify, and destroy infrastructure
resources to match the desired state described in a
[Terraform configuration](/docs/language/index.html).

When people refer to "running Terraform," they generally mean performing these
provisioning actions in order to affect real infrastructure objects. The
Terraform binary has many other subcommands for a wide variety of administrative
actions, but these basic provisioning tasks are the core of Terraform.

Terraform's provisioning workflow relies on three commands: `plan`, `apply`, and
`destroy`. All of these commands require an
[initialized](/docs/cli/init/index.html) working directory, and all of them act
only upon the currently selected [workspace](/docs/cli/workspaces/index.html).

## Planning

The `terraform plan` command evaluates a Terraform configuration to determine
the desired state of all the resources it declares, then compares that desired
state to the real infrastructure objects being managed with the current working
directory and workspace. It uses state data to determine which real objects
correspond to which declared resources, and checks the current state of each
resource using the relevant infrastructure provider's API.

Once it has determined the difference between the current state and the desired
state, `terraform plan` presents a description of the changes necessary to
achieve the desired state. It _does not_ perform any actual changes to real
world infrastructure objects; it only presents a plan for making changes.

Plans are usually run to validate configuration changes and confirm that the
resulting actions are as expected. However, `terraform plan` can also save its
plan as a runnable artifact, which `terraform apply` can use to carry out those
exact changes.

For details, see [the `terraform plan` command](/docs/cli/commands/plan.html).

## Applying

The `terraform apply` command performs a plan just like `terraform plan` does,
but then actually carries out the planned changes to each resource using the
relevant infrastructure provider's API. It asks for confirmation from the user
before making any changes, unless it was explicitly told to skip approval.

By default, `terraform apply` performs a fresh plan right before applying
changes, and displays the plan to the user when asking for confirmation.
However, it can also accept a plan file produced by `terraform plan` in lieu of
running a new plan. You can use this to reliably perform an exact set of
pre-approved changes, even if the configuration or the state of the real
infrastructure has changed in the minutes since the original plan was created.

For details, see [the `terraform apply` command](/docs/cli/commands/apply.html).

## Destroying

The `terraform destroy` command destroys all of the resources being managed by
the current working directory and workspace, using state data to determine which
real world objects correspond to managed resources. Like `terraform apply`, it
asks for confirmation before proceeding.

A destroy behaves exactly like deleting every resource from the configuration
and then running an apply, except that it doesn't require editing the
configuration. This is more convenient if you intend to provision similar
resources at a later date.

For details, see [the `terraform destroy` command](/docs/cli/commands/destroy.html).