# Uniform Standards for External Facing Repositories

## Why Are We Here?
Packet maintains a number of public repositories that help customers to run various workloads on Packet. These repositories are in various states of completeness and quality, and being public, developers often find them and start using them. This creates problems:

* Developers using low-quality repositories may infer that Packet generally provides a low quality experience.
* Many of our repositories are put online with no formal communication with, or training for, customer success. This leads to a below average support experience when things do go wrong.
* We spend a huge amount of time supporting users through various channels when with better upfront planning, documentation and testing much of this support work could be eliminated.

To that end, we propose three tiers of repositories: Private, Experimental, and Maintained.

## The Goal
Our repositories should be the example from which adjacent, competing, projects look for inspiration.

Each repository should not look entirely different from other repositories in the ecosystem, having a different layout, a different testing model, or a different logging model, for example, without reason or recommendation from the subject matter experts from the community.

We should share our improvements with each ecosystem while seeking and respecting the feedback of these communities.

Whether or not strict guidelines have been provided for the project type, our repositories should ensure that the same components are offered across the board. How these components are provided may vary, based on the conventions of the project type. GitHub provides general guidance on this which they have integrated into their user experience.

### Private Tier Minimum Requirements
* [ ] The Private badge

### Experimental Tier Minimum Requirements
* [ ] The Experimental badge
* [ ] The Experimental Statement in the README.md
* [ ] Getting Started Tutorial
* [ ] README.md
* [ ] LICENSE
* [ ] CI/CD
* [ ] OWNERS.md
* [ ] Developer Certificate of Origin https://github.com/apps/dco
* [ ] At least one maintainer

### Maintained Tier Minimum Requirements
* [ ] The Maintained badge
* [ ] The Maintained Statement in the README.md
* [ ] Getting Started Tutorial
* [ ] README.md
* [ ] LICENSE
* [ ] CI/CD
* [ ] OWNERS.md
* [ ] Developer Certificate of Origin https://github.com/apps/dco
* [ ] At least two maintainers
* [ ] A Manifest File
* [ ] Code of Conduct
* [ ] How to Contribute
* [ ] SUPPORT.md
* [ ] RELEASE.md
