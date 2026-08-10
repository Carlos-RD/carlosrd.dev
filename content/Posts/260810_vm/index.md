+++
date = "2026-08-10T13:07:55+02:00"
draft = false
title = "Stumbling on Vulnerability Management"
description = "Stumbling on Vulnerability Management"
categories = ["infosec"]
tags = ["Vulnerability Management"]
+++
<div style="text-align: justify">
In the last few days, I've found myself in the recruitment process for a Vulnerability Management position (how did I end up here?), and I'd like to share with you some of the quick and dirty notes I used to prepare for certain parts of the interviews. Notes that helped me understand Vulnerability Management in a holistic way.

## Vulnerability Management Concepts

[NIST Glossary](https://csrc.nist.gov/glossary) for further reference.

- **Attack Surface:** Total number of attack vectors (AVs) an actor can use to access a system.
- **Risk Gap:** Time elapsed between the discovery and remediation of a vulnerability.
- **CVE:** Common Vulnerabilities and Exposures. A system that provides a standard way of naming discovered vulnerabilities, formatted as CVE-\<Year>-\<Sequence Number>.
- **CVSS:** Common Vulnerability Scoring System. Calculates the severity of a vulnerability.

The lifecycle looks something like this: a researcher discovers a vulnerability and submits it to [CVE](https://www.cve.org/) → [FIRST](https://www.first.org/cvss/) assigns a CVSS score to the submitted CVE, and both are fed into the → [National Vulnerability Database](https://nvd.nist.gov/vuln/search#/nvd/home?resultType=records) → Vulnerability scanners _partially_ use this database to assign severity ratings. Let's look a bit deeper into the CVSS scoring system.

### CVSS Exploitability Metrics

The vector string [explains](https://www.first.org/cvss/v3.1/specification-document) the score given and how difficult the system is to exploit, e.g.: `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H`

- **CVSS version number:** In this case, 3.1, with the latest being 4.0 at the time of writing.
- **Attack Vector (AV):** Ways to gain access to a system: social engineering, phishing, vulnerability exploitation, malware, insider actors, etc. Essentially, the *remoteness* of the attacker: can the vulnerability be exploited locally or remotely? _(N = Network, A = Adjacent, L = Local, P = Physical)_
- **Attack Complexity (AC):** How difficult is the vulnerability to exploit? _(L = Low, H = High)_
- **Privileges Required (PR):** Level of access needed to exploit. _(N = None, L = Low, H = High)_
- **User Interaction (UI):** Does it need user assistance to exploit? _(N = None, R = Required)_
- **Scope (S):** Ability to affect resources beyond the defined privileges (access to other data or resources). _(U = Unchanged, C = Changed)_
- **Impact (C, I, A):** Effects on confidentiality, integrity, and availability. _(N = None, L = Low, H = High)_

### CVSS Temporal Metrics

These measure the existence of a patch, an active exploit, or a workaround.

- **Exploit Code Maturity (E):** Exploit exists. _(Not Defined (X), High (H), Functional (F), Proof-of-Concept (P), Unproven (U))_
- **Remediation Level (RL):** Patch or workaround exists. _(Not Defined (X), Unavailable (U), Workaround (W), Temporary Fix (T), Official Fix (O))_
- **Report Confidence (RC):** Credibility of the known technical details. _(Not Defined (X), Confirmed (C), Reasonable (R), Unknown (U))_

This is just a quantitative measure that ignores the organizational context where the vulnerability is found. Because of that, it's important to rely on other tools to help organizations prioritize remediation. More on that later.

## CISA KEV

The **Murican** Cybersecurity and Infrastructure Security Agency (CISA) maintains the [Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) (KEV) — a living document that tracks CVEs actively exploited in the wild, maintained based on real evidence. CVEs are added to and removed from it based on these steps:

- **CVE ID Check:** The vulnerability must have a CVE assigned, otherwise it is not added.
- **Active Attack:** Requires reliable evidence that it's being exploited in the wild. Proofs of concept (POCs) do not count. If it's not deemed dangerous, it's rejected and not added to the catalog.
- **Patch Existence:** If there's a patch or solution available, the CVE is discarded.
- Otherwise, the CVE is added to the catalog and classified as an immediate call to action.

Suffice to say, if the CVE you're dealing with is found in the CISA KEV, you have a *great day* ahead of you. 

![Bad time](badtime.jpeg#center)

OK, but what if the CVE isn't part of the KEV, can I go back to browsing Reddit? Not quite yet.

## Exploit Prediction Scoring System (EPSS)

It's a living prediction engine run by [FIRST](https://www.first.org/epss/) that determines the probability a CVE could be exploited in the next 30 days. The score is continuously updated following this cycle:

1. Collect data on each vulnerability.
2. Collect evidence of daily exploitation.
3. Train models on the vulnerability and attacker behavior.
4. Refresh the data and run the model to calculate the probability the vulnerability could be exploited.
5. Rinse and repeat.

## Prioritizing Further Analysis and Remediation

The tools discussed so far give us quantitative data that ignores organizational context. This is great! We have a bunch of numbers and metrics. But what should we actually be focusing on? Introducing the SSVC.

### Stakeholder-Specific Vulnerability Categorization (SSVC)

It's a framework that helps prioritize vulnerability analysis and remediation using an interactive [decision tree](https://www.cisa.gov/ssvc-calculator). CISA has published a [full SSVC guide online](https://www.cisa.gov/sites/default/files/publications/cisa-ssvc-guide%20508c.pdf).

**State of Exploitation:**
- **None:** No evidence of active exploitation and no publicly available proof of concept (POC) demonstrating how to exploit the vulnerability.
- **Public POC:** Sample source code that serves as a POC is found in public sources such as Metasploit or ExploitDB.
- **Active:** The exploit has been proven possible using a well-known method, and is present in NVD or CISA KEV.

**Automatable:** How quickly it can be exploited across a large number of devices (e.g., WannaCry). If CVSS Attack Complexity is high and User Interaction is required, it means the vulnerability is difficult to automate.
- **Yes:** No barriers preventing worm-like spread in an environment; no user interaction needed to spread.
- **No:** Can't be reliably automated, or there's at least one barrier to automation.

**Technical Impact:** How severe the attack is to the CIA triad. Uses the CIA fields from the CVSS score.
- **Partial:** Impact is limited, but not full control or access.
- **Total:** Full control and access.

**Mission Prevalence and Public Well-Being Impact:** The final branch of the decision tree.
- **Mission Essential Function (MEF):** Is it affecting a core system? Think a bank's core system.
- **Public Well-Being:** How the impact affects people.

**SSVC Outcomes:** Basically, SSVC's recommendation after all the data has been input. Helps prioritize remediation. The outcomes are:
- **Track:** Monitor the CVE, but it can wait.
- **Track\*:** Monitor closely, this is getting more serious.
- **Attend:** Fix soon. Like, soon, soon.
- **Act:** Fix now. This is super cereal.

## Tidying Everything Together: The Risk Assessment Matrix

The purpose of all these tools is to come up with a systematic way of assessing risk and prioritizing analysis and remediation once the scans return a massive list of vulnerabilities. The idea is to combine Severity (CVSS), Exploitability (CISA KEV and EPSS score), and Exposure (where are the affected assets located? Is the device internet-facing, or is it a domain controller reachable only internally?).

With this, we can create a Risk Assessment Matrix. The order I'd propose:

1. Systems with external exposure.
2. Any CVE present in CISA KEV.
3. Then EPSS at 50% (0.5) or higher.
4. Then CVSS 4.0 or higher (adjusted to regulatory obligations).
5. Internal systems.

Perhaps this list could help you prioritize the thousands of vulnerabilities discovered after the last scan. But for the love of God, ask yourself first whether something happened prior to the scan that could have caused the credentialed scan to fail, or the routing to change, or perhaps those vulnerabilities are indeed real, and you're in for a proper scramble. In any case, *"I hope this has been informative, and I'd like to thank you for reading."*
</div>  
