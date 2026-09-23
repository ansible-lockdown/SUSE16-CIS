# Changes to SUSE 16 CIS

# Based on CIS SUSE Linux Enterprise 16 Benchmark v1.0.0

## September 2026

- 1.3.1.2, 6.2.1.3, 6.2.1.4 GRUB_CMDLINE_LINUX edited in place; duplicate lines no longer corrupt /etc/default/grub
- 1.2.1.3 and 1.2.1.5 zypp settings set under [main] via ini_file
- 2.3.1.1 masking task moved from service to systemd module
- container discovery guarded against undefined virtualization_type
- 5.2.4 sudoers_exclude_nopasswd_list passed to the audit
- defaults split into defaults/main/main.yml and defaults/main/audit.yml
- prelim include_vars of audit.yml removed
- gitleaks pre-commit rev v8.30.1 -> v8.30.0
- 1.1.1.1, 1.1.1.3-1.1.1.6 install line written to 60-blacklist_fs-<module>.conf, overriding the vendor unblacklist
- 4.1.3 waits for firewalld to accept commands after starting it
- 4.1.5 adds lo to the loopback zone when it has no zone
- 5.1.1 sets permissions on sshd_config.d drop-ins and /usr/etc/ssh/sshd_config
- 5.1.5 banner.conf and 5.1.15 weak_macs.conf mode 0600
- 5.3.2.3.3 use_authtok set via pam-config, checked in the PAM files and reapplied after pam-config --update
- 4.1.5 lo zone gate uses rc 2; firewall-cmd prints no zone on stderr
- update vars with company_title: 'MindPoint Group - A Quantum Sky Company'
- suse16cis_tmp_svc defaults to auto - /tmp method discovered from tmp.mount
- tmp.mount enabled and unmasked when the systemd method is used
- README rendered from the canonical template

## August 2026 - QA findings remediation

- 1.3.1.3 and 1.3.1.4 rewritten, were AppArmor commands under SELinux control IDs
- suse16cis_gui now resolves, prelim_gnome_present was never registered
- section 1.8 renumbered to the benchmark, 1.8.3 screen lock implemented for the first time
- 1.7.4 pam_motd and 1.7.5 sshd banner content implemented, 1.7.6 now targets /etc/motd
- 2.4.1.7 now targets /etc/cron.yearly, 2.4.1.8 now targets /etc/cron.d
- 2.3.1.1 writes chrony pool/server config, confdir include and OPTIONS=-u chrony
- 2.3.1.1 also configures timesyncd when suse16cis_time_sync_tool is systemd-timesyncd
- 6.2.4.1-6.2.4.4 use the audit log directory, basename returned the log filename
- 5.4.2.1 and 5.4.1.5 awk expressions fixed, both were silent no-ops
- 1.2.1.1 gpg key check was quoted as one word and always returned 127
- 5.4.3.3 no longer writes a literal #\1 over every umask line, and skips 5.4.3.4's file
- 3.1.2 wireless discovery uses shell for the glob, print0 for xargs, /bin/false for modprobe
- 2.1.21 exim4 loop was nested inside the module args
- sudoers drop-in renamed to CIS_defaults, a dot in the name made sudo ignore it, visudo validate added
- 5.1.4 drop-in path was sshd_config.s, 5.1.17 was max_startups.cong
- 6.1.1.6 targeted /etc/systemd/journal.conf
- 6.1.3.1 uses zypper log paths, was /var/log/apt
- 5.4.1.2 set password_expire_max from pass_min_days
- 7.1.11 xargs -r, 7.1.12 find exclusions moved after the path
- 7.2.6 referenced an undefined register
- handlers: auditd reload condition was always true, immutable fact never fired
- 1.6.5 weak MAC subpolicy now -*-128* -HMAC-SHA1
- 2.1.20 removes xwayland, 2.2.2 removes openldap2_6-client, 5.1.15 adds umac-128-etm
- 5.1.6 crypto subpolicy was gated on rule 5.1.4
- 6.3.3 aide.conf path is now suse16cis_aide_conf_file and skips if absent
- 1.3.1.6 adds .autorelabel and the reboot flag
- 12 modifying blocks were tagged neither patch nor audit
- 4.1.5 and 4.1.6 were tagged audit while changing permanent firewalld state
- 6.2.4.x discovery tasks gained the level tags their consumers use
- prelim pwquality directory task was untagged and skipped by any --tags run
- 1.2.1.3 was tagged level1, benchmark is level2
- duplicate crypto-policy assert removed from main.yml
- rule toggles 1.2.1.5, 5.1.24, 6.1.2.9 and 6.3.3 enabled, the rest documented
- suse16cis_desktop_required removed, it drove a control not in this benchmark
- 3 dead templates removed, 1 was zero-byte
- meta declares openSUSE, actions/checkout to v7.0.0

## August 2026 - QA pass fixes and molecule standardization

- section 1.8 import gated on gdm, not the Debian gdm3
- suse16cis_gui emitted unquoted in the audit bridge template
- tasks/remount_tmp.yml rewritten as listen handlers, reboot warning no longer always fires
- 7.1.13 find -perm expression corrected, was returning nothing
- 6.1.2.8 subtask titles and stray 6.2.3.7 control ID corrected
- goss binary moved to krameff v0.5.0 with new checksums
- crypto policy handler guarded against a missing .stdout
- bracket notation updated
- CONTRIBUTING.md and README Contributing section added
- molecule: default scenario standardized to the SUSE15-CIS convention
- molecule: prepare.yml added for CIS runtime packages and SSH host keys
- molecule: audit fetch vars forced via set_fact above vars/audit.yml precedence
- molecule: suse16cis_disruption_high false on localhost and wsl
- molecule: README_Molecule_QuickStart.md added

## July 2026 - QA pass fixes

### Fixed

- Added copyright_year: '2026' to defaults/main.yml
- tasks/prelim.yml: renamed "Gather the package facts" to include PRELIM | prefix for naming consistency
- tasks/section_6/cis_6.1.1.x.yml: added set -o pipefail and failed_when: false to two audit-only
  grep | wc -l shell tasks (6.1.1.7) to ensure correct exit code handling when grep finds no matches
- Cross-repo (SUSE16-CIS-Audit): corrected suse16cis_legacy_boot from true to false to match
  remediation default (prevents audit testing BIOS boot controls on UEFI systems)

## June 2026 - QA pass fixes

### Fixed

- tasks/section_1/cis_1.8.x.yml: 1.8.4-1.8.7 template misalignment — controls had templates shifted one position forward
  - 1.8.4 (automount) now deploys `00-media-automount.j2` and `00-automount_lock.j2` (was deploying screensaver template)
  - 1.8.5 (autorun-never) now deploys `00-media-autorun.j2` and `00-autorun_lock.j2` with conf file (was deploying screensaver lock only)
  - 1.8.6 (XDMCP) now uses lineinfile on `/etc/gdm/custom.conf` to disable Enable=true (was deploying automount dconf template)
  - 1.8.7 (Xwayland) now uses lineinfile on `/etc/gdm/custom.conf` to set WaylandEnable=false (was deploying automount lock template)
- Removed 7 orphaned templates with no task references: modprobe.conf.j2, crypt_audit_procs.conf.j2, NO-SSHETM.pmod.j2, rsyslog.d/log_server.conf.j2, chrony.d/pool.conf.j2, chrony.d/server.conf.j2, timesyncd.conf.d/60-timesyncd.conf.j2
- README.md: removed stale `python-def` and `libselinux-python` dependencies; replaced with `python3`

## v1.0.0 - 2026-06-11

Initial release. There is no prior SUSE 16 CIS remediation or audit content in this lineage.

### Added

- **Private-SUSE16-CIS** — Ansible remediation role for all **336** benchmark controls (sections 1–7)
- **SUSE16-CIS-Audit** — matching Goss audit content
- **SUSE16-CIS/** — public-role mirror in this monorepo (source for a separate Galaxy repo cut later)
- Rule toggles, `lockdown_audit.yml.j2`, and audit vars aligned to the benchmark JSON
- `--benchmark suse16` support in repo validators; `run_all_checks.sh` integration
- Molecule scenarios: `default`, `localhost`, `wsl` — **`molecule test -s default` passes** (converge, idempotence, verify)
- Container exclusions via `vars/is_container.yml` (no Dockerfile stubs for missing OS paths); `create_benchmark_facts: false` in containers (compliance-facts timestamp is expected on bare metal when facts are enabled)

### Platform notes

- Section **1.3.1** benchmark IDs reference SELinux; remediation and audit implement **AppArmor** on SUSE (documented in role README)

### Release layout

- Development stays in this **Ansible-lockdown monorepo** for approximately three months
- A separate **ansible-lockdown/SUSE16-CIS** public repository will be created from the monorepo mirror when ready for Galaxy release
