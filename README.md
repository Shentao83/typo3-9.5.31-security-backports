# TYPO3 9.5.31 security backports

Seven community backports of [TYPO3's November 2026 security fixes](https://get.typo3.org/release-notes/14.3.12) for the public, classic [TYPO3 9.5.31 release](https://get.typo3.org/version/9.5.31). The patches target source trees with `typo3/sysext/` and were tested on PHP 7.4.33 with SQLite and MariaDB 10.3.39. See [test coverage](TESTING.md).

This is an independent project. TYPO3 9 ELTS ended on 30 September 2025. Use these patches as a temporary measure for installations that still run this exact release, and plan an upgrade to a supported version. TYPO3's published version ranges do not list 9.5. The scope here is based on a source review of 9.5.31 and applies only to the fixes listed below. Other 9.5 builds and Composer installations with separate system extension packages require their own review and testing.

| Advisory | CVE | Backport |
| --- | --- | --- |
| [2026-026](https://news.typo3.com/security/advisory/typo3-core-sa-2026-026) | CVE-2026-119622 | Rejects the fallback storage for online media, including the default upload destination. |
| [2026-027](https://news.typo3.com/security/advisory/typo3-core-sa-2026-027) | CVE-2026-119623 | Keeps the first-install wizard closed on configured instances. |
| [2026-028](https://news.typo3.com/security/advisory/typo3-core-sa-2026-028) | CVE-2026-125159 | Limits the image decoders selected from detected file content. |
| [2026-029](https://news.typo3.com/security/advisory/typo3-core-sa-2026-029) | CVE-2026-125160 | Shows a generic form upload error and logs exception details internally. |
| [2026-030](https://news.typo3.com/security/advisory/typo3-core-sa-2026-030) | CVE-2026-125161 | Validates both the raw and decoded redirect URL. |
| [2026-032](https://news.typo3.com/security/advisory/typo3-core-sa-2026-032) | CVE-2026-125163 | Rejects form uploads when the MIME allowlist is empty. |
| [2026-033](https://news.typo3.com/security/advisory/typo3-core-sa-2026-033) | CVE-2026-125164 | Requires the file selector to match the signed resource type. |

## Apply

Back up the source tree and configuration, then run the following in Bash or Git Bash from the TYPO3 root containing `typo3/sysext/`. Use the absolute path to this repository as `bundle`. The checksums cover the twelve affected files in the original release. If a check fails, review local changes before proceeding. Apply 026 and 027 together.

```bash
bundle=/absolute/path/to/typo3-9.5.31-security-backports
sha256sum -c "$bundle/checksums/typo3-9.5.31.sha256" &&
  git -c core.autocrlf=false apply --check --whitespace=error "$bundle"/patches/*.patch &&
  git -c core.autocrlf=false apply --whitespace=error "$bundle"/patches/*.patch
```

Before deployment, complete any first installation already in progress. Set an explicit `allowedMimeTypes` list for every `ext:form` file or image upload field that should accept uploads. Review custom image format settings, especially `SYS/FileInfo/fileExtensionToMimeType`. After deployment, clear the TYPO3 caches and verify backend sign-in, the uploads used by your site, image processing and file downloads. Use staging for these checks.

## Roll back

From the same TYPO3 root, with all seven patches applied and the same `bundle` value, run:

```bash
git -c core.autocrlf=false apply --reverse --check "$bundle"/patches/*.patch &&
  git -c core.autocrlf=false apply --reverse "$bundle"/patches/*.patch &&
  sha256sum -c "$bundle/checksums/typo3-9.5.31.sha256"
```

Clear the TYPO3 caches after reverting. Reversing the patches restores the PHP files and removes the fixes. Handle any separately changed configuration or database state through your normal backup procedure. If later edits prevent a clean reversal, restore the verified backup.

## Credits and license

The original fixes are the work of the TYPO3 contributors credited in [NOTICE.md](NOTICE.md). The backports are distributed under [GPL-2.0-or-later](LICENSE).
