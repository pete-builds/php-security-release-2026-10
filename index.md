---
title: "PHP critical security release, October 2026 (CVE-2026-109618, CVE-2026-109619)"
date: 2026-10-21
updated: 2026-10-21 9:53 AM ET
summary: "PHP released 8.5.12, 8.4.27, 8.3.36 and 8.2.35 on 21 Oct 2026, ahead of the 10:00 AM ET slot, fixing two critical (CVSS 4.0 9.2) unauthenticated remote code execution bugs. Public exploits exist; distro packages were not yet out at 9:41 AM ET."
---

# PHP critical security release, October 2026

**Status at 9:53 AM ET, 21 Oct 2026: fixes RELEASED.** PHP 8.5.12, 8.4.27, 8.3.36 and 8.2.35 are out. Ubuntu, Debian, Red Hat, the Ondrej PPA and the Docker `php` image had no fixed build at 9:50 AM ET. This page is re-checked every 30 minutes for two days.

## TL;DR

PHP released 8.5.12, 8.4.27, 8.3.36 and 8.2.35 on 21 October 2026 to fix two critical vulnerabilities that "can allow unauthenticated remote code execution", and urges every user to update immediately ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). Every PHP installation that serves web requests is at risk, including PHP-FPM, Apache mod_php, php-cgi, FrankenPHP and LiteSpeed, with the highest risk on mod_php ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). Install your package source's fixed build the moment it ships and restart PHP; the official ini workarounds only narrow specific attack paths, and php.net says "updating is the only safe choice" ([source](https://www.php.net/)).

## Current Status

- **Release: out.** Fixed versions are 8.5.12, 8.4.27, 8.3.36 and 8.2.35; both GitHub advisories list everything below those versions as affected ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w), [source](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp)). The advisories were published at 9:34 AM ET, ahead of the announced 10:00 AM ET slot; php.net gives no reason for the early release.
- **Severity:** both CVEs rated Critical, CVSS v4.0 9.2, vector `CVSS:4.0/AV:N/AC:H/AT:P/PR:N/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N` ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)).
- **Distro packages: none yet.** At 9:38 to 9:41 AM ET, Ubuntu still showed every PHP package as "Needs evaluation" with no PHP security notice, Debian and Red Hat had no CVE record (HTTP 404), the Ondrej PPA had no build newer than 8.5.11, and the Docker `php` image had no final 8.5.12, 8.4.27, 8.3.36 or 8.2.35 tag. Remi's repository "will not have updates today" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)).
- **Exploitation:** public proof-of-concept exploits exist; NCSC-NL had "no indication of active exploitation" but "considers large-scale exploitation likely in the short term" ([source](https://github.com/NCSC-NL/php2shell)). Patchstack still reports "active exploitation attempts from multiple hosting partners", while its newest entry says its own telemetry shows "only probing attacks, no hard evidence of breaches yet" ([source](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)).
- **KEV:** neither CVE in the CISA KEV catalog (version 2026.10.20, checked 9:37 AM ET) ([source](https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json)). NVD still returned no record for either CVE at 9:37 AM ET.
- **Unresolved:** when distro, Docker and hosting-panel builds ship; whether exploitation in the wild is confirmed.

## Table of Contents

- [TL;DR](#tldr)
- [Current Status](#current-status)
- [Table of Contents](#table-of-contents)
- [What To Do](#what-to-do)
- [What Happened](#what-happened)
- [Technical Analysis](#technical-analysis)
  - [The two vulnerabilities](#the-two-vulnerabilities)
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
2. **Update to 8.5.12, 8.4.27, 8.3.36 or 8.2.35 as soon as your package source ships it.** php.net says "Linux distributions and LTS vendors received the patches in advance, so check your vendor for updated packages" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). Restart PHP-FPM or the web server afterwards so the new binary is actually running (general practice, not from the advisories).
3. **If you use Remi's repository,** php.net urges switching "to distribution packages or build from source, so you can immediately apply updates" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)).
4. **Until you can update, apply the official interim measures** in [Interim mitigations](#interim-mitigations), and remember they "do not make an installation safe" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)).
5. **Plan off end-of-life PHP.** See [End-of-life PHP](#end-of-life-php).
6. **Do not run leaked exploit code** to test yourself: Tenable warns unverified exploit code is sometimes laced with malware ([source](https://www.tenable.com/blog/php2shell-cve-2026-109618-cve-2026-109619-frequently-asked-questions-about-critical-php-remote)).

## What Happened

On 20 October 2026 php.net announced the release, scheduled for Wednesday 21 October at 14:00 UTC, addressing "two critical vulnerabilities, both rated CVSS v4.0 9.2". It said "Full details, affected versions, and the fixed versions will be published with the release," and reserved the right to release earlier "should details of either vulnerability become public, or should evidence of active exploitation appear in the wild" ([source](https://www.php.net/)). The notice was relayed to the oss-security list the same day ([source](https://www.openwall.com/lists/oss-security/2026/10/20/34)).

Sansec reports that details about the embargoed bugs leaked on 20 October, that watchTowr confirmed they are remote code execution vulnerabilities tracked as CVE-2026-109618 and CVE-2026-109619, and that a screenshot of an exploit tool named "PHP2Shell", credited to Calif.IO, leaked alongside ([source](https://sansec.io/research/php2shell)). CERT-EU's Advisory 2026-018 shows a release date of 20-10-2026 20:33:27 (time zone not stated) and a v1.0 history entry dated 21/10/2026; it states no technical details are available beyond the CVSS scores ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)).

On 21 October PHP published the two GitHub security advisories at 9:34 AM ET ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w), [source](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp)) and its security advisory, "Critical remote code execution vulnerabilities, update now", together with the four releases ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). Both were visible on php.net by 9:37 AM ET, before the announced 10:00 AM ET slot.

## Technical Analysis

The descriptions below come from php.net's advisory and the two GitHub advisories, which the advisory says "have the full technical details" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)).

### The two vulnerabilities

- **CVE-2026-109618, "RCE via OOM corruption" (GHSA-w8hw-w9fx-j79w):** "out-of-memory handling can interrupt PHP while internal request data is only partially initialized. Request cleanup may then treat the invalid data as valid" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). The advisory reports: "Complete remote code execution was validated over HTTP against mod_php with default PHP input and memory limits and normal prefork configurations with multiple workers" ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)).
- **CVE-2026-109619, "RCE via stale global state" (GHSA-6wmf-4rv5-p3rp):** "request-owned internal state can survive after its backing memory has been freed. A later request handled by the same worker may use that stale state before normal initialization resets it" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). "Session upload progress is enabled by default, and no application call to session_start() is required"; "Session lifecycle RCE was validated against mod_php and FPM" ([source](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp)).

### Who is affected

- php.net: "all PHP installations serving web requests, including PHP-FPM, Apache with mod_php, php-cgi, FrankenPHP and LiteSpeed. We consider the risk highest for mod_php, with a demonstrated exploit where an unauthenticated request can trigger the attack before any application code runs" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)).
- For CVE-2026-109618 on FPM, the advisory says source analysis "supports FPM as a susceptible" target, "though no complete FPM exploit has been demonstrated for this SAPI" ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)).
- Supported branches: versions below 8.2.35, 8.3.36, 8.4.27 and 8.5.12 are listed as affected ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)). Older branches: CERT-EU said before the release that the bugs "could affect all PHP versions from PHP 5 onwards" ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)), and PHP published source backports for 7.4, 8.0 and 8.1 (see [End-of-life PHP](#end-of-life-php)).
- Patchstack: the flaw is in the PHP interpreter itself, so WordPress, Drupal, Magento, Laravel and other PHP applications are exposed if they run a vulnerable PHP version ([source](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)).

### Interim mitigations

php.net: "while only an update is a complete solution, the following measures reduce exposure to specific attack paths but do not make an installation safe" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). Set ini values in php.ini, the FPM pool or server configuration, then restart every worker.

| CVE | Official measure | Limit stated by PHP |
|---|---|---|
| CVE-2026-109618 | `memory_limit=-1`, set where applications cannot override it (for example `php_admin_value`) ([source](https://www.php.net/archive/2026.php#2026-10-21-5)) | An underlying allocation failure "can still reach the vulnerable path" |
| CVE-2026-109618 | Low `post_max_size`: 8M without file uploads, 32M with them; `upload_max_filesize` below it ([source](https://www.php.net/archive/2026.php#2026-10-21-5)) | Also enforce request size and part limits at the web server or proxy, including requests without a usable `Content-Length` |
| CVE-2026-109618 | Container or cgroup memory limit as an availability boundary ([source](https://www.php.net/archive/2026.php#2026-10-21-5)) | Do not substitute `RLIMIT_AS` or `RLIMIT_DATA`, which can cause the vulnerable allocation failure |
| CVE-2026-109619 | `session.upload_progress.enabled=0` ([source](https://www.php.net/archive/2026.php#2026-10-21-5)) | "Do not rely on ini_set() or FPM .user.ini"; blocks the confirmed session paths only |
| CVE-2026-109619 | On PHP 8.5, `fatal_error_backtraces=0` ([source](https://www.php.net/archive/2026.php#2026-10-21-5)) | Blocks the confirmed fatal backtrace path only |
| CVE-2026-109619 | Remove invalid or deprecated per-request settings and fix attacker-reachable fatal errors and timeouts ([source](https://www.php.net/archive/2026.php#2026-10-21-5)) | Hiding diagnostics with `error_reporting`, `display_errors` or `@` does not help |
| CVE-2026-109619 | `pm.max_requests=1` "if performance is not a factor" ([source](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp)) | Advisory only, not on php.net's list |

Pre-release vendor suggestions that are not on PHP's list: blocking `multipart/form-data` at the web server (Sansec, breaks uploads) ([source](https://sansec.io/research/php2shell)) and `session.use_strict_mode = 1` (Patchstack) ([source](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)). Cloudflare's changelog lists emergency Managed Ruleset and Free Ruleset releases dated 21 October that detect or block "anomalous request patterns" without naming PHP or either CVE ([source](https://developers.cloudflare.com/changelog/post/2026-10-21-emergency-waf-release-cfmr/), [source](https://developers.cloudflare.com/changelog/post/2026-10-21-emergency-waf-release-free/)).

### End-of-life PHP

php.net lists security support ending 31 Dec 2026 for 8.2, 31 Dec 2027 for 8.3, 31 Dec 2028 for 8.4 and 31 Dec 2029 for 8.5; older branches are end of life ([source](https://www.php.net/supported-versions.php)). "PHP 8.1 and older no longer receive updates from the PHP project", but "the PHP security team provided minimal source-backports for PHP 7.4, PHP 8.0, and PHP 8.1 to help people building from source or without access to an LTS vendor" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). They are source branches, not packaged releases: [PHP 7.4](https://github.com/php/php-src/tree/PHP-7.4-CVE-2026-109618-109619), [PHP 8.0](https://github.com/php/php-src/tree/PHP-8.0-CVE-2026-109618-109619), [PHP 8.1](https://github.com/php/php-src/tree/PHP-8.1-CVE-2026-109618-109619).

## Where Patches Will Come From

Most servers do not install PHP from php.net. The fix reaches you through your package source, and each moves on its own clock.

| Source | Status at last check |
|---|---|
| [php.net](https://www.php.net/) | **Released:** 8.5.12, 8.4.27, 8.3.36, 8.2.35 (9:37 AM ET) |
| Ubuntu CVE tracker | Published 21 Oct, priority Critical; every listed PHP package "Needs evaluation", none fixed, and no PHP security notice, at 9:39 AM ET ([109618](https://ubuntu.com/security/CVE-2026-109618), [109619](https://ubuntu.com/security/CVE-2026-109619), [notices](https://ubuntu.com/security/notices)) |
| [Debian security tracker](https://security-tracker.debian.org/tracker/CVE-2026-109618) | No page for either CVE (HTTP 404) and no PHP advisory on the [Debian security page](https://www.debian.org/security/), at 9:39 AM ET |
| [Red Hat CVE data](https://access.redhat.com/hydra/rest/securitydata/cve/CVE-2026-109618.json) | No record (HTTP 404 at 9:38 AM ET) |
| [Ondrej Sury PPA](https://launchpad.net/~ondrej/+archive/ubuntu/php) | No PHP 8.x build published since 14 Oct; newest are 8.5.11, 8.4.26, 8.3.35, 8.2.34 (9:40 AM ET) |
| [Docker official `php` image](https://hub.docker.com/_/php) | No 8.5.12, 8.4.27, 8.3.36 or 8.2.35 tag; tags named `8.5.12RC1` and `8.4.27RC1` are release candidates, not the security release (9:38 AM ET) |
| Remi's repository (RHEL family) | php.net: "The repo will not have updates today" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)); issue open with no maintainer reply ([source](https://github.com/remicollet/remirepo/issues/351)) |
| Plesk, cPanel, CloudLinux | Not verified: Plesk's support article returned HTTP 403 to the check, CloudLinux's blog timed out, and no cPanel advisory was found |

## Timeline

| Time (ET) | Event |
|---|---|
| 2026-10-21 9:41 AM | No fixed distro, PPA or Docker builds yet (see [Where Patches Will Come From](#where-patches-will-come-from)) |
| 2026-10-21 9:37 AM | php.net front page lists the four releases and the security advisory, ahead of the 10:00 AM slot ([source](https://www.php.net/)) |
| 2026-10-21 9:34 AM | GHSA-6wmf-4rv5-p3rp and GHSA-w8hw-w9fx-j79w published ([source](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp), [source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)) |
| 2026-10-21 9:19 AM | Ubuntu CVE pages live for both CVEs, priority Critical, no fixed packages yet ([source](https://ubuntu.com/security/CVE-2026-109618)) |
| 2026-10-21 | NCSC-NL: public proof-of-concept exploits available, no indication of active exploitation ([source](https://github.com/NCSC-NL/php2shell)) |
| 2026-10-21 | Sansec analysis ([source](https://sansec.io/research/php2shell)) |
| 2026-10-21 3:43 AM | Cloudflare reports early attacks against PHP sites, per Sansec ([source](https://sansec.io/research/php2shell)) |
| 2026-10-21 3:34 AM | Multiple active exploitation incidents reported to Patchstack by hosting partners, per Patchstack's timeline ([source](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)) |
| 2026-10-20 | CERT-EU Advisory 2026-018 released (field reads 20:33:27, zone not stated) ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)) |
| 2026-10-20 4:22 PM | Notice relayed to oss-security ([source](https://www.openwall.com/lists/oss-security/2026/10/20/34)) |
| 2026-10-20 | php.net pre-announcement ([source](https://www.php.net/)); details leak, per Sansec ([source](https://sansec.io/research/php2shell)) |

## Indicators of Compromise

No official source has published indicators of compromise as of 9:50 AM ET, and no vendor page reviewed lists IPs, file names or webshells. For detection, the CVE-2026-109618 advisory names multipart requests "without a usable declared content length" and multipart parts with "an unrestricted number of headers" as out-of-memory vectors ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)), and Patchstack suggests matching "loosely on 'multipart' in the Content-Type header, case-insensitively" because attackers vary the formatting ([source](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)). For CVE-2026-109619 the advisory warns that "absence of an error in logs is not proof of safety" ([source](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp)).

A search at 9:39 AM ET of abuse.ch ThreatFox, URLhaus and MalwareBazaar, AlienVault OTX and the CISA KEV catalog, synced 9:25 AM ET, returned no indicators, pulses or entries for PHP2Shell, CVE-2026-109618, CVE-2026-109619 or Calif.io. Control searches returned 50 indicators each for AgentTesla and Mirai and the KEV entry for CVE-2024-4577, so the search works; the OTX feed held no records in this index, so it adds no evidence either way.

## Dark Web and Threat Actor Footprint

Dark web index search (Ahmia) was attempted again at 9:39 AM ET and was unavailable: the search engine redirected the query, so no dark web result exists for this edition, positive or negative. The only actor-adjacent detail found is the "PHP2Shell" exploit: Sansec describes a leaked screenshot of the tool credited to Calif.IO ([source](https://sansec.io/research/php2shell)), and Tenable reports a video by researcher Kirill Firsov demonstrating the exploit chain, credited to LCFR and Calif.io, and says code claiming to exploit the bugs is circulating publicly but unverified ([source](https://www.tenable.com/blog/php2shell-cve-2026-109618-cve-2026-109619-frequently-asked-questions-about-critical-php-remote)). NCSC-NL states that "Multiple public proof-of-concept exploits are already available" ([source](https://github.com/NCSC-NL/php2shell)).

## Confidence Assessment

| Claim | Confidence |
|---|---|
| Fixed versions 8.5.12 / 8.4.27 / 8.3.36 / 8.2.35, CVSS 9.2 x2 | **High**: php.net advisory and both GitHub advisories; the same versions were reported by Tenable before the release, and CERT-EU and Tenable state CVSS 9.2 ([source](https://www.tenable.com/blog/php2shell-cve-2026-109618-cve-2026-109619-frequently-asked-questions-about-critical-php-remote), [source](https://cert.europa.eu/publications/security-advisories/2026-018/)) |
| CVE IDs CVE-2026-109618 / -109619 | **High**: named by php.net, the GitHub advisories, CERT-EU and Ubuntu; NVD still had no record at 9:50 AM ET |
| Affected SAPIs and mod_php highest risk | **Medium** by the two-source rule: one publisher, the PHP project, which is the authority; FPM exploit for CVE-2026-109618 not demonstrated, per PHP |
| Supported branches and EOL dates | **Medium** by the two-source rule: php.net is the only source, and it is the authority |
| Interim ini measures | **Medium** that PHP recommends them (one publisher, the PHP project); **Low** as a fix, by PHP's own wording |
| Active exploitation in the wild | **Low to Medium**: Patchstack and Cloudflare reported early attacks, but Patchstack's newest entry and NCSC-NL report probing only and no indication of active exploitation; no primary evidence published |
| No distro packages yet | **High** for the time checked (9:38 to 9:41 AM ET); changes fast |

## Open Questions

- When Ubuntu, Debian, RHEL-family distributions, the Ondrej PPA, the Docker `php` image, Plesk, cPanel and CloudLinux ship fixed builds.
- Whether exploitation in the wild is confirmed with primary evidence, and when CISA adds either CVE to KEV; neither was listed in catalog version 2026.10.20.
- Why PHP released before the 10:00 AM ET slot; its pre-announcement reserved early release for public details or active exploitation, and the advisory gives no reason.

## Sources

**Official**
- [php.net: Security advisory, Critical remote code execution vulnerabilities](https://www.php.net/archive/2026.php#2026-10-21-5)
- [php.net home page (releases and pre-announcement)](https://www.php.net/)
- [php.net: Supported Versions](https://www.php.net/supported-versions.php)
- [GHSA-w8hw-w9fx-j79w: RCE via OOM corruption (CVE-2026-109618)](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)
- [GHSA-6wmf-4rv5-p3rp: RCE via stale global state (CVE-2026-109619)](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp)
- [php-src backport branch, PHP 7.4](https://github.com/php/php-src/tree/PHP-7.4-CVE-2026-109618-109619)
- [php-src backport branch, PHP 8.0](https://github.com/php/php-src/tree/PHP-8.0-CVE-2026-109618-109619)
- [php-src backport branch, PHP 8.1](https://github.com/php/php-src/tree/PHP-8.1-CVE-2026-109618-109619)
- [oss-security relay, 2026-10-20](https://www.openwall.com/lists/oss-security/2026/10/20/34)

**Government CERT**
- [CERT-EU Security Advisory 2026-018](https://cert.europa.eu/publications/security-advisories/2026-018/)
- [NCSC-NL: php2shell](https://github.com/NCSC-NL/php2shell)

**Security vendors**
- [Sansec: PHP2Shell](https://sansec.io/research/php2shell)
- [Patchstack: PHP major vulnerability alert](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)
- [Tenable FAQ](https://www.tenable.com/blog/php2shell-cve-2026-109618-cve-2026-109619-frequently-asked-questions-about-critical-php-remote)
- [Cloudflare WAF emergency release, Managed Ruleset, 2026-10-21](https://developers.cloudflare.com/changelog/post/2026-10-21-emergency-waf-release-cfmr/)
- [Cloudflare WAF emergency release, Free Ruleset, 2026-10-21](https://developers.cloudflare.com/changelog/post/2026-10-21-emergency-waf-release-free/)

**Package sources**
- [Ubuntu: CVE-2026-109618](https://ubuntu.com/security/CVE-2026-109618)
- [Ubuntu: CVE-2026-109619](https://ubuntu.com/security/CVE-2026-109619)
- [Ubuntu security notices](https://ubuntu.com/security/notices)
- [Debian security tracker: CVE-2026-109618](https://security-tracker.debian.org/tracker/CVE-2026-109618)
- [Debian security advisories](https://www.debian.org/security/)
- [Red Hat security data: CVE-2026-109618](https://access.redhat.com/hydra/rest/securitydata/cve/CVE-2026-109618.json)
- [Ondrej Sury PHP PPA](https://launchpad.net/~ondrej/+archive/ubuntu/php)
- [Docker official php image](https://hub.docker.com/_/php)
- [Remi repository issue #351](https://github.com/remicollet/remirepo/issues/351)

**CVE databases and threat feeds**
- [CISA Known Exploited Vulnerabilities catalog (JSON feed)](https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json)
- [NVD API, CVE-2026-109618](https://services.nvd.nist.gov/rest/json/cves/2.0?cveId=CVE-2026-109618)
- [CVE.org API, CVE-2026-109618](https://cveawg.mitre.org/api/cve/CVE-2026-109618)

## Update History

| Time (ET) | Report change |
|---|---|
| 2026-10-21 9:53 AM | Verification fixes: timestamps corrected to the real check and publish times, Patchstack exploitation wording narrowed, confidence rows given second sources or relabelled Medium (single publisher). |
| 2026-10-21 9:53 AM | **DEEP PASS DONE 9:53 AM ET.** Fixed versions, both GitHub advisories, affected SAPIs, official interim measures and EOL source backports added from php.net and the advisories. Package sources rechecked (none fixed yet). Exploitation status updated from NCSC-NL and Patchstack's newest entry. Threat intel and KEV rechecked (no hits); dark web search attempted, unavailable. |
| 2026-10-21 9:46 AM | **Release landed** (headline update): fixed versions, release time and CVE naming from php.net. Details to follow. |
| 2026-10-21 9:21 AM | Verification fixes: IOC control narrowed (ThreatFox and OTX unproven), both Ubuntu pages cited, Debian and Red Hat check times shown, timeline order. |
| 2026-10-21 9:19 AM | Ubuntu CVE pages published (Critical, no fixes yet). |
| 2026-10-21 9:17 AM | Threat intel added: CISA KEV checked directly (not listed), feed search for indicators (none), dark web search attempted (search engine unavailable). |
| 2026-10-21 8:58 AM | Corrections from cold verification: CERT-EU release date, Patchstack timeline time, KEV claim removed, PHP2Shell video and public code, confidence sourcing, CVE database citations. |
| 2026-10-21 8:49 AM | First edition, before the release. |
