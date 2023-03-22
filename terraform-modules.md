# Terraform Equinix Modules

Ths guide discusses how to contribute Terraform modules to the Equinix Labs GH Org.

Terraform modules should either be sourced from [Template module template] or closely following the format of this project.

## Using the Terraform Equinix Modules Template

- [Join the Equinix Labs GitHub org](#join)
- [Choose a name for your repository](#choose)
- [Visit the Template module template](#visit)
- [Give the “terraform” group admin access to the repository](#give)
- [Develop your module](#develop)
- [Tag your module](#tag)
- [Publish your module](#publish)

Steps:

1. <a name="join"></a> Join the Equinix Labs GitHub organization with your Equinix Single Sign-On:

* [Equinix SSO]
  * You may need to the verify networking settings
  * You may need to Enable Two Factor on your GitHub Profile
  * You may need to Enable SSO on your [SSH key]
* Make your membership public, if you like:
  * [Equinix-Labs Org Members]

2. <a name="choose"></a> Choose a name for your repository

* Module names must be of the form “terraform-{provider}-{something-concise}”. This ensures that the module can eventually be published. All Terraform configurations we share should strive to become a published, reusable, module.
* Names should begin with “terraform-equinix” for any project with Equinix or Equinix Metal providers, or “terraform-metal” when only the Equinix Metal provider is used. “terraform-equinix-foo” is a good prefix for any module.

3. <a name="visit"></a> Visit the [Template module template]:

* Click “Use this template”
* Use Equinix SSO, if prompted
* Choose the Equinix Labs org (if you do not see this, make sure you joined the org in step 1)
* Give the repository the name determined in the previous step.
* Set a meaningful description, for example “Terraform module for Equinix Things to do something desirable”
* Click the box to make it public
* Click “Create repository from template”

4. <a name="give"></a> Give the “terraform” group admin access to the repository

* Click the “Settings” tab
* “Manage Access”
* “Invite Teams or People”
* “equinix-labs/terraform”
* Give the team “admin” access

5. <a name="develop"></a> Develop your module

* Look for “template” within the project and replace it with the appropriate repo name or advised content.
* Follow the standards provided in the [Terraform Module Development Standards] guide.
* GitHub `run-pre-commit-hooks` action should be preconfigured to verify that your PRs contain valid Terraform
* In your README.md and examples, avoid using any credentials that look like real credentials.  Avoid IP addresses that are real public IP addresses. These credentials will be flagged and reviewed by the security team and will waste the time of everyone involved (I am writing this after receiving a phone call about a flagged phony credential :-) ).
* When working on changes, Branch Protection may be enabled which will prevent pushing directly to the `main` branch. Use branches and open PRs against those branches. With the approval of any other member of the "equinix-labs/terraform" team, your PR may be merged. It is also possible to disable branch protection.
* Follow the [Conventional Commit Specification] to properly format and submit commits. Other types are allowed, based on the [Angular Convention types]. Example commit message: `fix: disabled log generation for system services`. For more examples, see [commit examples].
* Submit a PR when ready. The following [Angular Convention types] are supported in the PR title: fix, feat, docs, ci, and chore. Add the `[WIP]` prefix to indicate that the PR represents "work in progress". PR title must start with an upper case character after the colon `:`. Example PR Title: `fix: Disabled log generation for system services`

6. <a name="tag"></a> When your code has reached any level of stability (“it does what it says it does”), submit a PR following the format highlighted in the previous section 5. When you submit a PR, it should trigger two github workflow CI jobs: `run-pre-commit-hooks` and `validate-pr-title`. Once it is approved and merged, it will then trigger yet another two gihub workflow CI jobs: `generate-terraform-docs` and `generate-release`.

* The `run-pre-commit-hooks` github CI job will run checks on terraform, md, bash shell, python, yaml, and json files.
* The `validate-pr-title` github CI job will validate the title of the PR. PR title must start with an upper case character after the colon `:`. It must contain one of the keywords (i.e feat, fix, docs, etc) stated in the [Conventional Commit Specification].
* The `generate-terraform-docs` github CI job will populate the main README.md, the modules README.md and the examples README.md file.
* The `generate-release` github CI job relies on the [semantic release action]
* This action is configured with 2 major plugins: `commit-analyzer` and `release-notes-generator`
  - [commit-analyzer] - This will examine the commits and look for keywords in the commit message, based on the [Conventional Commit Specification], then update github release with the corresponding [semantic change]
  - [release-notes-generator] - This will update the CHANGELOG.md file with the relative info about each release and changes within.
* This is the reference [semantic change] table based on commit type:

  | Type | Description | Semantic Increment |
  |------|-------------|--------------------|
  | feat | Features | minor |
  | fix | Bug Fixes | patch |
  | perf | Performance Improvements | patch |
  | revert | Reverts | patch |
  | breaking | Breaking Changes | major |
  | docs | Documentation | N/A |
  | style | Styles | N/A |
  | refactor | Code Refactoring | N/A |
  | build | Builds | N/A |
  | ci | Continuous Integrations | N/A |
  | chore | Chores | N/A |

7. <a name="publish"></a> Publish the module

* Follow this link: [Publish a Terraform module]
* See [HashiCorp Publishing Modules article] for more details
* Any new releases will automatically be picked up
* You may update your README.md to reflect that the variables and outputs are defined in the registry documentation. You may also want to change your “git clone” instructions in favor of the “module “foo” {}” pattern.
* You should set the Terraform Registry URL in the repositories Settings / URL field. For example, `https://registry.terraform.io/modules/equinix-labs/hybrid-gateway`

## Reviewing Equinix Terraform Modules

- [Join the Equinix Labs GitHub org](#joinequinix)
- [Review steps 5-7 above](#develop)
- [Join Equinix Terraform team](#jointeam)
- [Review then merge PR](#reviewpr)

Steps:

1. <a name="joinequinix"></a>Join the Equinix Labs GitHub organization with your Equinix Single Sign-On:

* [Equinix SSO]
  * You may need to the verify networking settings
  * You may need to Enable SSO on your [SSH key]
* Make your membership public, if you like:
  * [Equinix-Labs Org Members]
* If the organization is not “equinix-labs”, “equinix”, etc, you may need to authorize the org first: [Select an Organization]

2. <a name="develop"></a> Review the guidelines offered above (Steps 5-7)

3. <a name="jointeam"></a> Join the [Equinis-labs Terraform team]

* Let an existing member of the team know that you’ve requested access so that they can approve your request.

4. <a name="reviewpr"></a> Review then merge PRs

[Terraform Module Development Standards]: terraform-module-standards.md
[SSH key]: https://github.com/settings/keys
[Template module template]: https://github.com/equinix-labs/terraform-equinix-template
[Equinix SSO]: https://github.com/orgs/equinix-labs/sso
[Equinix-Labs Org Members]: https://github.com/orgs/equinix-labs/people
[Publish a Terraform module]: https://registry.terraform.io/github/create
[HashiCorp Publishing Modules article]: https://developer.hashicorp.com/terraform/registry/modules/publish
[Equinis-labs Terraform team]: https://github.com/orgs/equinix-labs/teams/terraform/members
[Select an Organization]: https://registry.terraform.io/publish/provider
[Conventional Commit Specification]: https://www.conventionalcommits.org/en/v1.0.0/#specification
[Angular Convention types]: https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#type
[commit examples]: https://www.conventionalcommits.org/en/v1.0.0/#examples
[semantic release action]: https://github.com/marketplace/actions/action-for-semantic-release
[semantic change]: https://semver.org/#summary
[commit-analyzer]: https://github.com/semantic-release/commit-analyzer
[release-notes-generator]: https://github.com/semantic-release/release-notes-generator