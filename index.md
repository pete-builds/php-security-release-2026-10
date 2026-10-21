---
title: "PHP critical security release, October 2026 (CVE-2026-109618, CVE-2026-109619)"
date: 2026-10-21
updated: 2026-10-21 9:19 AM ET
summary: "PHP ships fixes for two critical (CVSS 4.0 9.2) vulnerabilities on 21 Oct 2026 at 10:00 AM ET. Details leaked early, early attacks are reported, and patching is the only reliable fix."
---

# PHP critical security release, October 2026

**Status at 9:19 AM ET, 21 Oct 2026: fixes NOT yet released.** This page is re-checked after the 10:00 AM ET release and every 30 minutes for two days.

## TL;DR

PHP's security team will ship fixes for two critical vulnerabilities, each rated CVSS v4.0 9.2, on Wednesday 21 October 2026 at 14:00 UTC (10:00 AM ET), and has not yet published affected or fixed versions ([source](https://www.php.net/)). Details leaked ahead of the coordinated release, and security vendors report early attacks, so anyone running PHP on the internet should be ready to update the moment fixed builds reach their package source ([source](https://sansec.io/research/php2shell)). Configuration tweaks reduce exposure at best; PHP developers say patching is the only real fix ([source](https://sansec.io/research/php2shell)).

## Current Status

- **Release: pending.** php.net's newest listed versions are still 8.5.11, 8.4.26, 8.3.35 and 8.2.34 ([source](https://www.php.net/)).
- **Severity:** two critical vulnerabilities, both CVSS v4.0 9.2 ([source](https://www.php.net/)).
- **CVE IDs** CVE-2026-109618 and CVE-2026-109619 come from CERT-EU and security vendors, not yet from php.net ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)).
- **Exploitation:** Patchstack reports active exploitation from multiple hosting partners, and Cloudflare reports early attacks ([source](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)). Neither CVE is in the CISA KEV catalog (version 2026.10.20, checked 9:17 AM ET) ([source](https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json)).
- **Distro packages:** Ubuntu published both CVEs at Critical priority, every PHP package still "Needs evaluation" (no fixed packages) as of 9:19 AM ET ([source](https://ubuntu.com/security/CVE-2026-109618)). Debian and Red Hat had no CVE page at last check.
- **Unresolved:** affected SAPIs, attack preconditions, fixed version numbers, official mitigations.

## Table of Contents

- [TL;DR](#tldr)
- [Current Status](#current-status)
- [Table of Contents](#table-of-contents)
- [What To Do](#what-to-do)
- [What Happened](#what-happened)
- [Technical Analysis](#technical-analysis)
  - [Who is affected](#who-is-affected)
  - [Interim mitigations](#interim-mitigations)
  - [End-of-life PHP](#end-of-life-php)
- [Where Patches Will Come From](#where-patches-will-come-from)
- [Timeline](#timeline)
- [Indicators of Compromise](#indicators-of-compromise)
- [Dark Web and Threat Actor Footprint](#dark-web-and-threat-actor-footprint)
- [Confidence Assessment](#confidence-assessment)
- [Open Questions](#open-questions)
- [Sources](#sources)
- [Update History](#update-history)

## What To Do

1. **Inventory every PHP runtime you serve to the internet**, including control panels and admin tools that bundle their own PHP. CERT-EU's first recommendation is to identify PHP-based applications and prepare to patch ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)).
2. **Update as soon as your package source ships the fix.** Supported branches 8.2, 8.3, 8.4 and 8.5 are the ones that receive security fixes ([source](https://www.php.net/supported-versions.php)). Updating the package is not enough on its own: restart PHP-FPM or the web server so the new binary is actually running (general practice, not from the advisories).
3. **If you cannot patch promptly,** CERT-EU advises considering isolating internet-facing applications from public networks until a fix is available ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)).
4. **Treat ini changes as partial.** See [Interim mitigations](#interim-mitigations).
5. **Plan off end-of-life PHP.** See [End-of-life PHP](#end-of-life-php).
6. **Do not run leaked exploit code** to test yourself: Tenable warns unverified exploit code is sometimes laced with malware ([source](https://www.tenable.com/blog/php2shell-cve-2026-109618-cve-2026-109619-frequently-asked-questions-about-critical-php-remote)).

## What Happened

On 20 October 2026 php.net announced the release, scheduled for Wednesday 21 October at 14:00 UTC, addressing "two critical vulnerabilities, both rated CVSS v4.0 9.2". It said "Full details, affected versions, and the fixed versions will be published with the release," and reserved the right to release earlier "should details of either vulnerability become public, or should evidence of active exploitation appear in the wild" ([source](https://www.php.net/)). The notice was relayed to the oss-security list the same day ([source](https://www.openwall.com/lists/oss-security/2026/10/20/34)).

Sansec reports that details about the embargoed bugs leaked on 20 October, that watchTowr confirmed they are remote code execution vulnerabilities tracked as CVE-2026-109618 and CVE-2026-109619, and that a screenshot of an exploit tool named "PHP2Shell", credited to Calif.IO, leaked alongside ([source](https://sansec.io/research/php2shell)). CERT-EU's Advisory 2026-018 shows a release date of 20-10-2026 20:33:27 (time zone not stated) and a v1.0 history entry dated 21/10/2026; it states no technical details are available beyond the CVSS scores ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)).

## Technical Analysis

No official technical description exists before the release. Everything below except branch support comes from secondary sources.

### Who is affected

- CERT-EU: the vulnerabilities "could affect all PHP versions from PHP 5 onwards" ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)).
- Sansec: "all web servers running PHP 5.4 or later", based on the leaked tool's banner claim ([source](https://sansec.io/research/php2shell)).
- Patchstack: the flaw is in the PHP interpreter itself, so WordPress, Drupal, Magento, Laravel and other PHP applications are exposed if they run a vulnerable PHP version ([source](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)).
- php.net has not confirmed any affected range ([source](https://www.php.net/)).

### Interim mitigations

None of these is official guidance from php.net, and the sources disagree.

| Measure | Who suggests it | Caveat |
|---|---|---|
| `session.upload_progress.enabled = Off` and `session.use_strict_mode = 1` | Patchstack, which says it reduces exposure but does not fix the vulnerability ([source](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)) | Sansec relays PHP developer Banyard: patching is the only mitigation, so do not rely on that setting alone ([source](https://sansec.io/research/php2shell)). PHP 8.6 release manager Joe Ferguson: "ini settings won't save you" ([source](https://www.tenable.com/blog/php2shell-cve-2026-109618-cve-2026-109619-frequently-asked-questions-about-critical-php-remote)). |
| Block `multipart/form-data` at the web server | Sansec ([source](https://sansec.io/research/php2shell)) | Breaks file uploads. |
| Cloudflare WAF rules | Reported by Patchstack and Tenable | Described as a stopgap only. |
| Isolate internet-facing apps until patched | CERT-EU ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)) | Downtime. |

### End-of-life PHP

php.net lists security support ending 31 Dec 2026 for 8.2, 31 Dec 2027 for 8.3, 31 Dec 2028 for 8.4 and 31 Dec 2029 for 8.5; older branches are end of life ([source](https://www.php.net/supported-versions.php)). Tenable says end-of-life branches are not expected to receive fixes ([source](https://www.tenable.com/blog/php2shell-cve-2026-109618-cve-2026-109619-frequently-asked-questions-about-critical-php-remote)). Version numbers reported before the release point to 8.5.12, 8.4.27, 8.3.36 and 8.2.35 (Tenable, unconfirmed by php.net).

## Where Patches Will Come From

Most servers do not install PHP from php.net. The fix reaches you through your package source, and each moves on its own clock.

| Source | Status at last check |
|---|---|
| [Debian security tracker](https://security-tracker.debian.org/tracker/CVE-2026-109618) | No page for CVE-2026-109618 (HTTP 404) |
| Ubuntu CVE tracker | Published 21 Oct, priority Critical, description "Remote code execution issue"; every listed PHP package "Needs evaluation", none fixed, at 9:19 AM ET ([109618](https://ubuntu.com/security/CVE-2026-109618), [109619](https://ubuntu.com/security/CVE-2026-109619)). Notes reference GitHub advisories GHSA-w8hw-w9fx-j79w and GHSA-6wmf-4rv5-p3rp, not yet public (404). |
| [Red Hat CVE database](https://access.redhat.com/security/cve/CVE-2026-109618) | No page (HTTP 404; page may render client-side) |
| Remi's repository (RHEL family) | Sansec reports it will not get the fix on 21 Oct ([source](https://sansec.io/research/php2shell)); an open issue asks about it with no maintainer reply ([source](https://github.com/remicollet/remirepo/issues/351)) |
| Hosting panels, Docker image, Ondrej PPA | Not yet checked against a first-party page |

## Timeline

| Time (ET) | Event |
|---|---|
| 2026-10-21 9:19 AM | Ubuntu CVE pages live for both CVEs, priority Critical, no fixed packages yet ([source](https://ubuntu.com/security/CVE-2026-109618)) |
| 2026-10-21 10:00 AM | Scheduled release ([source](https://www.php.net/)). **Pending at last check.** |
| 2026-10-21 | Sansec analysis ([source](https://sansec.io/research/php2shell)) |
| 2026-10-21 3:43 AM | Cloudflare reports early attacks against PHP sites, per Sansec ([source](https://sansec.io/research/php2shell)) |
| 2026-10-21 3:34 AM | Multiple active exploitation incidents reported to Patchstack by hosting partners, per Patchstack's timeline ([source](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)) |
| 2026-10-20 | CERT-EU Advisory 2026-018 released (field reads 20:33:27, zone not stated) ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)) |
| 2026-10-20 4:22 PM | Notice relayed to oss-security ([source](https://www.openwall.com/lists/oss-security/2026/10/20/34)) |
| 2026-10-20 | php.net pre-announcement ([source](https://www.php.net/)); details leak, per Sansec ([source](https://sansec.io/research/php2shell)) |

## Indicators of Compromise

No official source has published indicators of compromise as of 9:17 AM ET, and none of the vendor posts reviewed included primary exploitation evidence. A search of abuse.ch ThreatFox, URLhaus and MalwareBazaar, AlienVault OTX and the CISA KEV catalog, synced 8:25 AM ET, returned no indicators, pulses or entries for CVE-2026-109618, CVE-2026-109619 or Calif.io. A control search for "PHP" in the same index returned results, so the empty result reflects the feeds, not a broken search.

## Dark Web and Threat Actor Footprint

Dark web index search (Ahmia) was attempted at 9:15 AM ET and failed: the search engine redirected every query, so no dark web result exists for this edition, positive or negative. The only actor-adjacent detail found is the "PHP2Shell" exploit: Sansec describes a leaked screenshot of the tool credited to Calif.IO ([source](https://sansec.io/research/php2shell)), and Tenable reports a video by researcher Kirill Firsov demonstrating the exploit chain, credited to LCFR and Calif.io, and says code claiming to exploit the bugs is circulating publicly but unverified ([source](https://www.tenable.com/blog/php2shell-cve-2026-109618-cve-2026-109619-frequently-asked-questions-about-critical-php-remote)).

## Confidence Assessment

| Claim | Confidence |
|---|---|
| Release date, time, CVSS 9.2 x2, details withheld | **High** (php.net; independently restated by CERT-EU and Sansec) |
| Supported branches and EOL dates | **High** for the dates (php.net is the authority); EOL branches getting no fix also stated by Tenable |
| CVE IDs CVE-2026-109618 / -109619 | **Medium**: CERT-EU and vendors agree; absent from [NVD](https://services.nvd.nist.gov/rest/json/cves/2.0?cveId=CVE-2026-109618) (0 results) and the [CVE.org API](https://cveawg.mitre.org/api/cve/CVE-2026-109618) (404) at 8:56 AM ET |
| Early attacks / active exploitation | **Medium**: Patchstack and Cloudflare (via Sansec and Tenable); no primary evidence published |
| Affected range "PHP 5 onwards" | **Medium-Low**: CERT-EU says "could affect"; Sansec's figure rests on a leaked tool banner |
| ini settings as mitigation | **Low** as a fix; PHP developers quoted against relying on it |
| Fixed version numbers 8.5.12 / 8.4.27 / 8.3.36 / 8.2.35 | **Low** until php.net publishes |

## Open Questions

- Fixed versions, affected SAPIs (FPM, CGI, mod_php, CLI) and preconditions: due with the release.
- When Debian, Ubuntu, RHEL-family, Plesk, cPanel and the Docker `php` image ship builds.
- When CISA adds either CVE to KEV; neither was listed in catalog version 2026.10.20.
- Whether any official mitigation exists for systems that cannot patch.

## Sources

**Official**
- [php.net: Upcoming Critical PHP Security Release](https://www.php.net/)
- [php.net: Supported Versions](https://www.php.net/supported-versions.php)
- [oss-security relay, 2026-10-20](https://www.openwall.com/lists/oss-security/2026/10/20/34)

**Government CERT**
- [CERT-EU Security Advisory 2026-018](https://cert.europa.eu/publications/security-advisories/2026-018/)

**Security vendors**
- [Sansec: PHP2Shell](https://sansec.io/research/php2shell)
- [Patchstack: PHP major vulnerability alert](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)
- [Tenable FAQ](https://www.tenable.com/blog/php2shell-cve-2026-109618-cve-2026-109619-frequently-asked-questions-about-critical-php-remote)

**Package sources**
- [Ubuntu: CVE-2026-109618](https://ubuntu.com/security/CVE-2026-109618)
- [Ubuntu: CVE-2026-109619](https://ubuntu.com/security/CVE-2026-109619)
- [Remi repository issue #351](https://github.com/remicollet/remirepo/issues/351)

**CVE databases and threat feeds**
- [CISA Known Exploited Vulnerabilities catalog (JSON feed)](https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json)
- [NVD API, CVE-2026-109618](https://services.nvd.nist.gov/rest/json/cves/2.0?cveId=CVE-2026-109618)
- [CVE.org API, CVE-2026-109618](https://cveawg.mitre.org/api/cve/CVE-2026-109618)

## Update History

| Time (ET) | Report change |
|---|---|
| 2026-10-21 9:19 AM | Ubuntu CVE pages published (Critical, no fixes yet). |
| 2026-10-21 9:17 AM | Threat intel added: CISA KEV checked directly (not listed), feed search for indicators (none), dark web search attempted (search engine unavailable). |
| 2026-10-21 8:58 AM | Corrections from cold verification: CERT-EU release date, Patchstack timeline time, KEV claim removed, PHP2Shell video and public code, confidence sourcing, CVE database citations. |
| 2026-10-21 8:49 AM | First edition, before the release. |
