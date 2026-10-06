+++
date = "2026-10-06T12:03:43+02:00"
draft = false
title = "Can I Buy One of Them Zero Trust?"
description = "Can I Buy One of Them Zero Trust?"
categories = ["Infosec"]
tags = ["Zero Trust"]
+++
<div style="text-align: justify">

Back in the day when I was leading a team of security engineers, one of our clients asked us if we could work on a Zero Trust project. The client's request was clear, they wanted to _"buy one of them Zero Trust"_. I guess their VP wanted to toss that term around during their next dinner with their _muy importante people._ Now, this was years ago and I had no clue what Zero Trust was, so this post is about what I wish I'd known back then.

## ZT, ZTA, ZTNA, ZTMM: What's the Difference?

Now, that's a lot of zeroes and trusts in their names: ZTA, ZTNA, ZTMM, **ZzZzZz…** But are they all _same-same-but-different_?

### Zero Trust (ZT)

These are _the tables of the law_ representing the cybersecurity covenant. Unlike what the Russians once said, Zero Trust is all about never trust, always verify. The paradigm shifts from trusted/untrusted zones, where once an identity is inside the trusted zone no further verification is needed, to the continuous evaluation and validation of every access.

We move from protecting network segments to protecting resources. In ZT, every access request is evaluated based on identity, device, resource, context, and policy. Being authenticated doesn't necessarily mean being trusted. There's no implicit trust inheritance either, and three principles are always kept in mind:

- **Verify explicitly:** Authenticate and evaluate the context of each access request.
- **Least privilege:** Grant only the minimum access required.
- **Assume breach:** Design to limit the impact if something is compromised.

### Zero Trust Architecture (ZTA)

ZT holds the philosophy on the stone tablets. ZTA puts it into action by incorporating identity-focused controls (IAM, MFA, PAM), network microsegmentation, ongoing monitoring, dynamic policy enforcement, and all the other fancy terms. In ZTA, every device, user, and application is guilty until proven innocent, I mean, secure. And once the identity is trusted, we have _Big Brother_ continuously monitoring that session. [NIST Special Publication 800-207](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-207.pdf) outlines the 7 tenets of ZTA:

1. All data sources and computing services are considered resources.
2. All communication is secured regardless of network location.
3. Access to individual enterprise resources is granted on a per-session basis.
4. Access to resources is determined by dynamic policy.
5. The enterprise monitors and measures the integrity and security posture of all owned and associated assets.
6. All resource authentication and authorization are dynamic and strictly enforced before access is allowed.
7. The enterprise collects as much information as possible about the current state of assets, network infrastructure, and communications and uses it to improve its security posture.

### Zero Trust Network Access (ZTNA)

ZTNA applies the rules and regulations of ZT to **_user-to-application_** connections. Hold on, applications, you said? Why is it called **Network Access** then? Well, _ain't got no clue…_ What I do know is that ZTNA focuses on remote users who want to access applications. Basically, ZTNA _"replaces"_ the traditional VPN model, whereby an identity, once authorized, is considered part of the network and therefore has access to its resources. This can provide broad network-level connectivity, letting people snoop around for information.

ZTNA verifies identity, device posture (OS and application patch levels, antivirus version, EDR), context (MFA authentication, behavioral anomalies and access patterns) and environment (geographic location and time) continuously, not only at the beginning of the session. If those parameters don't meet the minimum established by the policy (e.g., reaching the minimum score needed), access is not granted. This reduces the attack surface by providing a much more granular way of controlling access to specific resources.

## Zero Trust Maturity Model (ZTMM)

Alright, so we know that we shouldn't trust anything, and everything should be monitored continuously to ensure trust is maintained based on what _"the law"_ says. But… how do we _"buy one of em Zero Trust"_? Introducing [CISA's Zero Trust Maturity Model](https://www.cisa.gov/sites/default/files/2023-04/zero_trust_maturity_model_v2_508.pdf).

As one can imagine, ZT is not a product that can be purchased off the shelf. That would be too easy and boring. ZT should be treated as a transformation of the way an organization designs access and trust across its infrastructure. This involves collaboration from the IAM team, network and infrastructure security, application and data security, IT, Security Operations, leadership, and everyone's mother. Fear not, CISA provides a path to implementing ZT in an organization based on five pillars.

### Zero Trust Pillars

1. **Identity:** Things like IAM, PAM, EntraID.
2. **Devices:** Posture and compliance.
3. **Networks:** Microsegmentation, ZTNA, Encyption. 
4. **Applications and Workloads:** Secured Software Development Lifecyle, App access, Security testing.
5. **Data:** Inventory, Classification, Encryption, DLP.

CISA's pillars provide areas where improvements can be made over time, aiming for a progressive transition. Each pillar contains ideas on how to implement changes while keeping in mind Visibility and Analytics, Automation and Orchestration, and Governance. But that is a topic for another day. 

So, where can I buy one of 'em Zero Trust? 
</div>