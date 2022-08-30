---
description: >-
  This page summarizes our Security documentation, Audits, Processes and
  Security Management Plan, which is our plan for reducing risks and preventing
  and responding to incidents.
---

# Security

## Security Repository

Our security Github repository is now publicly available ([https://github.com/babylon-finance/security](https://github.com/babylon-finance/security))&#x20;

It includes our [**Security Audits**](https://github.com/babylon-finance/security/blob/main/audits/README.md), our [**Security Contact PGP keys**](https://github.com/babylon-finance/security/blob/main/SECURITY.md#receiving-disclosures)**, our** [**Public Disclosures**](https://github.com/babylon-finance/security/tree/main/disclosures) **** information **** (if any) and it will also include our **** [**Bug Bounty Program**](https://github.com/babylon-finance/security/blob/main/SECURITY.md#bug-bounty-program)

{% embed url="https://github.com/babylon-finance/security" %}

{% embed url="https://immunefi.com/bounty/babylonfinance" %}

## Security Master Plan

Security is an essential component of the protocol. It's an ongoing process, not an add-on or review that happens once and is complete. We follow a security-by-design process that includes **continuous** actions at the infrastructure and SDLC (Software Development Life Cycle) level. This continuous process is part of our Security Management Plan which is our plan for reducing risks and preventing and responding to incidents.

## Methodology

The following agile methodology has been defined for Babylon in order to identify, address and protect the protocol from the most dangerous risks:

| Step   | Process                                                                                                                                                                        |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Step 0 | **Methodology:** Define the agile methodology influenced by ISO27K standards to run Risk Assessment, Risk Management, and set-up an Incident Response Plan.                    |
| Step 1 | **Threat modeling:** Threat modeling and risk identification through in-depth research about prior DeFi security incidents.                                                    |
| Step 2 | **Technical security analysis:** Identify security issues and bugs by running continuous security audits by internal and/or external staff/companies.                          |
| Step 3 | **Risk Assessment and Countermeasures:** Identify main risks as well as their ideal countermeasures and remediations based on the state of play.                               |
| Step 4 | **Projects and initiatives and their prioritization:** Define the Security Risk Management projects, their prioritization and suggest some proposals to improve (Deming Cycle) |
| Step 5 | **Approval:** Get Approval                                                                                                                                                     |
| Step 6 | **Launch the Security Master Plan**                                                                                                                                            |
| Step 7 | **Launch the Incident Response Plan**                                                                                                                                          |

## Threat Modeling summary

As part of the risk assessment and management process we present below some of the most relevant security risks identified during this step. As part of the security-by-design methodology our team has identified and implemented several countermeasures either to reduce their impact and/or the probability of occurrence.

![](../.gitbook/assets/Security\_Risks\_\(and\_associated\_countermeasures\).png)

## Technical Security Analysis summary

The **scope** of our technical security analysis includes but it is not limited to the protocol, dApp as well as the required infrastructure:

![](../.gitbook/assets/Captura\_de\_pantalla\_2021-06-28\_a\_las\_17.53.53.png)

#### Internal (continuous) security audit

Most organizations run a technical security analysis at certain moments. However, we think it's important to have a **continuous security analysis process.** Any new change, fix or pull request is audited using a security-by-design pattern. The audit is run by a different team member and there should be a minimum of two different team members auditing the code pushed by a third team member.

![](../.gitbook/assets/Captura\_de\_pantalla\_2021-06-28\_a\_las\_17.35.57.png)

As shown in the picture, issues are categorized depending on their severity.

The continuous process helps us to identify problems and fix them quickly while we document the pull request that solves the issue. The fix usually includes the exploit to reproduce, validates the issue is exploitable, and include the patch that disables the exploit.

#### External security audits

We work with external security audit partners to review our code at different snapshots in time. These companies work closely with our internal security audit team.

We plan to work with **3 different security companies** for three complete audits (each audit has one to two rounds).

The first security company has already finished its audit (two rounds) and did not find any critical issues. The audit report and company details will be published together with reports from the other two companies once we launch publicly.

## Security Risk Management and Projects Prioritization

After a security analysis, we implemented several countermeasures, each of which are in a different stage today. This process is considered one of the most important parts of Risk Management. Security Projects are defined by different topics, each with different actions and countermeasures as shown in the following image:

![](../.gitbook/assets/Security\_Risk\_Management.png)

Just as an example, some of the most relevant countermeasures are:

* **Internal (continuous) security audit (ISA) process**
* **External security audits by different security audit firms**
* **Pause guardian**
* **Reentrancy guard**
* **Flashloans hard lock**
* **Key management policy (2FA, Multisig, etc.)**
* **Security test coverage**
* **Pair programming**
* **Security event management** (monitoring and detection)
* **Cyber exercises**
* **Incident response plan**
* **Vulnerability management**
* **Bug bounty planned**
* **Need to know policy**
* **Awareness raising**

Following the standards like ISO27K and best practices, we are also identifying new proposals to implement in the future (Deming cycle).

## Incident response summary

Nearly every organization experiences cyber attacks, and unfortunately Babylon will not be an exception. This is why we have an incident response plan that will allow us to more rapidly respond and contain an incident if/when one occurs.&#x20;

As part of the incident response plan we identify different phases, each of them with different sub-processes:

![](../.gitbook/assets/Captura\_de\_pantalla\_2021-06-28\_a\_las\_18.17.37.png)

We know that **100% security does not exist** so we need to be prepared for recovery. That is why we work on containment and recovery measures that might help Babylon and its users in the event of an incident.

#### Monitoring and detection

We have implemented different mechanisms to provide monitoring and detection capabilities. Some of them are well-known like **OpenZeppelin Defender** and **Tenderly**. We also work with **business partners** that are helping Babylon to improve its alerting system channels.

![](../.gitbook/assets/Captura\_de\_pantalla\_2021-06-28\_a\_las\_20.09.06.png)

![](../.gitbook/assets/Captura\_de\_pantalla\_2021-06-28\_a\_las\_20.09.16.png)

#### _Support channel_

_The protocol is in alpha testing so we have opened a support channel for our users which might help in the detection of hidden issues or bugs. Any new issue is validated, categorized and prioritized to better support our users._

Note: all **security incidents and vulnerabilities** must be reported using an **encrypted email** as described below in the next section.

#### Security Incidents and Vulnerability Management Program

Our Vulnerability Management Program has two parts:

* **Bug bounty program (starting soon)**
* **Vulnerability / Security Incident report intake channel (please use encrypted communications using our public** [**PGP/encrypted communication channel**](https://keys.openpgp.org/vks/v1/by-fingerprint/5495D6527E623F06B32FDDDA876AAFCF0323BF77)**)**

At Babylon Finance we are committed to working with researchers who submit security vulnerability notifications to us. We will resolve issues on an appropriate timeline and perform a coordinated release, giving credit to the reporter if they would like.

Please submit **all security issues** **by email** to **the following point of contact** and please give as much details as you can in order we can reproduce and validate the security issue, vulnerability or incident:

| **Contact**  | **Public key**                                                                                     | **Email**                       | **Key ID**    |
| ------------ | -------------------------------------------------------------------------------------------------- | ------------------------------- | ------------- |
| **Security** | ****[PGP](https://keys.openpgp.org/vks/v1/by-fingerprint/5495D6527E623F06B32FDDDA876AAFCF0323BF77) | **security at babylon.finance** | **0323 BF77** |
| **Raúl**     | [PGP](https://keys.openpgp.org/vks/v1/by-fingerprint/D3A79AD075F1ABDA54C05380F3BB8736610687C1)     | **raul at babylon.finance**     | **6106 87C1** |

#### DeFi Cyber Threat Intelligence and best practices

We believe that DeFi security will improve if we foster collaboration between the different DeFi protocols. If you're interested in sharing best practices or working together, please feel free to reach out to the email above.

### References:

* OpenZeppelin Defender entries from \[Advisor]\([https://defender.openzeppelin.com/#/advisor/doclist?)](https://defender.openzeppelin.com/#/advisor/doclist?\)).
* Magoo graveyard at [Github](https://magoo.github.io/Blockchain-Graveyard/).
* Thesis paper from [MIT research](http://web.mit.edu/smadnick/www/wp/2019-05.pdf).
* Yearn finance security process: [https://github.com/yearn/yearn-security](https://github.com/yearn/yearn-security), [https://github.com/yearn/yearn-security/blob/master/SECURITY.md](https://github.com/yearn/yearn-security/blob/master/SECURITY.md)
* FIRST: [https://www.first.org/standards/frameworks/csirts/csirt\_services\_framework\_v2.1](https://www.first.org/standards/frameworks/csirts/csirt\_services\_framework\_v2.1)
* NIST: [https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
* SEI (Carnegie Mellon): [https://resources.sei.cmu.edu/library/asset-view.cfm?assetID=6305](https://resources.sei.cmu.edu/library/asset-view.cfm?assetID=6305)
* Reference Security Incident Taxonomy Working Group. Reference Security Incident Classification Taxonomy: [https://github.com/enisaeu/Reference-Security-Incident-Taxonomy-Task-Force/blob/master/working\_copy/humanv1.md](https://github.com/enisaeu/Reference-Security-Incident-Taxonomy-Task-Force/blob/master/working\_copy/humanv1.md)
* Security policies (spanish)(INCIBE): [https://www.incibe.es/protege-tu-empresa/herramientas/politicas](https://www.incibe.es/protege-tu-empresa/herramientas/politicas)
* Cyber-incident management procedure (INCIBE): [https://www.incibe-cert.es/sites/default/files/contenidos/guias/doc/incibe-cert\_cyber\_incident\_management\_private\_sector.pdf](https://www.incibe-cert.es/sites/default/files/contenidos/guias/doc/incibe-cert\_cyber\_incident\_management\_private\_sector.pdf)
* Agile online Risk Assessment tool (spanish)(INCIBE): [https://adl.incibe.es/#](https://adl.incibe.es)
* MIT: Systematic Approach to Analyzing Security and Vulnerabilities of Blockchain Systems: [http://web.mit.edu/smadnick/www/wp/2019-05.pdf](http://web.mit.edu/smadnick/www/wp/2019-05.pdf)
* ISO 27001: [https://www.iso.org/isoiec-27001-information-security.html](https://www.iso.org/isoiec-27001-information-security.html)
* ISO 31000: [https://www.iso.org/iso-31000-risk-management.html](https://www.iso.org/iso-31000-risk-management.html)
* ISO 22301: [https://www.iso.org/standard/75106.html](https://www.iso.org/standard/75106.html)
* SecurityMetrics [https://www.securitymetrics.com/lp/pci/how-to-make-and-implement-a-successful-incident-response-plan](https://www.securitymetrics.com/lp/pci/how-to-make-and-implement-a-successful-incident-response-plan)
* Traffic Light Protocol (TLP): [https://www.first.org/tlp/](https://www.first.org/tlp/)
