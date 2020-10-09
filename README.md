# Uniform Standards for External Facing Repositories

![](https://img.shields.io/badge/stability-maintained-green.svg) [![Slack](https://slack.packet.com/badge.svg)](https://slack.packet.com) [![Twitter Follow](https://img.shields.io/twitter/follow/packethost.svg?style=social&label=Follow)](https://twitter.com/intent/follow?screen_name=packethost)

This repository is [Maintained](https://github.com/packethost/standards/blob/master/maintained-statement.md)!

If you require support, please email [support@packet.com](mailto:support@packet.com), visit the Equinix Metal IRC channel (#equinixmetal on freenode), subscribe to the [Packet Community Slack channel](https://slack.packet.com) or post an issue within this repository.

[Contributions](https://github.com/packethost/standards/blob/master/CONTRIBUTING.md) are welcome to help extend this work!

## About Uniform Standards

Packet maintains a number of public repositories that help customers to run various workloads on Packet. These repositories are in various states of completeness and quality, and being public, developers often find them and start using them. This creates problems:

* Developers using low-quality repositories may infer that Packet generally provides a low quality experience.
* Many of our repositories are put online with no formal communication with, or training for, customer success. This leads to a below average support experience when things do go wrong.
* We spend a huge amount of time supporting users through various channels when with better upfront planning, documentation and testing much of this support work could be eliminated.

To that end, we propose four tiers of repositories: [Private](https://github.com/packethost/standards#private-tier-minimum-requirements), [End of Life](https://github.com/packethost/standards#end-of-life-tier-minimum-requirements), [Experimental](https://github.com/packethost/standards#experimental-tier-minimum-requirements), and [Maintained](https://github.com/packethost/standards#maintained-tier-minimum-requirements).

### The Goal

Our repositories should be the example from which adjacent, competing, projects look for inspiration.

Each repository should not look entirely different from other repositories in the ecosystem, having a different layout, a different testing model, or a different logging model, for example, without reason or recommendation from the subject matter experts from the community.

We should share our improvements with each ecosystem while seeking and respecting the feedback of these communities.

Whether or not strict guidelines have been provided for the project type, our repositories should ensure that the same components are offered across the board. How these components are provided may vary, based on the conventions of the project type. GitHub provides general guidance on this which they have integrated into their user experience.

## The Requirements

Each tier, Private, Experimental, and Maintained has its own list of requirements that mostly builds on the tier before. Feel free to copy / paste from one of the issue templates provided in [the process](#the-process) below to create a new issue in the repository. If anything's missing, file an issue on this repository.

### Private Tier

These repositories are not public at all, therefore, there are no minimum requirements. If you want to go public with that repository, you might want to think about the next level requirements.

### End of Life Tier

We no longer support nor maintain these projects. Our end of life project requirements are outlined in [the process](#the-process) below.

### Experimental Tier

Use at your own risk and do not expect thorough support!  Our experimental project requirements are outlined in [the process](#the-process) below.

### Maintained Tier

We stand behind these repositories and support using them in production! Our maintained project requirements are outlined in [the process](#the-process) below.

## The Process

1. A repository is reviewed by Packet and filed as Maintained, Experimental, EndOfLife or Private.
2. Maintained repositories are moved to <https://github.com/packethost>. Experimental repositories are moved to <https://github.com/packet-labs>. End of Life repositories may remain where they are archived.
3. An issue is filed on the repository using the [maintained issue template](https://raw.githubusercontent.com/packethost/standards/master/ISSUE_TEMPLATE/maintained-issue.md) or the [Experimental Issue template](https://raw.githubusercontent.com/packethost/standards/master/ISSUE_TEMPLATE/experimental-issue.md)  or the [End of Life Issue template](https://raw.githubusercontent.com/packethost/standards/master/ISSUE_TEMPLATE/end-of-life-issue.md) which creates a checklist directly on the repository.
4. Work is completed via pull requests.
5. You can track the progress with us of the [PacketHost repositories](https://github.com/orgs/packethost/projects/4) and the progress for the [Packet-Labs repositories](https://github.com/orgs/packet-labs/projects/1) via automated kanban.
