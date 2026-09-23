# SUSE 16 CIS

## Configure a SUSE 16 machine to be [CIS](https://www.cisecurity.org/cis-benchmarks/) compliant

### Based on [CIS SUSE Linux Enterprise 16 Benchmark v1.0.0](https://www.cisecurity.org/cis-benchmarks/)

---

## Contents

- [Status](#status)
- [Caution(s)](#cautions)
- [Requirements](#requirements)
- [Quick Start](#quick-start)
- [Customising the Role](#customising-the-role)
- [Auditing](#auditing)
- [Documentation](#documentation)
- [Looking for support?](#looking-for-support)
- [Testing](#testing)
- [Known Issues](#known-issues)
- [Credits and Thanks](#credits-and-thanks)

---

## Status

### Public Repository

![Org Stars](https://img.shields.io/github/stars/ansible-lockdown?label=Org%20Stars&style=social)
![Stars](https://img.shields.io/github/stars/ansible-lockdown/SUSE16-CIS?label=Repo%20Stars&style=social)
![Forks](https://img.shields.io/github/forks/ansible-lockdown/SUSE16-CIS?style=social)
![Followers](https://img.shields.io/github/followers/ansible-lockdown?style=social)
[![X URL](https://img.shields.io/twitter/url/https/x.com/AnsibleLockdown.svg?style=social&label=Follow%20%40AnsibleLockdown)](https://x.com/AnsibleLockdown)
![Discord Badge](https://img.shields.io/discord/925818806838919229?logo=discord)
![License](https://img.shields.io/github/license/ansible-lockdown/SUSE16-CIS?label=License)

### Lint & Pre-Commit Tools

![YamlLint](https://img.shields.io/badge/yamllint-Present-brightgreen?style=flat&logo=yaml&logoColor=white)
![Ansible-Lint](https://img.shields.io/badge/ansible--lint-Present-brightgreen?style=flat&logo=ansible&logoColor=white)

### Community Release Information

![Release Branch](https://img.shields.io/badge/Release%20Branch-Main-brightgreen)
![Release Tag](https://img.shields.io/github/v/tag/ansible-lockdown/SUSE16-CIS?label=Release%20Tag&&color=success)
![Main Release Date](https://img.shields.io/github/release-date/ansible-lockdown/SUSE16-CIS?label=Release%20Date)
![Benchmark Version Main](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_linux_IaC/badges/SUSE16-CIS/benchmark-version-main.json)
![Benchmark Version Devel](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_linux_IaC/badges/SUSE16-CIS/benchmark-version-devel.json)

[![Main Pipeline Status](https://github.com/ansible-lockdown/SUSE16-CIS/actions/workflows/main_pipeline_validation.yml/badge.svg?)](https://github.com/ansible-lockdown/SUSE16-CIS/actions/workflows/main_pipeline_validation.yml)
[![Devel Pipeline Status](https://github.com/ansible-lockdown/SUSE16-CIS/actions/workflows/devel_pipeline_validation.yml/badge.svg?)](https://github.com/ansible-lockdown/SUSE16-CIS/actions/workflows/devel_pipeline_validation.yml)

![Devel Commits](https://img.shields.io/github/commit-activity/m/ansible-lockdown/SUSE16-CIS/devel?color=dark%20green&label=Devel%20Branch%20Commits)
![Open Issues](https://img.shields.io/github/issues-raw/ansible-lockdown/SUSE16-CIS?label=Open%20Issues)
![Closed Issues](https://img.shields.io/github/issues-closed-raw/ansible-lockdown/SUSE16-CIS?label=Closed%20Issues&&color=success)
![Pull Requests](https://img.shields.io/github/issues-pr/ansible-lockdown/SUSE16-CIS?label=Pull%20Requests)

### Subscriber Release Information

![Private Release Branch](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_linux_IaC/badges/Private-SUSE16-CIS/release-branch.json)
![Private Benchmark Version](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_linux_IaC/badges/Private-SUSE16-CIS/benchmark-version.json)
[![Private Remediate Pipeline](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_linux_IaC/badges/Private-SUSE16-CIS/remediate.json)](https://github.com/ansible-lockdown/Private-SUSE16-CIS/actions/workflows/main_pipeline_validation.yml)
![Private Pull Requests](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_linux_IaC/badges/Private-SUSE16-CIS/prs.json)
![Private Closed Issues](https://img.shields.io/endpoint?url=https://ansible-lockdown.github.io/github_linux_IaC/badges/Private-SUSE16-CIS/issues-closed.json)

---

## Caution(s)

This role **will make changes to the system** which may have unintended consequences. This is not an auditing tool but
rather a remediation tool to be used after an audit has been conducted.

- Testing is the most important thing you can do. Did we mention testing?
- Check mode is not supported. The role will complete in check mode without errors, but use it with caution.
- This role was developed against a clean install of the operating system. If you are applying it to an existing system,
  review the role for any site specific changes that are needed.
- To use a release version, point to the main branch and the release for the benchmark you wish to work with.
- This is a Tumbleweed deployment, not Leap.
- Based on and tested against the 20260618 snapshot. If you are using an older snapshot, update first; snapshots carry a large number of changes.

---

## Requirements

**General:**

- Basic knowledge of Ansible. If you are unfamiliar with Ansible, these links help get started:
  - [Main Ansible documentation page](https://docs.ansible.com)
  - [Ansible Getting Started](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html)
  - [Ansible Community Info](https://docs.ansible.com/ansible/latest/community/index.html)
- A functioning Ansible installation (or AWX / Automation Controller), configured and able to reach the target hosts.
- Read through the tasks in this role to understand what each control does. Some tasks are disruptive and can have
  unintended consequences on a live production system. Also familiarise yourself with the variables in
  `defaults/main/main.yml`.

**Technical Dependencies:**

- SUSE 16 (openSUSE Tumbleweed)
- ansible-core 2.16.1 or newer
- Collections listed in [collections/requirements.yml](collections/requirements.yml): `community.general`, `community.crypto`, `ansible.posix`
- If using the audit: access to download or add the goss binary and audit content to the system (other options are
  available for getting the content onto the host)

---

## Quick Start

Install the collections, then point a playbook at the role:

```sh
ansible-galaxy collection install -r collections/requirements.yml
```

```yaml
- name: Apply CIS hardening
  hosts: suse_servers
  become: true
  roles:
    - role: SUSE16-CIS
```

The repository includes a ready made `site.yml` that targets `all` hosts, or the hosts passed in the `hosts` variable:

```sh
ansible-playbook -i inventory site.yml -e hosts=suse_servers
```

To run the audit before and after remediation, set `setup_audit: true` and `run_audit: true` (see [Auditing](#auditing)).

---

## Customising the Role

This role is designed so that the end user should not have to edit the tasks themselves. All customising should be done
by overriding variables from `defaults/main/main.yml` (remediation) and `defaults/main/audit.yml` (audit), for example
in `group_vars`, `host_vars` or with extra vars in the project, job or workflow.

- Every control has a toggle named after its ID, for example `suse16cis_rule_1_1_1_1`, so individual controls can be
  switched off.
- Each section can be switched off with `suse16cis_section1` to `suse16cis_section7`.

### Matching a Security Level

It is possible to only run level 1 or level 2 controls for CIS. This is managed using tags:

- level1-server
- level1-workstation
- level2-server
- level2-workstation

The level variables in `defaults/main/main.yml` also need to reflect this, as they control the testing that takes place if
you are using the audit component.

### Tags

There are many tags available for added control precision. Each control has its own set of tags noting what level, what
OS element it relates to, whether it is a patch or audit, and the rule number. NIST references follow a specific
conversion format for consistency and clarity.

Below is an example of the tag section from a control within this role. Using this example, if you set your run to skip
all controls with the tag `cramfs`, this task will be skipped. The opposite can also happen where you run only controls
tagged with `cramfs`.

```yaml
  tags:
    - level1-server
    - level1-workstation
    - patch
    - rule_1.1.1.1
    - cramfs
    - NIST800-53R5_CM-7
```

**Conversion format for NIST references:**

1. Standard prefix: all references are prefixed with "NIST".
2. Standard types:
   - "800-53" references are formatted as NIST800-53.
   - "800-53r5" references are formatted as NIST800-53R5 (with 'R' capitalised).
   - "800-171" references are formatted as NIST800-171.
3. Details:
   - Section and subsection numbers use periods (.) for numeric separators.
   - Parenthetical elements are separated by underscores (_), e.g., IA-5(1)(d) becomes IA-5_1_d.
   - Subsection letters (e.g., "b") are appended with an underscore.

---

## Auditing

This can be turned on or off with the variables `setup_audit` and `run_audit` in `defaults/main/main.yml`. The value
is false by default, please refer to the wiki for more details. The role passes its variables to the audit, so only the
controls that are enabled in the role are checked.

The audit uses a small (16MB) go binary called [goss](https://github.com/krameff/goss) along with the relevant
configurations to check, without the need for infrastructure or other tooling. It is a quick, lightweight check of both
the configuration and the live/running settings, which aims to remove
[false positives](https://www.mindpointgroup.com/blog/is-compliance-scanning-still-relevant/) in the process.

Refer to [SUSE16-CIS-Audit](https://github.com/ansible-lockdown/SUSE16-CIS-Audit).

### Example Audit Summary

This is based on a vagrant image with selections enabled, e.g. no GUI or firewall.
Note: more tests are run during the audit as we check config and running state.

```txt
ok: [default] => {
    "msg": [
        "The pre remediation audit results are: Count: 763, Failed: 234, Skipped: 4, Duration: 9.741s",
        "The post remediation audit results are: Count: 763, Failed: 19, Skipped: 4, Duration: 12.725s",
        "Full breakdown can be found in /opt",
        ""
    ]
}

PLAY RECAP *******************************************************************************************************************************************
default                    : ok=270  changed=23   unreachable=0    failed=0    skipped=140  rescued=0    ignored=0
```

---

## Documentation

- [Read The Docs](https://ansible-lockdown.readthedocs.io/en/latest/)
- [Getting Started](https://www.lockdownenterprise.com/docs/getting-started-with-lockdown#GH_AL_SUSE16_cis)
- [Customizing Roles](https://www.lockdownenterprise.com/docs/customizing-lockdown-enterprise#GH_AL_SUSE16_cis)
- [Per-Host Configuration](https://www.lockdownenterprise.com/docs/per-host-lockdown-enterprise-configuration#GH_AL_SUSE16_cis)
- [Getting the Most Out of the Role](https://www.lockdownenterprise.com/docs/get-the-most-out-of-lockdown-enterprise#GH_AL_SUSE16_cis)
- [Changelog](./Changelog.md)

---

## Looking for support?

[Lockdown Enterprise](https://www.lockdownenterprise.com#GH_AL_SUSE16_cis)

[Ansible support](https://www.mindpointgroup.com/cybersecurity-products/ansible-counselor#GH_AL_SUSE16_cis)

### Community

On our [Discord Server](https://www.lockdownenterprise.com/discord) to ask questions, discuss features, or just chat with other Ansible-Lockdown users

### Contributing

Bug reports and feature requests are welcome from everyone, please raise an issue.

Pull requests are accepted from approved contributors only. To be onboarded, join the [Discord Server](https://www.lockdownenterprise.com/discord) and request contributor access. See [CONTRIBUTING.md](CONTRIBUTING.md) for the full process.

---

## Testing

### Pipeline Testing

Automated tests run on pull requests into devel:

- self-hosted runners using OpenTofu
- ansible collections pulled at the latest version from the requirements file
- the audit runs using the devel branch

### Local Testing

Molecule scenarios: `default`, `localhost`, `wsl`.

```bash
molecule test -s default
```

---

## Known Issues

- Section 1.3.1 - SELinux benchmark IDs, AppArmor remediation. The CIS SUSE 16 benchmark labels 1.3.1 with SELinux control titles (for example "Ensure SELinux is installed"), but SLES 16 uses AppArmor as its mandatory access control framework. The role maps those IDs to AppArmor tasks (`apparmor`, `aa-enforce`, GRUB `apparmor=1` / `security=apparmor` and related package checks), and the goss audit uses the same IDs with AppArmor tests. This is intentional platform mapping, not missing coverage.
- 1.2.2.1 returns rc 4 on some installations. This is a SUSE packaging change, fixed by installing the new package `gio-branding-upstream` (done manually, it also asks to remove the old one).

---

## Credits and Thanks

Massive thanks to the fantastic community and all its members.

This includes a huge thanks and credit to the original authors and maintainers.

Mark Bolwell
