---
uid: releaseNotes
---

# Release notes

*{ Product version x.x }* 

## Overview

<!--Insert a brief description of your product here and a cross-reference for more information.  If these release notes cover service packs or patches in addition to the major release numbers, briefly identify each version covered.-->

## Fixes and enhancements

<!--*Remove this section if this is the first release of a product. If multiple releases are covered by this note, for example, if service packs and patches are added, these can either be sectioned by release, or, if lengthy, can have sub-pages per release.* -->

### Fixes

The following items were resolved in release *{ x.x.x.xxxx }* - *{ mm/dd/yyyy }*:

<!-- Use table style:-->
Item | Description
---- | -----------
*{ work item # }* | *{ work item release note }*

### Enhancements

The following features were added in release *{ x.x.x.xxxx }* - *{ mm/dd/yyyy }*:

<!--*Use bullet style:*-->
* *{ new feature }*
* *{ new feature }*

<!--OR

*Use  table style:*-->
Item | Description
---- | -----------
*{ work item # }* | *{ feature description }*

## Known issues

The following problems and enhancements have been deferred until a future release.

<!--*Use bullets and tables as necessary (table format below).* -->
Item | Description
---- | -----------
*{ item # }* | *{ problem/enhancement description }*

## System requirements

<!--*Provide a cross-reference to the system requirements section. For example,*-->

Refer to [System requirements](xref:SystemRequirements).

## Installation and upgrade

<!--*Provide a cross-reference to the installation procedure. For example,*-->

Refer to [Install the adapter](xref:InstallTheAdapter).

## Uninstallation

<!--*Provide a cross-reference to the uninstallation procedure. For example,*-->

Refer to [Uninstall the adapter](xref:UninstallTheAdapter).

## Security information and guidance

### OSIsoft’s commitment

Because the PI System often serves as a barrier protecting control system networks and mission-critical infrastructure assets, OSIsoft is committed to (1) delivering a high-quality product and (2) communicating clearly what security issues have been addressed. This release of *{ product name }* is the highest quality and most secure version of the *{ product name }* released to date. OSIsoft's commitment to improving the PI System is ongoing, and each future version should raise the quality and security bar even further.

### Vulnerability communication

The practice of publicly disclosing internally discovered vulnerabilities is consistent with the Common Industrial Control System Vulnerability Disclosure Framework developed by the Industrial Control Systems Joint Working Group (ICSJWG). Despite the increased risk posed by greater transparency, OSIsoft is sharing this information to help you make an informed decision about when to upgrade to ensure your PI System has the best available protection.

For more information, refer to [OSIsoft’s Ethical Disclosure Policy (https://www.osisoft.com/ethical-disclosure-policy)](https://www.osisoft.com/ethical-disclosure-policy).

To report a security vulnerability, refer to [OSIsoft's Report a Security Vulnerability (https://www.osisoft.com/report-a-security-vulnerability)](https://www.osisoft.com/report-a-security-vulnerability).

### Vulnerability scoring

OSIsoft has selected the Common Vulnerability Scoring System (CVSS) to quantify the severity of security vulnerabilities for disclosure. To calculate the CVSS scores, OSIsoft uses the National Vulnerability Database (NVD) calculator maintained by the National Institute of Standards and Technology (NIST).  OSIsoft uses Critical, High, Medium and Low categories to aggregate the CVSS Base scores. This removes some of the opinion related errors of CVSS scoring.  As noted in the CVSS specification, Base score range from 0 for the lowest severity to 10 for the highest severity.

### Overview of new vulnerabilities found or fixed

This section is intended to provide relevant security-related information to guide your installation or upgrade decision. OSIsoft is proactively disclosing aggregate information about the number and severity of *{ product name }* security vulnerabilities that are fixed in this release.

<!--*Provide an overview of the types of security vulnerabilities fixed in this release*-->

<!--*NOTE:  If NO security vulnerabilities are identified in the current release, please use the following statement:*-->

No security-related information is applicable to this release

<!--*When vulnerabilities exist, product teams should decide which format works best specific to the release and/or is applicable.  Two different samples are provided below.*-->

**Sample A** - For this release of the *{ product name }*, *{ x number }* of security vulnerability has been identified and fixed.

Based on the CVSS scoring system this issue has been categorized as a High (7.0 – 8.9). This high-level security issue is network accessible and has been resolved in the *{ product release name }*. To reduce exposure to this security issue, either limit access to the port used by the PI SQL products, or upgrade to the latest release.  

**Sample A with OSIsoft sub-component** - Based on the CVSS scoring system this issue has been categorized as a High (7.0 – 8.9). This high-level security issue has been resolved in *{ OSIsoft’s PI SDK version 1.1 sub-component }* which has been packaged in this *{ PI ProcessBook 1998 release }*. To reduce exposure to this security issue upgrade to the latest release.

**Sample A with 3rd Party sub-component** - Based on the CVSS scoring system this issue has been categorized as a High (7.0 – 8.9). This high-level security issue has been resolved in the 3rd Party *{ OpenSSL sub-component }* which has been packaged in this *{ PI Adapter for \<AdapterName\> 1.2 }* release . To reduce exposure to this security issue upgrade to the latest release.

**Sample B** - For this release of the *{ product name }*, *{x number}* of security vulnerability has been identified and fixed. The table below provides an overview of the types and severity of the security fixes.

Security vulnerabilities fixed in this release  
Severity category| CVSS base score range  | Number of fixed vulnerabilities
---- | ------ | -----------
Critical | 9.0-10 | *{ quantity }*
High | 7.0-8.9 | *{ quantity }*
Medium | 4.0-6.9 | *{ quantity }*
Low | 0-3.9 | *{ quantity }*

**Sample B with sub-components** - For this release of the *{product name}*, *{x number}* of security vulnerability has been identified and fixed. The tables below provide an overview of the types and severity of the security fixes. {*Some/All*} of the vulnerabilities were associated with *{OSIsoft/3rd Party}* sub-component {*name*} which is packaged in this release.

Summary of security vulnerabilities fixed in this release

Severity category| CVSS base score range  | Number of fixed vulnerabilities
---- | ------ | -----------
Critical | 9.0-10 | *{ quantity }*
High | 7.0-8.9 | *{ quantity }*
Medium | 4.0-6.9 | *{ quantity }*
Low | 0-3.9 | *{ quantity }*

Security vulnerabilities fixed in the *{OSIsoft/3rd Party}* sub-component {*name*}
Severity category| CVSS base score range  | Number of fixed vulnerabilities
---- | ------ | -----------
Critical | 9.0-10 | *{ quantity }*
High | 7.0-8.9 | *{ quantity }*
Medium | 4.0-6.9 | *{ quantity }*
Low | 0-3.9 | *{ quantity }*

<!--*Optional “Microsoft Software Security Defenses topic below” (applies to C++ projects)*-->

### Microsoft software security defenses

In addition to finding and fixing security bugs within the *{ product name }* , it is equally critical that OSIsoft leverage security defenses provided by the Microsoft Visual C++ compiler that builds it and the Microsoft Windows operating system that runs it. Over the past decade, Microsoft has continually added new defenses and improved existing defenses with successive versions of the compiler and the operating system.  To learn more about many of these key defenses, consult the Microsoft whitepaper Mitigating Software Vulnerabilities.

## Documentation overview

<!--*Remove this section if there is no documentation besides the documentation in which these release notes are included. For additional documentation, provide a brief description.

Example:*-->

**PI System Explorer User Guide:** Provides an overview and explains the functions of the PI System Explorer interface.

## Technical support and resources

<!--*Provide a cross-reference to the Technical Support and feedback section. For example,*-->

Refer to [Technical support and feedback](xref:TechnicalSupportAndFeedback).
