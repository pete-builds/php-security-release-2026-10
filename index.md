---
title: "PHP critical security release, October 2026 (CVE-2026-109618, CVE-2026-109619)"
date: 2026-10-21
updated: 2026-10-21 8:16 PM ET
summary: "PHP released 8.5.12, 8.4.27, 8.3.36 and 8.2.35 on 21 Oct 2026, ahead of the 10:00 AM ET slot, fixing two critical (CVSS 4.0 9.2) unauthenticated remote code execution bugs. By 4:24 PM ET Ubuntu, Debian, the Ondrej PPA, the Docker php image and Plesk had fixed builds. By 5:37 PM ET Red Hat had issued errata, AlmaLinux had moved fixed packages to production, and CloudLinux had released patched ea-php; fixed alt-php for CloudLinux 7 to 9 followed at 6:29 PM ET from a separate repository, and by 6:56 PM ET CloudLinux 10 had fixed alt-php 5.3 to 8.5."
---

# PHP critical security release, October 2026

**Status at 8:16 PM ET, 21 Oct 2026: fixes RELEASED, packages arriving. An unconfirmed Fediverse claim of a further bug in 8.5.12 was reportedly dismissed by PHP as "not a real issue" (secondhand).** PHP 8.5.12, 8.4.27, 8.3.36 and 8.2.35 are out. Ubuntu, Debian, the Ondrej PPA, the Docker `php` image and Plesk now ship fixed builds; CloudLinux has released patched ea-php, fixed alt-php 5.3 to 8.5 for CloudLinux 10, and fixed alt-php for CloudLinux 7, 8 and 9 through a separate ELS repository while its own repositories catch up; Red Hat has issued errata for RHEL 8, 9 and 10 (two streams still pending) and AlmaLinux 8, 9 and 10 have fixed packages in production. Neither CVE is in CISA KEV. This page is re-checked every 30 minutes for two days.

## TL;DR

PHP released 8.5.12, 8.4.27, 8.3.36 and 8.2.35 on 21 October 2026 to fix two critical vulnerabilities that "can allow unauthenticated remote code execution", and urges every user to update immediately ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). Every PHP installation that serves web requests is at risk, including PHP-FPM, Apache mod_php, php-cgi, FrankenPHP and LiteSpeed, with the highest risk on mod_php ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). Install your package source's fixed build the moment it ships and restart PHP; the official ini workarounds only narrow specific attack paths, and php.net says "updating is the only safe choice" ([source](https://www.php.net/)).

## Current Status

- **Release: out.** Fixed versions are 8.5.12, 8.4.27, 8.3.36 and 8.2.35; both GitHub advisories list everything below those versions as affected ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w), [source](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp)). The advisories were published at 9:34 AM ET, ahead of the announced 10:00 AM ET slot; php.net gives no reason for the early release.
- **Severity:** both CVEs rated Critical, CVSS v4.0 9.2, vector `CVSS:4.0/AV:N/AC:H/AT:P/PR:N/UI:N/VC:H/VI:H/VA:H/SC:N/SI:N/SA:N` ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)).
- **Packages: most major sources have shipped.** Ubuntu published USN-8948-1 (26.10, 26.04 LTS, 24.04 LTS, 22.04 LTS) and USN-8948-2 (20.04 LTS and older) ([source](https://ubuntu.com/security/notices)); Debian marks bookworm and trixie fixed via DLA-4848-1 and DSA-6574-1, with unstable still unfixed ([source](https://security-tracker.debian.org/tracker/CVE-2026-109618)); the Ondrej PPA, the Docker `php` image and Plesk carry 8.2.35 through 8.5.12 (see [Where Patches Will Come From](#where-patches-will-come-from)). Red Hat rates CVE-2026-109618 Important and says "Installing the fixed release is the only complete fix" ([source](https://access.redhat.com/security/cve/CVE-2026-109618)). Red Hat lists six fixed errata across RHEL 8, 9 and 10, with RHEL 8 php:8.2 and RHEL 9 php:8.4 still "Affected" ([source](https://access.redhat.com/security/cve/CVE-2026-109618), [source](https://access.redhat.com/errata/RHSA-2026:87072), [source](https://access.redhat.com/errata/RHSA-2026:87080)), and AlmaLinux says its patched PHP packages "are now rolling out to the production repositories for every PHP stream on AlmaLinux 8, 9, and 10" ([source](https://almalinux.org/blog/2026-10-21-php2shell-cve-2026-109618-cve-2026-109619/)).
- **Exploitation:** Patchstack's newest entry still says its telemetry shows "only probing attacks, with no hard evidence of breaches yet" ([source](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)). CERT-EU's v1.1 says "initial proofs of concept (PoCs) have begun to circulate" ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)). Tenable reports that "watchTowr says it has reproduced both vulnerabilities and is observing exploitation attempts against both", and quotes the PHP Foundation that the PHP security team "did not identify any functional exploits at the time of release/public disclosure" among the leaked PoCs ([source](https://www.tenable.com/blog/php2shell-cve-2026-109618-cve-2026-109619-frequently-asked-questions-about-critical-php-remote)). CISA's SSVC entry in NVD records exploitation "none" as of 10:34 AM ET ([source](https://services.nvd.nist.gov/rest/json/cves/2.0?cveId=CVE-2026-109618)).
- **KEV:** neither CVE in the CISA KEV catalog (version 2026.10.21, released 10:04 AM ET, checked 2:53 PM ET) ([source](https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json)).
- **NVD:** both CVEs published 10:17 AM ET; NVD's own CVSS 3.1 score is 9.8 Critical, alongside PHP's CVSS 4.0 9.2 ([source](https://services.nvd.nist.gov/rest/json/cves/2.0?cveId=CVE-2026-109618), [source](https://services.nvd.nist.gov/rest/json/cves/2.0?cveId=CVE-2026-109619)).
- **Unconfirmed, and reportedly dismissed: a further bug in 8.5.12.** A Fediverse user posting as @elise@katze.sh wrote "still works on 8.5.12!" at 10:32 AM ET about an issue they had earlier called "another one", not the same vulnerability, and "confirmed working on 8.5.10 too" ([source](https://fedi.katze.sh/@elise/117479383739450605), [source](https://fedi.katze.sh/@elise/117478655339557825)); at 10:51 AM ET they wrote "and, reported! 3 line PoC" ([source](https://fedi.katze.sh/@elise/117479459565085083)). An oss-security reply at 11:24 AM ET said "the fix to at least PHP 8.5.12 may be incomplete", and one at 11:41 AM ET relayed that it "seems like a *different* vulnerability" ([source](https://www.openwall.com/lists/oss-security/2026/10/21/16), [source](https://www.openwall.com/lists/oss-security/2026/10/21/17)). At 1:29 PM ET the same oss-security poster relayed, from a Matrix conversation, that Canonical asked PHP and was told it is "unrelated and not a real issue", and asked the PHP security team to reconfirm on the list ([source](https://www.openwall.com/lists/oss-security/2026/10/21/23)). No CVE or impact assessment is public, and php.net and both GitHub advisories say nothing about a further bug. PHP's advice to update to the released versions stands.
- **Unresolved:** Red Hat fixes for RHEL 8 php:8.2 and RHEL 9 php:8.4, fixed alt-php in CloudLinux's own repositories for CloudLinux 7 to 9, alt-php 5.2 on CloudLinux 10, CloudLinux 6, and its native php packages other than 8.2 and 8.3 on CloudLinux 9 and 8.2 on CloudLinux 8, Plesk's panel PHP (sw-engine) fix for Plesk Obsidian 18.0.80, and whether exploitation in the wild is confirmed.

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

1. **Inventory every PHP runtime you serve to the internet**, including control panels and admin tools that bundle their own PHP. CERT-EU recommends consulting NCSC-NL's repository "to identify third-party software that embeds PHP" ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)), and NCSC-NL says to "Check third-party images, appliances and embedded runtimes separately" ([source](https://github.com/NCSC-NL/php2shell)).
2. **Update to 8.5.12, 8.4.27, 8.3.36 or 8.2.35 as soon as your package source ships it.** php.net says "Linux distributions and LTS vendors received the patches in advance, so check your vendor for updated packages" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). Restart PHP-FPM or the web server afterwards so the new binary is actually running (general practice, not from the advisories).
3. **If you use Remi's repository,** php.net urges switching "to distribution packages or build from source, so you can immediately apply updates" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)).
4. **Until you can update, apply the official interim measures** in [Interim mitigations](#interim-mitigations), and remember they "do not make an installation safe" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)).
5. **Plan off end-of-life PHP.** See [End-of-life PHP](#end-of-life-php).
6. **Do not run leaked exploit code** to test yourself: Tenable warns unverified exploit code is sometimes laced with malware ([source](https://www.tenable.com/blog/php2shell-cve-2026-109618-cve-2026-109619-frequently-asked-questions-about-critical-php-remote)).

## What Happened

On 20 October 2026 php.net announced the release, scheduled for Wednesday 21 October at 14:00 UTC, addressing "two critical vulnerabilities, both rated CVSS v4.0 9.2". It said "Full details, affected versions, and the fixed versions will be published with the release," and reserved the right to release earlier "should details of either vulnerability become public, or should evidence of active exploitation appear in the wild" ([source](https://www.php.net/)). The notice was relayed to the oss-security list the same day ([source](https://www.openwall.com/lists/oss-security/2026/10/20/34)).

Sansec reports that details about the embargoed bugs leaked on 20 October, that watchTowr confirmed they are remote code execution vulnerabilities tracked as CVE-2026-109618 and CVE-2026-109619, and that a screenshot of an exploit tool named "PHP2Shell", credited to Calif.IO, leaked alongside ([source](https://sansec.io/research/php2shell)). CERT-EU's Advisory 2026-018 now carries a release date of 21-10-2026 15:20:18 (time zone not stated) and a history of "v1.0 -- Initial publication" and v1.1 updates, both dated 21/10/2026, with full technical details ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)).

On 21 October PHP published the two GitHub security advisories at 9:34 AM ET ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w), [source](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp)) and its security advisory, "Critical remote code execution vulnerabilities, update now", together with the four releases ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). Both were visible on php.net by 9:37 AM ET, before the announced 10:00 AM ET slot.

## Technical Analysis

The descriptions below come from php.net's advisory and the two GitHub advisories, which the advisory says "have the full technical details" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)).

### The two vulnerabilities

- **CVE-2026-109618, "RCE via OOM corruption" (GHSA-w8hw-w9fx-j79w):** "out-of-memory handling can interrupt PHP while internal request data is only partially initialized. Request cleanup may then treat the invalid data as valid" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). The advisory reports: "Complete remote code execution was validated over HTTP against mod_php with default PHP input and memory limits and normal prefork configurations with multiple workers" ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)).
- **CVE-2026-109619, "RCE via stale global state" (GHSA-6wmf-4rv5-p3rp):** "request-owned internal state can survive after its backing memory has been freed. A later request handled by the same worker may use that stale state before normal initialization resets it" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). "Session upload progress is enabled by default, and no application call to session_start() is required"; "Session lifecycle RCE was validated against mod_php and FPM" ([source](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp)).

### Who is affected

- php.net: "all PHP installations serving web requests, including PHP-FPM, Apache with mod_php, php-cgi, FrankenPHP and LiteSpeed. We consider the risk highest for mod_php, with a demonstrated exploit where an unauthenticated request can trigger the attack before any application code runs" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)).
- For CVE-2026-109618 on FPM, the advisory says source analysis "supports FPM as a susceptible" target, "though no complete FPM exploit has been demonstrated for this SAPI" ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)).
- Supported branches: versions below 8.2.35, 8.3.36, 8.4.27 and 8.5.12 are listed as affected ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)). Older branches: Wiz Research says "Every PHP version released in the last decade is affected by at least one of these vulnerabilities, going back to version 5.4" ([source](https://www.wiz.io/blog/php2shell-php-rce)), and PHP published source backports for 7.4, 8.0 and 8.1 (see [End-of-life PHP](#end-of-life-php)).
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

A pre-release vendor suggestion that is not on PHP's list: blocking `multipart/form-data` at the web server (Sansec, breaks uploads) ([source](https://sansec.io/research/php2shell)). Cloudflare's changelog lists emergency Managed Ruleset and Free Ruleset releases dated 21 October that detect or block "anomalous request patterns" without naming PHP or either CVE ([source](https://developers.cloudflare.com/changelog/post/2026-10-21-emergency-waf-release-cfmr/), [source](https://developers.cloudflare.com/changelog/post/2026-10-21-emergency-waf-release-free/)).

### End-of-life PHP

php.net lists security support ending 31 Dec 2026 for 8.2, 31 Dec 2027 for 8.3, 31 Dec 2028 for 8.4 and 31 Dec 2029 for 8.5; older branches are end of life ([source](https://www.php.net/supported-versions.php)). "PHP 8.1 and older no longer receive updates from the PHP project", but "the PHP security team provided minimal source-backports for PHP 7.4, PHP 8.0, and PHP 8.1 to help people building from source or without access to an LTS vendor" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)). They are source branches, not packaged releases: [PHP 7.4](https://github.com/php/php-src/tree/PHP-7.4-CVE-2026-109618-109619), [PHP 8.0](https://github.com/php/php-src/tree/PHP-8.0-CVE-2026-109618-109619), [PHP 8.1](https://github.com/php/php-src/tree/PHP-8.1-CVE-2026-109618-109619).

## Where Patches Will Come From

Most servers do not install PHP from php.net. The fix reaches you through your package source, and each moves on its own clock.

| Source | Status at last check |
|---|---|
| [php.net](https://www.php.net/) | **Released:** 8.5.12, 8.4.27, 8.3.36, 8.2.35 (9:37 AM ET); the ChangeLog for each lists "Fixed GHSA-w8hw-w9fx-j79w, GHSA-6wmf-4rv5-p3rp (Avoid state corruption via bailout)" ([source](https://www.php.net/ChangeLog-8.php)) |
| Ubuntu | **Fixed:** USN-8948-1 for 26.10, 26.04 LTS, 24.04 LTS and 22.04 LTS, USN-8948-2 for 20.04 LTS and older ([source](https://ubuntu.com/security/notices)). Fixed in, for example, php8.3 8.3.6-0ubuntu0.24.04.13 on 24.04 and php8.1 8.1.2-1ubuntu2.28 on 22.04; 20.04's php7.4 fix needs Ubuntu Pro ([source](https://ubuntu.com/security/CVE-2026-109618)) (2:53 PM ET) |
| [Debian security tracker](https://security-tracker.debian.org/tracker/CVE-2026-109618) | **Fixed in stable releases:** php8.2 8.2.35-1~deb12u1 for bookworm (DLA-4848-1), php8.4 8.4.27-1~deb13u1 for trixie (DSA-6574-1); forky and sid still vulnerable (2:53 PM ET) |
| [Red Hat](https://access.redhat.com/security/cve/CVE-2026-109618) | **Mostly fixed:** the CVE page lists six fixed errata: RHEL 10 `php` (RHSA-2026:87070, 4:11 PM ET) and `php8.4` (RHSA-2026:87071, 4:10 PM ET); RHEL 9 `php` (RHSA-2026:87151, 4:06 PM ET), `php:8.2` (RHSA-2026:87072, 4:35 PM ET) and `php:8.3` (RHSA-2026:87079, 4:36 PM ET); RHEL 8 `php:7.4` (RHSA-2026:87080, dated 11:53 AM ET, though it was not on this page at the 2:53 PM ET check). Still "Affected" with no erratum: RHEL 8 `php:8.2`, RHEL 9 `php:8.4`, Red Hat Hardened Images `php` and Dev Spaces `udi-rhel9`; RHEL 6 and 7 are "Not affected". Both RHSAs opened directly list both CVEs ([source](https://access.redhat.com/errata/RHSA-2026:87072), [source](https://access.redhat.com/errata/RHSA-2026:87080)). Red Hat rates it **Important**, CVSS 3.1 8.1, and says standard RHEL deployments use PHP-FPM, where RCE "is difficult in practice" (checked 5:45 PM ET) |
| [AlmaLinux](https://almalinux.org/blog/2026-10-21-php2shell-cve-2026-109618-cve-2026-109619/) | **Fixed:** "2026-10-21 21:15 UTC: The patched PHP packages are now rolling out to the production repositories for every PHP stream on AlmaLinux 8, 9, and 10" (5:15 PM ET); AlmaLinux says to run `sudo dnf clean metadata && sudo dnf upgrade 'php*'`, restart php-fpm (or httpd for mod_php on AlmaLinux 8), and notes "Most mirrors sync every few hours". Its errata feeds did not yet list either CVE at 5:33 PM ET |
| [Ondrej Sury PPA](https://launchpad.net/~ondrej/+archive/ubuntu/php) | **Fixed:** 8.2.35, 8.3.36, 8.4.27 and 8.5.12 published for Ubuntu 22.04 and 24.04 (2:53 PM ET) |
| [Docker official `php` image](https://hub.docker.com/_/php) | **Fixed:** 8.2.35, 8.3.36, 8.4.27 and 8.5.12 tags present; Docker Hub's last-push times run from 12:45 PM ET, with some 8.5.12 tags re-pushed as late as 3:00 PM ET |
| Remi's repository (RHEL family) | php.net said on release day "The repo will not have updates today" ([source](https://www.php.net/archive/2026.php#2026-10-21-5)); at 10:21 AM ET the maintainer replied that updates are planned for "Fedora 43+, RHEL 8+", not EL7, with a delay of "<1h for today set", and closed the issue ([source](https://github.com/remicollet/remirepo/issues/351)). Package availability not checked |
| [Plesk](https://support.plesk.com/hc/en-us/articles/44397729587351-Vulnerability-CVE-2026-109618-and-CVE-2026-109619-Remote-code-execution-in-PHP-shipped-with-Plesk) | **Fixed** for Linux and Windows PHP 8.2 to 8.5; EOL builds 7.4.33, 8.0.30 and 8.1.34 keep their version numbers, and "Versions built on 21 October 2026 have fixes included". The panel's own PHP (sw-engine) is now fixed: "Fixes for sw-engine (PHP engine used by Plesk itself) are already available in Plesk Obsidian 18.0.81 Update 4 for both Plesk for Linux and Plesk for Windows", while "The fix for sw-engine in Plesk Obsidian 18.0.80 is still pending", listed as "Planned as hotfix 18.0.80 Update 11" (article edited 8:00 PM ET) |
| [CloudLinux](https://cloudlinux.statuspage.io/incidents/zl0vt1yq3dj5) | **Status at 6:56 PM ET:** ea-php52 through ea-php85 are fixed for CloudLinux 7, 8, 9 and 10. On CloudLinux 7, 8 and 9, "fixed alt-php 7.0-7.4 are being released to the beta repository, and the remaining versions will follow", and until then fixed alt-php comes from the ELS ALT PHP repository. On CloudLinux 10, per the blog's 7:00 PM ET update, "a regular update installs fixed alt-php 5.3 to 5.6, 7.0 to 7.4 and 8.0 to 8.5 from the TuxCare ELS alt-php repository", and alt-php 5.2 there "is being released to stable" (CloudLinux edited the status update at 8:04 PM ET and it no longer lists the CloudLinux 10 versions). On CloudLinux for Ubuntu 22.04, fixed alt-php 5.2, 5.6, 7.0-7.4 and 8.0-8.5 are in stable. CloudLinux confirms its packages "carry the fix as a backport, so their version numbers stay below PHP's", and that advisory metadata is still unpublished, so `yum --security` will not list them ([source](https://cloudlinux.statuspage.io/incidents/zl0vt1yq3dj5), [source](https://blog.cloudlinux.com/php-cve-2026-109618-and-cve-2026-109619)). **Fixed alt-php for CloudLinux 7 to 9, from a separate repository (6:29 PM ET):** "Fixed alt-php packages for CloudLinux OS 7, 8 and 9 are available now in the ELS ALT PHP repository." Servers must be registered with CLN, add it with `els-php-release` (`yum install -y els-php-release` on CloudLinux 7, `dnf install -y els-php-release` on 8 and 9), then update `alt-php*`; "CloudLinux OS 10 already receives alt-php from this repository and only needs a regular update". Restart Apache, PHP-FPM or LiteSpeed afterwards, and confirm with a changelog check such as `rpm -q --changelog alt-php82-common | grep -E 'CVE-2026-(109618|109619)'` ([source](https://cloudlinux.statuspage.io/incidents/zl0vt1yq3dj5)). Earlier, at 5:59 PM ET, CloudLinux corrected its 3:00 PM ET (19:00 UTC) blog update, which had announced the first fixed alt-php packages: "Those packages went to the TuxCare ELS alt-php repository. CloudLinux 7, 8 and 9 servers install alt-php from the CloudLinux repositories, where the fixed packages are not available yet." On CloudLinux 10 it then said only "fixed alt-php 7.4 is confirmed there" (widened at 6:56 PM ET, see above); CloudLinux 6 was unconfirmed ([source](https://cloudlinux.statuspage.io/incidents/zl0vt1yq3dj5), [source](https://blog.cloudlinux.com/php-cve-2026-109618-and-cve-2026-109619)). Its 4:18 PM ET status update had listed fixed alt-php for many platforms and timed the rest for 5:15 and 6:30 PM ET; the correction supersedes that list for CloudLinux 6 to 10, CloudLinux has since confirmed fixed alt-php in stable on CloudLinux for Ubuntu 22.04 (see 6:56 PM) but has not said whether its Debian, Alpine, AlmaLinux or other Ubuntu entries stand. CloudLinux's blog said at 6:30 PM ET (22:30 UTC) that its native `php` packages (not alt-php or ea-php) "are in stable for PHP 8.2 and 8.3 on CloudLinux 9, and for PHP 8.2 on CloudLinux 8"; other versions are "Fixed builds in testing or early rollout, not generally available yet" ([source](https://blog.cloudlinux.com/php-cve-2026-109618-and-cve-2026-109619)). The blog added the ELS route for CloudLinux 7 to 9 at 6:45 PM ET (22:45 UTC). TuxCare ELS/ESU distribution PHP is fixed for RHEL 7, Oracle Linux 6 and 7, CentOS 8.4 and 8.5, Amazon Linux 2 and others. At 4:55 PM ET CloudLinux said "Patched ea-php packages have been released to the stable repository for all versions from ea-php52 through ea-php85". The fixed packages keep upstream version numbers below the fixed releases (for example ea-php84 `8.4.26-2.elX.cloudlinux.1`, ea-php85 `8.5.11-2.elX.cloudlinux.1`), so a version check alone will not show the fix; CloudLinux says "To confirm a version is fixed, check its PHP package, for example: `rpm -q ea-php71-php-cli --changelog | grep -E 'CVE-2026-(109618|109619)'`", then restart Apache and PHP-FPM. Imunify360 ModSecurity ruleset 8.84 blocks the known exploit pattern, and ruleset 8.85 (1:17 PM ET) "blocks further variants of the exploit tool" ([source](https://cloudlinux.statuspage.io/incidents/zl0vt1yq3dj5)) |
| cPanel | No first-party cPanel advisory checked. CloudLinux's status page said at 10:55 AM ET that patched ea-php74 through ea-php85 "are scheduled for release in EasyApache4 v25.91", and at 4:55 PM ET that its own ea-php builds (release suffix `.cloudlinux`) for ea-php52 through ea-php85 are in its stable repository (see the CloudLinux row) ([source](https://cloudlinux.statuspage.io/incidents/zl0vt1yq3dj5)) |

## Timeline

| Time (ET) | Event |
|---|---|
| 2026-10-21 8:00 PM | Plesk: panel PHP (sw-engine) fixed in Plesk Obsidian 18.0.81 Update 4 for Linux and Windows; 18.0.80 still pending ([source](https://support.plesk.com/hc/en-us/articles/44397729587351-Vulnerability-CVE-2026-109618-and-CVE-2026-109619-Remote-code-execution-in-PHP-shipped-with-Plesk)) |
| 2026-10-21 6:56 PM | CloudLinux status: ea-php fixed for CloudLinux 7 to 10; fixed alt-php 5.3 to 8.5 on CloudLinux 10, and 5.2, 5.6, 7.0 to 7.4 and 8.0 to 8.5 in stable on CloudLinux for Ubuntu 22.04; alt-php 7.0 to 7.4 heading to beta on CloudLinux 7 to 9 ([source](https://cloudlinux.statuspage.io/incidents/zl0vt1yq3dj5), [source](https://blog.cloudlinux.com/php-cve-2026-109618-and-cve-2026-109619)) |
| 2026-10-21 6:30 PM | CloudLinux blog: native php fixed in stable for 8.2 and 8.3 on CloudLinux 9 and 8.2 on CloudLinux 8 ([source](https://blog.cloudlinux.com/php-cve-2026-109618-and-cve-2026-109619)) |
| 2026-10-21 6:29 PM | CloudLinux: fixed alt-php for CloudLinux 7, 8 and 9 available in the ELS ALT PHP repository, which servers must add; CloudLinux 10 needs only a regular update ([source](https://cloudlinux.statuspage.io/incidents/zl0vt1yq3dj5)) |
| 2026-10-21 5:59 PM | CloudLinux correction: its earlier alt-php fixes went to the TuxCare ELS repository; CloudLinux 7, 8 and 9 have no fixed alt-php yet; CloudLinux 10 has fixed alt-php 7.4 ([source](https://cloudlinux.statuspage.io/incidents/zl0vt1yq3dj5), [source](https://blog.cloudlinux.com/php-cve-2026-109618-and-cve-2026-109619)) |
| 2026-10-21 5:15 PM | AlmaLinux: patched PHP for every stream on AlmaLinux 8, 9 and 10 rolling out to production repositories ([source](https://almalinux.org/blog/2026-10-21-php2shell-cve-2026-109618-cve-2026-109619/)) |
| 2026-10-21 4:55 PM | CloudLinux releases patched ea-php52 through ea-php85 to its stable repository, with upstream version numbers below the fixed releases ([source](https://cloudlinux.statuspage.io/incidents/zl0vt1yq3dj5)) |
| 2026-10-21 4:06 to 4:36 PM | Red Hat errata for RHEL 9 and 10: RHSA-2026:87151, :87071, :87070, :87072 and :87079 ([source](https://access.redhat.com/security/cve/CVE-2026-109618)) |
| 2026-10-21 4:18 PM | CloudLinux: alt-php fixes reported in stable repos for many platforms (CloudLinux 6 to 10 entries later corrected, see 5:59 PM), remaining versions now expected by 5:15 PM ET and alt-php 8.5 by 6:30 PM ET; security-only update filters will not list them until advisory metadata is published ([source](https://cloudlinux.statuspage.io/incidents/zl0vt1yq3dj5)) |
| 2026-10-21 3:16 PM | The CVE Program's record for CVE-2026-109619, mirrored in NVD, adds the 11:24 and 11:41 AM oss-security replies as references ([source](https://services.nvd.nist.gov/rest/json/cves/2.0?cveId=CVE-2026-109619)) |
| 2026-10-21 2:53 PM | Ubuntu (USN-8948-1, -2), Debian (DSA-6574-1, DLA-4848-1), Ondrej PPA and Plesk show fixed builds; Red Hat and AlmaLinux have no erratum; neither CVE in KEV (see [Where Patches Will Come From](#where-patches-will-come-from)) |
| 2026-10-21 2:50 PM | Plesk support article updated: fixed PHP 8.2 to 8.5 for Linux and Windows, panel sw-engine fix still planned as a hotfix ([source](https://support.plesk.com/hc/en-us/articles/44397729587351-Vulnerability-CVE-2026-109618-and-CVE-2026-109619-Remote-code-execution-in-PHP-shipped-with-Plesk)) |
| 2026-10-21 1:29 PM | oss-security reply relays, secondhand from a Matrix conversation, that PHP told Canonical the 8.5.12 claim is "unrelated and not a real issue" ([source](https://www.openwall.com/lists/oss-security/2026/10/21/23)) |
| 2026-10-21 1:17 PM | CloudLinux Imunify360 WAF ruleset 8.85 rolled out, following 8.83 and 8.84 earlier in the day ([source](https://cloudlinux.statuspage.io/incidents/zl0vt1yq3dj5)) |
| 2026-10-21 12:45 PM | Earliest Docker Hub last-push time among the fixed `php` image tags (8.2.35, 8.3.36, 8.4.27, 8.5.12) ([source](https://hub.docker.com/_/php)) |
| 2026-10-21 11:53 AM | Red Hat RHSA-2026:87080 for RHEL 8 php:7.4, per the release time on its CVE page ([source](https://access.redhat.com/security/cve/CVE-2026-109618)) |
| 2026-10-21 11:24 AM | oss-security reply: "the fix to at least PHP 8.5.12 may be incomplete", citing a Fediverse post; a second reply at 11:41 AM relays that it "seems like a *different* vulnerability" ([source](https://www.openwall.com/lists/oss-security/2026/10/21/16), [source](https://www.openwall.com/lists/oss-security/2026/10/21/17)) |
| 2026-10-21 10:51 AM | @elise@katze.sh: "and, reported! 3 line PoC" ([source](https://fedi.katze.sh/@elise/117479459565085083)) |
| 2026-10-21 10:34 AM | CISA SSVC entry added in NVD: exploitation "none", automatable "no" ([source](https://services.nvd.nist.gov/rest/json/cves/2.0?cveId=CVE-2026-109618)) |
| 2026-10-21 10:32 AM | @elise@katze.sh: "still works on 8.5.12!" ([source](https://fedi.katze.sh/@elise/117479383739450605)) |
| 2026-10-21 10:21 AM | Remi repository maintainer: updates for "Fedora 43+, RHEL 8+", none for EL7 ([source](https://github.com/remicollet/remirepo/issues/351)) |
| 2026-10-21 10:17 AM | NVD publishes both CVEs; NVD scores them CVSS 3.1 9.8 ([source](https://services.nvd.nist.gov/rest/json/cves/2.0?cveId=CVE-2026-109618)) |
| 2026-10-21 10:11 AM | NCSC-NL rewrites its readme around PHP's official advisory and drops its earlier exploitation assessment ([source](https://github.com/NCSC-NL/php2shell)) |
| 2026-10-21 | Wiz Research publishes its PHP2Shell writeup ([source](https://www.wiz.io/blog/php2shell-php-rce)) |
| 2026-10-21 9:41 AM | No fixed distro, PPA or Docker builds yet (see [Where Patches Will Come From](#where-patches-will-come-from)) |
| 2026-10-21 9:37 AM | php.net front page lists the four releases and the security advisory, ahead of the 10:00 AM slot ([source](https://www.php.net/)) |
| 2026-10-21 9:34 AM | GHSA-6wmf-4rv5-p3rp and GHSA-w8hw-w9fx-j79w published ([source](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp), [source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)) |
| 2026-10-21 9:19 AM | Ubuntu CVE pages live for both CVEs, priority Critical, no fixed packages yet ([source](https://ubuntu.com/security/CVE-2026-109618)) |
| 2026-10-21 | NCSC-NL: public proof-of-concept exploits available, no indication of active exploitation ([source](https://github.com/NCSC-NL/php2shell/blob/460e876ca62875cf42aca1c622e69bf6202815cd/README.md)) |
| 2026-10-21 | Sansec analysis ([source](https://sansec.io/research/php2shell)) |
| 2026-10-21 | CERT-EU Advisory 2026-018 v1.0 published, then v1.1 with technical details (release date field reads 21-10-2026 15:20:18, zone not stated) ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)) |
| 2026-10-21 3:43 AM | Cloudflare reports early attacks against PHP sites, per Sansec ([source](https://sansec.io/research/php2shell)) |
| 2026-10-21 3:34 AM | Multiple active exploitation incidents reported to Patchstack by hosting partners, per Patchstack's timeline ([source](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)) |
| 2026-10-20 4:22 PM | Notice relayed to oss-security ([source](https://www.openwall.com/lists/oss-security/2026/10/20/34)) |
| 2026-10-20 | php.net pre-announcement ([source](https://www.php.net/)); details leak, per Sansec ([source](https://sansec.io/research/php2shell)) |

## Indicators of Compromise

No official source has published indicators of compromise as of 2:53 PM ET, and no vendor page reviewed lists IPs, file names or webshells. For detection, the CVE-2026-109618 advisory names multipart requests "without a usable declared content length" and multipart parts with "an unrestricted number of headers" as out-of-memory vectors ([source](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)). For CVE-2026-109619 the advisory warns that "absence of an error in logs is not proof of safety" ([source](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp)).

Wiz Research, which reported the bugs, says defenders "should pay particular attention to unusual multipart requests followed by memory-limit errors or repeated HTTP 500 responses, unexpectedly large or malformed PHPSESSID response cookies, and unexplained PHP worker churn" ([source](https://www.wiz.io/blog/php2shell-php-rce)). Wiz also demonstrated reading "passwords, tokens, and API keys" from PHP process memory without code execution ([source](https://www.wiz.io/blog/php2shell-php-rce)), and Patchstack advises treating a server showing these signs as potentially compromised and considering rotating the credentials it holds ([source](https://patchstack.com/articles/php-major-vulnerability-alert-10-2026/)). These are behavioural signs, not indicators of compromise.

A search at 2:53 PM ET of abuse.ch ThreatFox, URLhaus and MalwareBazaar, AlienVault OTX and the CISA KEV catalog, synced 2:26 PM ET, returned no indicators, pulses or entries for PHP2Shell, CVE-2026-109618, CVE-2026-109619 or Calif.io. A control search returned 50 indicators for AgentTesla, so the search works; the OTX feed held no records in this index, so it adds no evidence either way.

## Dark Web and Threat Actor Footprint

Dark web index search (Ahmia) was attempted again at 2:53 PM ET and was unavailable: the search engine redirected the query, so no dark web result exists for this edition, positive or negative. The only actor-adjacent detail found is the "PHP2Shell" exploit: Sansec describes a leaked screenshot of the tool credited to Calif.IO ([source](https://sansec.io/research/php2shell)), and Tenable reports a video by researcher Kirill Firsov demonstrating the exploit chain, credited to LCFR and Calif.io, and says code claiming to exploit the bugs is circulating publicly but unverified ([source](https://www.tenable.com/blog/php2shell-cve-2026-109618-cve-2026-109619-frequently-asked-questions-about-critical-php-remote)). NCSC-NL's readme stated that "Multiple public proof-of-concept exploits are already available" until its 10:11 AM ET rewrite ([source](https://github.com/NCSC-NL/php2shell/blob/460e876ca62875cf42aca1c622e69bf6202815cd/README.md)). Wiz Research says it combined the bugs into one exploit script, php2shell, and credits "the PHP team, Calif and OpenAI" for discovery, coordination and patch verification ([source](https://www.wiz.io/blog/php2shell-php-rce)).

## Confidence Assessment

| Claim | Confidence |
|---|---|
| Fixed versions 8.5.12 / 8.4.27 / 8.3.36 / 8.2.35, CVSS 9.2 x2 | **High**: php.net advisory and both GitHub advisories; CERT-EU independently states the same fixed versions and CVSS 9.2 ([source](https://cert.europa.eu/publications/security-advisories/2026-018/)) |
| CVE IDs CVE-2026-109618 / -109619 | **High**: named by php.net, the GitHub advisories, CERT-EU, Ubuntu and NVD, which published both at 10:17 AM ET |
| Affected SAPIs and mod_php highest risk | **Medium** by the two-source rule: one publisher, the PHP project, which is the authority; FPM exploit for CVE-2026-109618 not demonstrated, per PHP |
| Supported branches and EOL dates | **Medium** by the two-source rule: php.net is the only source, and it is the authority |
| Interim ini measures | **Medium** that PHP recommends them (one publisher, the PHP project); **Low** as a fix, by PHP's own wording |
| Active exploitation in the wild | **Low to Medium**: Patchstack and Cloudflare reported early attacks, but Patchstack's newest entry reports probing only, CISA's SSVC entry records exploitation "none", and neither CVE is in KEV; no primary evidence published |
| A further, separate PHP bug works on 8.5.12 | **Low**: one Fediverse user's own posts, relayed but not confirmed on oss-security; no CVE; a secondhand oss-security relay says PHP called it "unrelated and not a real issue", itself unconfirmed by PHP on the list |
| Fixed packages from Ubuntu, Debian, Ondrej PPA, Docker, Plesk, Red Hat (RHEL 8, 9 and 10, two streams pending) and AlmaLinux | **Medium** by the two-source rule for the time checked (2:53 to 5:45 PM ET): each vendor's status comes from one source, its own tracker, feed or registry, which is the authority for that vendor; changes fast |

## Open Questions

- When Red Hat fixes RHEL 8 php:8.2 and RHEL 9 php:8.4, when Remi ships fixed builds, when fixed alt-php reaches CloudLinux's own repositories for CloudLinux 7 to 9 and alt-php 5.2 reaches stable on CloudLinux 10, when CloudLinux's remaining native php packages leave testing, whether cPanel has published an advisory, and when Plesk's sw-engine fix for 18.0.80 lands.
- Whether the PHP security team confirms on oss-security that the issue reported against 8.5.12 is "not a real issue", as relayed secondhand.
- Whether exploitation in the wild is confirmed with primary evidence, and when CISA adds either CVE to KEV; neither was listed in catalog version 2026.10.21.
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
- [oss-security reply: 8.5.12 fix may be incomplete, 2026-10-21](https://www.openwall.com/lists/oss-security/2026/10/21/16)
- [oss-security reply: possibly a different vulnerability, 2026-10-21](https://www.openwall.com/lists/oss-security/2026/10/21/17)
- [oss-security reply: PHP reportedly says "not a real issue", 2026-10-21](https://www.openwall.com/lists/oss-security/2026/10/21/23)

**Fediverse posts (unconfirmed)**
- [@elise@katze.sh: "still works on 8.5.12!"](https://fedi.katze.sh/@elise/117479383739450605)
- [@elise@katze.sh: "this is another one"](https://fedi.katze.sh/@elise/117478655339557825)
- [@elise@katze.sh: "and, reported!"](https://fedi.katze.sh/@elise/117479459565085083)

**Government CERT**
- [CERT-EU Security Advisory 2026-018](https://cert.europa.eu/publications/security-advisories/2026-018/)
- [NCSC-NL: php2shell](https://github.com/NCSC-NL/php2shell)

**Security vendors**
- [NCSC-NL php2shell readme, revision of 21 Oct 2026 10:03 AM ET](https://github.com/NCSC-NL/php2shell/blob/460e876ca62875cf42aca1c622e69bf6202815cd/README.md)
- [Wiz Research: PHP2Shell](https://www.wiz.io/blog/php2shell-php-rce)
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
- [Debian security tracker: CVE-2026-109619](https://security-tracker.debian.org/tracker/CVE-2026-109619)
- [Red Hat: CVE-2026-109618](https://access.redhat.com/security/cve/CVE-2026-109618)
- [Red Hat security data: CVE-2026-109618](https://access.redhat.com/hydra/rest/securitydata/cve/CVE-2026-109618.json)
- [AlmaLinux errata](https://errata.almalinux.org/)
- [AlmaLinux blog: php2shell patched packages](https://almalinux.org/blog/2026-10-21-php2shell-cve-2026-109618-cve-2026-109619/)
- [Red Hat RHSA-2026:87072 (php:8.2, RHEL 9)](https://access.redhat.com/errata/RHSA-2026:87072)
- [Red Hat RHSA-2026:87080 (php:7.4, RHEL 8)](https://access.redhat.com/errata/RHSA-2026:87080)
- [php.net ChangeLog for PHP 8](https://www.php.net/ChangeLog-8.php)
- [Ondrej Sury PHP PPA](https://launchpad.net/~ondrej/+archive/ubuntu/php)
- [Docker official php image](https://hub.docker.com/_/php)
- [Remi repository issue #351](https://github.com/remicollet/remirepo/issues/351)
- [Plesk: CVE-2026-109618 and CVE-2026-109619 in PHP shipped with Plesk](https://support.plesk.com/hc/en-us/articles/44397729587351-Vulnerability-CVE-2026-109618-and-CVE-2026-109619-Remote-code-execution-in-PHP-shipped-with-Plesk)
- [CloudLinux: updated package availability](https://cloudlinux.zendesk.com/hc/en-us/articles/31194180517916-PHP-CVE-2026-109618-CVE-2026-109619-reported-pre-disclosure-RCEs-updated-package-availability)
- [CloudLinux status incident](https://cloudlinux.statuspage.io/incidents/zl0vt1yq3dj5)
- [CloudLinux blog: package status and Imunify360 protection](https://blog.cloudlinux.com/php-cve-2026-109618-and-cve-2026-109619)

**CVE databases and threat feeds**
- [CISA Known Exploited Vulnerabilities catalog (JSON feed)](https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json)
- [NVD API, CVE-2026-109618](https://services.nvd.nist.gov/rest/json/cves/2.0?cveId=CVE-2026-109618)
- [CVE.org API, CVE-2026-109618](https://cveawg.mitre.org/api/cve/CVE-2026-109618)

## Update History

| Time (ET) | Report change |
|---|---|
| 2026-10-21 8:16 PM | Plesk support article edited 8:00 PM ET: sw-engine fixed in 18.0.81 Update 4, 18.0.80 still pending; Plesk row, Unresolved bullet and Open Questions updated; a missing comma in the Unresolved bullet fixed. CloudLinux edited its 6:56 PM status update at 8:04 PM ET without new facts, so the CloudLinux 10 version quote now comes from its blog; Red Hat (six errata, two streams pending) and AlmaLinux unchanged; no new oss-security posts on PHP; KEV still not listed. |
| 2026-10-21 7:16 PM | CloudLinux 6:56 PM ET status update and 6:45 and 7:00 PM ET blog updates added: fixed alt-php across versions on CloudLinux 10 and Ubuntu 22.04, alt-php 7.0 to 7.4 heading to beta on CloudLinux 7 to 9, CloudLinux's own statement that its fixes are backports; the note that the blog lagged the status page replaced, since it caught up at 6:45 PM ET. Red Hat (six errata, two streams pending) and AlmaLinux unchanged; no new oss-security posts on PHP; KEV still not listed. Verification fixes: Ubuntu 22.04 version range separated from CloudLinux 10's, the superseded 5:59 PM CloudLinux 10 quote replaced with the status text it still carries, the Ubuntu-entries sentence and Unresolved/Open Questions scope updated, summary extended. |
| 2026-10-21 6:46 PM | CloudLinux 6:29 PM ET update: fixed alt-php for CloudLinux 7 to 9 via the ELS ALT PHP repository, with the steps to add it and the changelog check; banner, summary, Unresolved bullet and Open Questions updated. Red Hat (six errata, two streams pending) and AlmaLinux unchanged; no new oss-security posts on PHP; KEV still not listed. Verification fixes: CloudLinux's 6:30 PM ET blog update on native php added, the quoted "testing or early rollout" wording corrected, and the blog's lag behind the status page noted. |
| 2026-10-21 6:16 PM | Verification fixes: correction tied to CloudLinux's 3:00 PM ET blog update it names, its scope narrowed to CloudLinux 6 to 10, CloudLinux system php "in testing" added, stale "partway through" removed from the summary. |
| 2026-10-21 6:16 PM | CloudLinux correction (5:59 PM ET status page, 21:45 UTC blog): alt-php is not fixed in the CloudLinux 7, 8 and 9 repositories; the CloudLinux row, banner, summary, Unresolved bullet, Open Questions and the 4:18 PM timeline row corrected. Red Hat (six errata, two streams pending) and AlmaLinux unchanged; KEV still not listed. |
| 2026-10-21 5:37 PM | Red Hat RHSA-2026:87072 (RHEL 9, php:8.2) and RHSA-2026:87080 (RHEL 8, php:7.4) and AlmaLinux production packages (5:15 PM ET) added; found through the per-run search, since the Red Hat CVE page and AlmaLinux errata feed checked earlier showed neither. Other watch-list sources unchanged; KEV still not listed. Verification fixes: Red Hat scope widened from "at least RHEL 8 and 9" to all six errata on its CVE page (RHEL 8, 9 and 10) with release times, the two still-affected streams named, and the undated Red Hat timeline row replaced with timed rows. |
| 2026-10-21 5:05 PM | CloudLinux ea-php release (4:55 PM ET) added, with its below-upstream version numbers and the changelog check it gives for confirming a fix. Other watch-list sources unchanged; no new oss-security posts on PHP; KEV still not listed. Verification fixes: "backported" wording replaced with what the version strings show, the changelog check quoted with its `-php-cli` package example, and the cPanel row reconciled with the ea-php release. |
| 2026-10-21 4:24 PM | CloudLinux status update (4:18 PM ET): alt-php availability, revised timing (bulk 5:15 PM ET, 8.5 at 6:30 PM ET) and the security-filter caveat added; stale ea-php wording in Open Questions corrected; verification added ruleset 8.85 and refreshed the summary line. Other watch-list sources unchanged; no new oss-security posts on PHP since 1:29 PM ET; KEV still not listed. |
| 2026-10-21 3:24 PM | Verification fixes: added the 1:29 PM ET oss-security reply relaying PHP's reported "not a real issue" answer, softened the banner, cited the "reported!" post directly, called the poster a Fediverse user rather than a researcher (no such claim in the profile), and narrowed "no details public" to no CVE or impact assessment. |
| 2026-10-21 3:24 PM | Unconfirmed report of a further, separate bug that still works on 8.5.12 added (Fediverse posts and oss-security replies, surfaced when NVD added two of them as references at 3:16 PM ET), labelled Low confidence. php.net, GHSAs, distro trackers, CERT-EU, Patchstack, NCSC-NL and Remi rechecked at 3:23 PM ET with no change; KEV still not listed. |
| 2026-10-21 2:57 PM | Verification fixes: stale CERT-EU v1.0 statements (release date, "PHP 5 onwards", first recommendation) replaced with v1.1 text and Wiz's version range; stale Patchstack "loosely on multipart" and `use_strict_mode` advice removed; Docker push window corrected to start at 12:45 PM ET; CloudLinux row recited to its status page and ea-php moved to the cPanel row; Tenable's watchTowr exploitation-attempt report and the PHP Foundation's "no functional exploits" finding added; packages confidence lowered to Medium (one source per vendor); stale NVD note and unverifiable Tenable leg removed from confidence rows; timeline reordered. |
| 2026-10-21 2:57 PM | Packages: Ubuntu, Debian, Ondrej PPA, Docker and Plesk now fixed; CloudLinux in progress; Red Hat rated Important with no erratum; AlmaLinux none. NVD records and CISA SSVC added; KEV 2026.10.21 rechecked (not listed). NCSC-NL quotes repinned to the readme revision that carried them, since its 10:11 AM ET rewrite dropped them. Wiz Research writeup and detection signs added. Threat intel rechecked (no hits); dark web search attempted, unavailable. |
| 2026-10-21 9:53 AM | Verification fixes: timestamps corrected to the real check and publish times, Patchstack exploitation wording narrowed, confidence rows given second sources or relabelled Medium (single publisher). |
| 2026-10-21 9:53 AM | **DEEP PASS DONE 9:53 AM ET.** Fixed versions, both GitHub advisories, affected SAPIs, official interim measures and EOL source backports added from php.net and the advisories. Package sources rechecked (none fixed yet). Exploitation status updated from NCSC-NL and Patchstack's newest entry. Threat intel and KEV rechecked (no hits); dark web search attempted, unavailable. |
| 2026-10-21 9:46 AM | **Release landed** (headline update): fixed versions, release time and CVE naming from php.net. Details to follow. |
| 2026-10-21 9:21 AM | Verification fixes: IOC control narrowed (ThreatFox and OTX unproven), both Ubuntu pages cited, Debian and Red Hat check times shown, timeline order. |
| 2026-10-21 9:19 AM | Ubuntu CVE pages published (Critical, no fixes yet). |
| 2026-10-21 9:17 AM | Threat intel added: CISA KEV checked directly (not listed), feed search for indicators (none), dark web search attempted (search engine unavailable). |
| 2026-10-21 8:58 AM | Corrections from cold verification: CERT-EU release date, Patchstack timeline time, KEV claim removed, PHP2Shell video and public code, confidence sourcing, CVE database citations. |
| 2026-10-21 8:49 AM | First edition, before the release. |
