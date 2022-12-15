# Terraform Module Development Standards

This document serves as a guidepost for effective development with Terraform. Our goal is to provide a unified and consistent approach to managing code by outlining some rules of engagement. These following standards cover basic style and structure for your Terraform module configurations. Portions of this page are modifications based on work created and [shared by Google](https://developers.google.com/readme/policies) and used according to terms described in the [Creative Commons 4.0 Attribution License](https://creativecommons.org/licenses/by/4.0/).

The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

## Terraform Modules

The following guidelines apply to both [Child Modules](#child-modules) and [Root Modules](#root-modules).

Open source modules should be published to the [Terraform Registry](https://registry.terraform.io/).

### Module Structure

* Terraform modules must follow the [standard module structure](https://developer.hashicorp.com/terraform/language/modules/develop#module-structure).
* Every module must start with a `main.tf` file, where resources are located by default.
  * Among other resources, all [locals](https://developer.hashicorp.com/terraform/language/values/locals) should be de ned here in a single locals block.
* All modules must have a `README.md` (with basic documentation in Markdown format).
* Examples should be located in `examples/`, each with its own subdirectory and a `README.md` file.
* Logical groupings of resources can be grouped into their own files and given descriptive names, like `network.tf`, `instances.tf`, or `loadbalancer.tf`.
  * Avoid giving every resource its own file. Resources should be grouped by shared purpose. For example, “metal_project_api_key” and “metal_project_ssh_key” would likely be combined in `project.tf`.
* Only Terraform (`*.tf`) and repo metadata files (like `README.md`, `CHANGELOG.md`, or `kitchen.yml`) should exist at the root directory of a module.
* Additional documentation should be stored in a `docs/` subdirectory.

### Naming Convention

* All configuration objects should be named using underscores to delimit multiple words. This practice ensures consistency with the naming convention for resource types, data source types, and other prede ned values. Note that this convention does not apply to name [arguments](https://developer.hashicorp.com/terraform/language/resources/syntax#resource-arguments).

```hcl
    # Good
    resource "equinix_metal_device" "web_server" { 
      name = “web-server”
      # ...
    }

    # Bad
    resource “equinix_metal_device” “web-server” {
      name = “web-server”
      # … 
    }
```

### Variables

* All variables shall be declared in `variables.tf`.
* Variables shall have descriptive names relevant to their usage or purpose.
  * nputs, local variables, and outputs representing numeric values such as disk sizes or RAM size SHOULD be named with units (like ram_size_gb).
  * For units of storage, the unit pre x (kilo, mega, giga) SHOULD be binary (powers of 1024). For all other units of measurement, the unit pre x SHOULD be decimal (powers of 1000).
  * Boolean variables SHOULD be named with positive values (like enable_external_access) to simplify conditional logic.
* Variables must have descriptions. These are automatically included in any published modules’ auto-generated documentation through [terraform-docs](https://github.com/terraform-docs/terraform-docs). Descriptions add additional context for new developers that descriptive names cannot.
* Variables should have defined types.
* Variables with non-environment-specific values (like disk size) should be given default values.
* Variables for environment-specific values (like `project_id`) should not be given defaults. This forces the calling module to provide meaningful values.
* Variables should only have empty defaults (like empty strings or lists) where leaving the variable empty is a valid preference which will not be rejected by the underlying API(s).
* Be thoughtful in your use of static literals (hardcoded strings, etc.) and parameterize anything which must vary per instance or environment. [Local values](https://developer.hashicorp.com/terraform/language/values/locals) can be used in cases where a literal is reused in multiple places without exposing it as a variable.
* When deciding whether to expose a variable, ensure that you have a concrete use case for changing that variable. Don’t expose variables on the off chance that it’s needed.
  * Adding a variable with a default value is backwards compatible, and thus “cheap”.
  * Removing a variable is backwards incompatible, and thus “expensive”.

### Outputs

* All outputs shall be organized into `outputs.tf`.
* Outputs should have meaningful descriptions.
* Output descriptions should be documented in the `README`. Descriptions should also be auto-generated on commit with [terraform-docs](https://github.com/terraform-docs/terraform-docs).
* Make an effort to output all the useful values root modules would want to reference or share with modules. Particularly for open source or heavily used modules, expose all outputs that have potential for consumption.
* **Outputs** should **not** directly pass through input variables, as this will prevent them from properly adding items to the dependency graph. Instead, outputs should always strive to reference attributes from resources to ensure [implicit dependencies](https://developer.hashicorp.com/terraform/tutorials/configuration-language/dependencies#manage-implicit-dependencies) are created.
  * Instead of referencing an input variable for an instance directly, you would pass through the attribute like this:

  ```hcl
    output "name" {
      description = "Name of instance"
      # NOT THIS:
      # value = var.name
      value	= equinix_metal_device.main.name
    }
  ```

### Data Sources

* [Data sources](https://developer.hashicorp.com/terraform/language/data-sources) are located adjacent to the resources which reference them.
  * If you are fetching an image to be used in launching an instance, you can place it alongside the instance instead of collecting data resources in their own file.
* If the number of data sources grows considerably, it’s a reasonable practice to move these to a dedicated `data.tf` file.
* Data sources should use variable or [resource interpolation](https://developer.hashicorp.com/terraform/language/expressions/strings#interpolation), where appropriate, to fetch data relative to your current environment.

### Scripts (called by Terraform)

* Bespoke scripts can be called by Terraform through provisioners, including the local-exec provisioner.
* Custom scripts should be avoided, if possible, and constrained to instances where native Terraform resources do not support the desired behavior. Any custom scripts used must have a clearly documented reasoning and ideally a deprecation plan. Additionally, custom scripts should not replace configuration management tools in cases where they are more appropriate.
* Bespoke scripts called by Terraform must be organized into `scripts/`.
* Use scripts only when absolutely necessary, as the state of resources created through scripts is not accounted for or managed by Terraform. You’ll likely want to add a policy of `ignore_changes = [*]` on such resources.

### Helper scripts (not called by Terraform)

* Helper scripts should be organized in a `helpers/` directory.
* Helper scripts shall be documented in the `README` with an explanation and example invocations.
* Helper scripts accepting arguments should provide argument-checking and `--help` output.

### Static files

* Static files which are referenced by Terraform (like startup scripts loaded onto Metal devices) but not executed must be organized into `files/`.
* Lengthy heredocs should be externalized from their HCL and into external files. These should be referenced with the `file()` function.

### Templates

* Files which are injected with the Terraform [templatefile function](https://developer.hashicorp.com/terraform/language/functions/templatefile) should be given the file extension `.tftpl`.
* Templates must be placed in `templates/`.

### Resources

* Resources that are the only one of their type (i.e., a single load balancer for an entire module) should be named **‘main’** to simplify references to that resource.
  * It takes extra mental work to remember `some_metal_resource.my_unique_name.id` vs. `some_metal_resource.main.id`.
* Resources that share the same type as others in the same module should be given meaningful names to differentiate them.
* Resources must be named in snake-case (like `db_instance`).
* Resource names should be singular.
* Resource names shouldn’t repeat the resource type within the name. For example: Do this: `resource "equinix_metal_ip_attachment" "main" { ... }` Not this: `resource "equinix_metal_ip_attachment" "main_ip_attachment" { ... }`
* Ensure that [deletion protection](https://developer.hashicorp.com/terraform/language/meta-arguments/lifecycle#prevent_destroy) is enabled for stateful resources like databases. For example:

```hcl
    resource "aws_db_instance" "main" { 
      name = "primary-instance"
      engine = "mysql"

      lifecycle { 
        prevent_destroy = true
      }
    }
```

### Formatting

* All Terraform files must conform to the standards of `terraform fmt`.

### Expressions

* Limit the complexity of any individual interpolated expressions. If many functions are needed in a single expression, consider splitting it out into multiple expressions using [locals](https://developer.hashicorp.com/terraform/language/values/locals).
* Never have more than one ternary operation in a single line. Instead, use multiple local values to build up the logic.
* Be sparing when using user-specified variables to set the `count` variable for resources.
  * If a resource attribute is provided for such a variable (like `project_id`) and that resource does not yet exist, Terraform will not be able to generate a plan and will report the error “value of count cannot be computed.”
* Use count to instantiate a resource conditionally. For example:

```hcl
    variable "readers" { 
      description = "..."
      type        = "list"
      default     = []
    } 
    resource "foo" "bar" {
      // Do not create this resource if the list of readers is empty.
      count = length(var.readers) == 0 ? 0 : 1 ...
    }
```

## Child Modules

Modules that are meant for reuse should follow the following standards, as well as the normal [Terraform guidelines](#terraform-modules).

### Structure

* All child modules should have an `OWNERS` file (or `CODEOWNERS` on GitHub) documenting who is responsible for the module.
* Child modules should follow [SemVer v2.0.0](https://semver.org/spec/v2.0.0.html) when new versions are tagged/released.
* Child Modules [must not declare providers or backends](https://developer.hashicorp.com/terraform/language/modules/develop/providers). Leave that to the root modules.
  * Working examples should codify if a specific provider version is needed for a given module.

### Variables

* It’s a good practice to allow flexibility in the labeling of resources through the module’s interface. Consider providing a `labels` variable with a default value of an empty map to apply throughout labelable resources:

```hcl
    variable "labels" {
      description = "A map of labels to apply to contained resources."
      default     = {}
      type        = "map"
    }
```

### Outputs

* Outputs are required for child modules that define resources.
  * Variables and outputs are used to infer dependencies between modules and resources. Without any outputs, users cannot properly order your module in relation to their Terraform configurations.
  * Every resource defined in a common module should have at least one output which references that resource.

### Inline Modules

* Inline modules may be used to organize complex Terraform modules into smaller units, or de-duplicate common resources.
* Inline modules shall be placed in `modules/$modulename`.
* Inline modules should be treated as private and should not be used by outside modules, unless the common module specifically documents them otherwise.
* Be aware that Terraform doesn’t track refactored resources; if you start out with a number of resources in the top level module and then push them into submodules, Terraform will try to recreate all refactored resources.
* Outputs defined by internal modules are not automatically exposed; if you want to share outputs from internal modules you’ll need to re-output them.

## Root Modules

Root configs, or root modules, are the working directories from which you run the Terraform
CLI. They should follow the following standards, as well as the normal [Terraform guidelines](#terraform-modules) where applicable. Explicit recommendations for root modules supersede the general guidelines.

It is important to keep a single root config from ballooning in size with too many resources being stored in the same directory and state. This is because **all** resources in a particular root config are refreshed every time Terraform is run, which can lead to slow execution time if too many resources are included in a single state. A rule of thumb is that a single state shouldn’t include more than a ~100 resources, and ideally only a few dozen.

Resources for different applications and projects should be separated into their own Terraform directories that **can** be managed independently of each other. A service might represent a particular application or a common service like shared networking. Importantly, all the Terraform code for a particular service should be nested under **one** directory (including subdirectories).

### Directory Structure

There are multiple ways to organize Terraform root configurations, especially when it comes to managing multiple environments. When it comes to managing the Terraform config for a particular service, the recommended structure is to use environment directories.

#### Directories per environment

In this style, each service must split its Terraform config into multiple directories. In this structure, the directory layout must be as follows:

```bash
-- SERVICE-DIRECTORY/
    -- OWNERS
    -- modules/
        -- service/
            -- main.tf
            -- variables.tf
            -- outputs.tf
            -- README
        -- ...other…
    -- environments/
        -- dev/
            -- backend.tf
            -- main.tf
            -- provider.tf
        -- qa/
            -- backend.tf
            -- main.tf
            -- provider.tf
        -- prod/
            -- backend.tf
            -- main.tf
            -- provider.tf
```

#### Environment directories

Each environment directory (`dev`, `qa`, `prod`) within corresponds to a [Terraform Workspace](https://developer.hashicorp.com/terraform/language/state/workspaces) and deploys a version of the service to that environment. This config should reference modules to share code across environments, including typically a service module which includes the base shared Terraform config for the service.

This environment directory must contain the following files:

* A `backend.tf` file declaring the Terraform [backend](https://developer.hashicorp.com/terraform/language/settings/backends/configuration) state location.
* A `main.tf` file which instantiates the service module.
* A `provider.tf` file which declares provider configuration.

#### Workspaces per environment

Alternatively, a single Terraform directory can be used per service and shared across environments. Each environment would have its own [workspace](https://developer.hashicorp.com/terraform/language/state/workspaces).

When using workspaces, all environments share the same modules, and the configuration is driven by a `tfvars` file and a workspace. Workspaces are helpful in that they limit the amount of code that must be copy-pasted between environment directories, which can help enforce parity between environments while maintaining their own state files.

By default a single workspace named **“default”** exists. It is recommended to create and use a workspace for each environment and use the **“default”** workspace only when working with resources that may be used across multiple environments like some service accounts.

Workspaces can be listed with the `workspace` subcommand:

```bash
> terraform workspace list
* default
```

To create a new workspace:

```bash
> terraform workspace new prod
Created and switched to workspace "prod"!

You're now on a new, empty workspace. Workspaces isolate their state,
so if you run "terraform plan" Terraform will not see any existing state
for this configuration.
```

Workspaces isolate their state by creating an additional state file in the state backend for each workspace.

You must select a workspace before issuing terraform commands to that workspace.

```bash
> terraform workspace select prod
Switched to workspace "prod".
```

Once you have switched to a workspace, all terraform commands work in the same manner you are accustomed to. A `tfvars` should be kept for each workspace to encapsulate the inputs for a given environment. `tfvars` can be stored in remote storage like GCS or S3 or encrypted and stored alongside code in a source code management system using KMS or a tool like git-crypt or git-secret.

```bash
> terraform apply -var-file=prod.tfvars
```

The `tfvars` file will drive the configuration of a specific environment. To enable/disable resources per environment, it is common to use the `count` attribute.

Example: prod.tfvars

```hcl
replica_count = 5
```

Example: staging.tfvars

```hcl
replica_count = 1
```

Example: dev.tfvars

```hcl
replica_count = 0
```

Example: main.tf

```hcl
resource "google_sql_database_instance" "primary" {
  name = "${terraform.workspace}-primary" //... 
}

resource "google_sql_database_instance" "replica" { 
  name = "${terraform.workspace}-replica-${count.index}"
  count = "${var.replica_count} //...
}
```

### Outputs

* Information from a root module which other root modules may depend on must be exported as outputs.
* Root module outputs can be referenced using remote state.

Publishing outputs with remote states:

* Make sure to re-output nested module outputs that are useful as remote state.
  * Only root module-level outputs can be referenced from other Terraform environments/applications.
* Information related to a service’s endpoints should be exposed to remote state to allow use by other dependent apps for configuration.

## Versioning

### Terraform

Terraform v0.12 is a significant release that includes some backwards incompatibilities and significant changes with each new version. Once a Terraform config has been upgraded to Terraform 0.12, it should be pinned to the latest release it has been tested with.

```hcl
terraform {
  required_version = "~> 0.12.16"
}
```

### Equinix provider

In Terraform root configurations, you should pin the Equinix provider to a known good **minor** version. This will allow automatic upgrade to new patch releases while still keeping a solid target. Prerelease versions may only be **an exact** version constraint (the `=` operator or no operator). Prerelease versions do not match inexact operators such as `>=`, `~>`, etc.

You should make updating the version pin a regular practice:

```hcl
provider "equinix" {
  source = "equinix/equinix"
  version = "= 1.11.0-alpha.2"
}
```

In shared modules, the provider version should **not** be pinned. Instead, a [version constraint](https://developer.hashicorp.com/terraform/language/providers/requirements#best-practices-for-provider-versions) should be added targeting the minimum working minor version. For example, using `~>1.11` indicates the module will work with any provider version `>= 1.11, < 2.0`.

```hcl
terraform { 
  required_providers { 
    equinix = {
        source = "equinix/equinix"
        version = "~> 1.11"
    }
  }
}
```

Note, example configurations within modules (ex. in `examples/` or in test configuration) are considered root configurations and can be pinned to a minor version.

### Modules

References to shared modules must be [constrained](https://developer.hashicorp.com/terraform/language/modules/syntax#version) to a release tag. Targeting a specific commit hash or branch is dangerous as it gives no context to the version of the underlying module. Updating modules should involve as little guesswork as possible for both authors and reviewers.

### Constrain by git reference

References to shared modules may be constrained to any arbitrary git reference (commit, branch, or tag). For reasons outlined above, we only recommend using this to reference tags:

```hcl
module "vpc" { 
    source = "git::https://github.com/terraform-aws-modules/terraform-aws-vpc?ref=v3.18.0"
    ...
}
```

### Constrain by version

When a git tag is released to the Terraform Module Registry, it creates a numbered version of that module (note that this does not apply to Github repositories, only to modules released to registries). An invocation can be constrained to said version:

```hcl
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  version = "3.18.1"
  ...
}
```
