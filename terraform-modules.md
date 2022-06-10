# Terraform Equinix Modules

Ths guide discusses how to contribute Terraform modules to the Equinix Labs GH Org.

Terraform modules should either be sourced from <https://github.com/equinix-labs/terraform-equinix-template>
or closely following the format of this project.


## Using the Terraform Equinix Modules Template

1. Join the Equinix Labs GitHub organization with your Equinix Single Sign-On:
  * https://github.com/orgs/equinix-labs/sso
    * You may need to the verify networking settings
    * You may need to Enable Two Factor on your GitHub Profile
    * You may need to Enable SSO on your SSH key: https://github.com/settings/keys
  * Make your membership public, if you like:
    * https://github.com/orgs/equinix-labs/people

2. Choose a name for your repository
  * Module names must be of the form “terraform-{provider}-{something-concise}”. This ensures that the module can eventually be published. All Terraform configurations we share should strive to become a published, reusable, module.
  * Names should begin with “terraform-equinix” for any project with Equinix or Equinix Metal providers, or “terraform-metal” when only the Equinix Metal provider is used. “terraform-equinix-foo” is a good prefix for any module.

3. Visit the Template module project:
  * <https://github.com/equinix-labs/terraform-equinix-template>
  * Click “Use this template”
  * Use Equinix SSO, if prompted
  * Choose the Equinix Labs org (if you do not see this, make sure you joined the org in step 1)
  * Give the repository the name determined in the previous step.
  * Set a meaningful description, for example “Terraform module for Equinix Things to do something desirable”
  * Click the box to make it public
  * Click “Create repository from template”

4. Give the “terraform” group admin access to the repository
  * Click the “Settings” tab
  * “Manage Access”
  * “Invite Teams or People”
  * “equinix-labs/terraform”
  * Give the team “admin” access

5. Develop your module
  * Look for “template” within the project and replace it with the appropriate repo name or advised content.
  * Keep the module standards in mind (https://www.terraform.io/docs/language/modules/)
  * Use “terraform fmt” in your root and submodules to keep your project clean
  * GitHub actions should be preconfigured to verify that your PRs contain valid Terraform
  * In your README.md and examples, avoid using any credentials that look like real credentials.  Avoid IP addresses that are real public IP addresses. These credentials will be flagged and reviewed by the security team and will waste the time of everyone involved (I am writing this after receiving a phone call about a flagged phony credential :-) ).
  * When working on changes, Branch Protection may be enabled which will prevent pushing directly to the `main` branch. Use branches and open PRs against those branches. With the approval of any other member of the "equinix-labs/terraform" team, your PR may be merged. It is also possible to disable branch protection.

6. When your code has reached any level of stability (“it does what it says it does”), tag it.
  * Use the “Tags” tab, then “Releases”
  * Create a semantic version tag (ex. v0.1.0)
Indicate any new features between tags and use the keepachangelog.com format within the summary:

    ```
    ### Added
    - new variable “foo”
    - new output “bar”
    ### Changed
    - metal_device.foo no longer installs neovim
    ````
  * Use a leading major version of “v0.” This indicates that there are no stability guarantees between releases. You can also Indicate that this version is a “Pre-release” using the GitHub checkbox. On subsequent releases, increment the minor version whenever a feature is added (“0.1.0” -> “0.2.0”). Increment the patch level when bug fixes are made that don’t affect the user interface (variables and outputs) (“0.1.0” -> “0.1.1”).

7. Publish the module
  * <https://registry.terraform.io/github/create>
  * See https://www.terraform.io/docs/registry/modules/publish.html for more details
  * Any new releases will automatically be picked up
  * You may update your README.md to reflect that the variables and outputs are defined in the registry documentation. You may also want to change your “git clone” instructions in favor of the “module “foo” {}” pattern. 
  * You should set the Terraform Registry URL in the repositories Settings / URL field. For example, https://registry.terraform.io/modules/equinix-labs/hybrid-gateway

## Reviewing Equinix Terraform Modules

1. Join the Equinix Labs GitHub organization with your Equinix Single Sign-On:
  * https://github.com/orgs/equinix-labs/sso
    * You may need to the verify networking settings
    * You may need to Enable SSO on your SSH key: https://github.com/settings/keys
  * Make your membership public, if you like:
    * https://github.com/orgs/equinix-labs/people
  * If the organization is not “equinix-labs”, “equinix”, etc, you may need to authorize the org first: https://registry.terraform.io/publish/provider
2. Review the guidelines offered above (Steps 5-7)
3. Join the Terraform team: [https://github.com/orgs/equinix-labs/teams/terraform/members](https://github.com/orgs/equinix-labs/teams/terraform/members)
  * Let an existing member of the team know that you’ve requested access so that they can approve your request.
4. Review and Merge PRs

