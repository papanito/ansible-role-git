# Ansbile role "papanito.git"  <!-- omit in toc -->

[![Ansible Role](https://img.shields.io/ansible/role/50326)](https://galaxy.ansible.com/papanito/git) [![Ansible Quality Score](https://img.shields.io/ansible/quality/50326)](https://galaxy.ansible.com/papanito/git) [![Ansible Role](https://img.shields.io/ansible/role/d/50326)](https://galaxy.ansible.com/papanito/git) [![GitHub issues](https://img.shields.io/github/issues/papanito/ansible-role-git)](https://github.com/papanito/ansible-role-git/issues) [![GitHub pull requests](https://img.shields.io/github/issues-pr/papanito/ansible-role-git)](https://github.com/papanito/ansible-role-git/pulls)

- [Requirements](#requirements)
- [Role Variables](#role-variables)
  - [General parameters](#general-parameters)
    - [Parameters for `git_repos`](#parameters-for-git_repos)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
- [License](#license)
- [Author Information](#author-information)

This role is used for simplified git checkouts. The idea is to provide a list of git repos to clone to a common directory, which can be overridden.

## Requirements

none

## Role Variables

### General parameters

These are all variables

|Parameter|Description|Default Value|
|---------|-----------|-------------|
|`git_repos`|Dictionary of git repos, see [Parameters for `git_repos`](#parameters-for-git-repos)|-|
|`accept_hostkey`|if `yes`, ensure that `-o StrictHostKeyChecking=no` is present as an ssh option.|`No`|
|`install_packages`|if `no`  git packages are not installed, otherwise you need to run the role with `--ask-become-pass`|`Yes`|

#### Parameters for `git_repos`

`git_repos` are always mounted with [type `cifs` and in state `mounted`](https://docs.ansible.com/ansible/latest/modules/mount_module.html). However there are some options to provide:

|Parameter|Description|Default Value|
|---------|-----------|-------------|
|`repo`|[Mandatory] git, SSH, or HTTP(S) protocol address of the git repository.|-|
|`dest`|Base dir of path of where the repository should be checked out. This parameter is required, unless `clone` is set to `no`<br>The final path where the git checkout happens is `{{ dest }}/{{ git_repos.key }}`|-|
|`remote`|Name of the remote.|`origin`|
|`bare`|if `yes`, repository will be created as a bare repo, otherwise it will be a standard repo with a workspace.|`No`|
|`clone`|If `no`, do not clone the repository even if it does not exist locally|`Yes`|
|`force`|If `yes`, any modified files in the working repository will be discarded. Prior to 0.7, this was always 'yes' and could not be disabled. Prior to 1.9, the default was `yes`|`No`|
|`recursive`|If `no` repository will be cloned without the `--recursive` option, skipping sub-modules.|`Yes`|
|`update`|If `no` do not retrieve new revisions from the origin repository|`Yes`|
|`dependencies`|List of packages (dependencies) to be installer. It suffices if providing the package name. However it supports additional package types if you add a list `{ name:PACKAGENAME, type:PACKAGETYPE}`, whereas `PACKAGETYPE` can be<ul><li>`aur`: Archlinux AUR</li></ul>|`[]`|

**Examples:**

Checkout repo `git@github.com:qeeqbox/social-analyzer.git` to `~/Workspace/social-analyzer-demo`.

```yml
  vars:
    git_repos:
      social-analyzer-demo:
        repo: git@github.com:qeeqbox/social-analyzer.git
        dest: ~/Workspace
```

Checkout two repos to `~/Workspace/social-analyzer` and `~/Workspace/airgeddon`. In addition som packages are installed (which are required to run the `airgeddon` script)

```yml
  vars:
    git_repos:
      social-analyzer:
        repo: git@github.com:qeeqbox/social-analyzer.git
      airgeddon:
        repo: git@github.com:v1s1t0r1sh3r3/airgeddon.git
        dependencies:
          - { name: crunch, type: aur }
          - aircrack-nge
```

## Dependencies

none

## Example Playbook

Run `ansible-playbook ./tests/test.yml -i ./tests/inventory -e "install_packages=No"`

```yaml
---
- hosts: localhost
  vars:
    default_workspace: "~/Workspace/demo"
    git_repos:
      social-analyzer:
        repo: git@github.com:qeeqbox/social-analyzer.git

 roles:
    - papanito.git
```

## License

This is Free Software, released under the terms of the Apache v2 license.

## Author Information

Written by [Papanito](https://wyssmann.com) - [Gitlab](https://gitlab.com/papanito) / [Github](https://github.com/papanito)