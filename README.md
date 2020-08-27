# Uniform Standards for External Facing Repositories
![](https://img.shields.io/badge/stability-maintained-green.svg)

This repository is [Maintained](https://github.com/packethost/standards/blob/master/maintained-statement.md)!

If you require support, please email [support@packet.com](mailto:support@packet.com), visit the Packet IRC channel (#packethost on freenode), subscribe to the [Packet Community Slack channel](https://slack.packet.com) or post an issue within this repository.

[Contributions](https://github.com/packethost/standards/blob/master/CONTRIBUTING.md) are welcome to help extend this work!

## About https://github.com/packethost/standards
Packet maintains a number of public repositories that help customers to run various workloads on Packet. These repositories are in various states of completeness and quality, and being public, developers often find them and start using them. This creates problems:

* Developers using low-quality repositories may infer that Packet generally provides a low quality experience.
* Many of our repositories are put online with no formal communication with, or training for, customer success. This leads to a below average support experience when things do go wrong.
* We spend a huge amount of time supporting users through various channels when with better upfront planning, documentation and testing much of this support work could be eliminated.

To that end, we propose three tiers of repositories: [Private](https://github.com/packethost/standards#private-tier-minimum-requirements), [Experimental](https://github.com/packethost/standards#experimental-tier-minimum-requirements), and [Maintained](https://github.com/packethost/standards#maintained-tier-minimum-requirements).

### The Goal
Our repositories should be the example from which adjacent, competing, projects look for inspiration.

Each repository should not look entirely different from other repositories in the ecosystem, having a different layout, a different testing model, or a different logging model, for example, without reason or recommendation from the subject matter experts from the community.

We should share our improvements with each ecosystem while seeking and respecting the feedback of these communities.

Whether or not strict guidelines have been provided for the project type, our repositories should ensure that the same components are offered across the board. How these components are provided may vary, based on the conventions of the project type. GitHub provides general guidance on this which they have integrated into their user experience.

## The Requirements
Each tier, Private, Experimental, and Maintained has its own list of requirements that mostly builds on the tier before. Feel free to copy / paste the pertinent section as an issue in the relevant repository. If anything's missing, file an issue on this repository.

### Private Tier Minimum Requirements
Not public at all, therefore, no minimum requirements, but if you want to go public with that repository, you might want to think about the next level requirements.
```
* [ ] Flagged Private
```

### Experimental Tier Minimum Requirements
Use at your own risk and do not expect thorough support!
```
* [ ] Flagged Public
* [ ] [The Experimental badge](https://github.com/packethost/standards/blob/master/glossary.md#experimental-badge)
* [ ] [The Experimental Statement in the README.md](https://github.com/packethost/standards/blob/master/glossary.md#experimental-statement)
* [ ] [Getting Started Tutorial](https://github.com/packethost/standards/blob/master/glossary.md#getting-started-tutorial)
* [ ] [README.md](https://github.com/packethost/standards/blob/master/glossary.md#readmemd)
* [ ] [LICENSE](https://github.com/packethost/standards/blob/master/glossary.md#license)
* [ ] [CI/CD](https://github.com/packethost/standards/blob/master/glossary.md#cicd)
* [ ] [OWNERS.md](https://github.com/packethost/standards/blob/master/glossary.md#ownersmd)
* [ ] [Developer Certificate of Origin](https://github.com/packethost/standards/blob/master/glossary.md#developer-certificate-of-origin)
* [ ] At least one [maintainer](https://github.com/packethost/standards/blob/master/glossary.md#maintainer)
```

### Maintained Tier Minimum Requirements
We stand behind these repositories and support using them in production!
```
* [ ] Flagged Public
* [ ] [The Maintained badge](https://github.com/packethost/standards/blob/master/glossary.md#maintained-badge)
* [ ] [The Maintained Statement in the README.md](https://github.com/packethost/standards/blob/master/glossary.md#maintained-statement)
* [ ] [Getting Started Tutorial](https://github.com/packethost/standards/blob/master/glossary.md#getting-started-tutorial)
* [ ] [README.md](https://github.com/packethost/standards/blob/master/glossary.md#readmemd)
* [ ] [LICENSE](https://github.com/packethost/standards/blob/master/glossary.md#license)
* [ ] [CI/CD](https://github.com/packethost/standards/blob/master/glossary.md#cicd)
* [ ] [OWNERS.md](https://github.com/packethost/standards/blob/master/glossary.md#ownersmd)
* [ ] [Developer Certificate of Origin](https://github.com/packethost/standards/blob/master/glossary.md#developer-certificate-of-origin)
* [ ] At least two [maintainers](https://github.com/packethost/standards/blob/master/glossary.md#maintainer)
* [ ] [A Manifest File](https://github.com/packethost/standards/blob/master/glossary.md#manifest-file)
* [ ] [Code of Conduct](https://github.com/packethost/standards/blob/master/glossary.md#code-of-conduct)
* [ ] [How to Contribute](https://github.com/packethost/standards/blob/master/glossary.md#contributors-guide)
* [ ] [SUPPORT.md](https://github.com/packethost/standards/blob/master/glossary.md#supportmd)
* [ ] [RELEASE.md](https://github.com/packethost/standards/blob/master/glossary.md#releasemd)
```
