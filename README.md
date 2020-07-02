# Git server - Ansible role

[![Build Status](https://travis-ci.org/swcc/ansible-gitaas.svg?branch=master)](https://travis-ci.org/swcc/ansible-gitaas) [![Ansible Galaxy](https://img.shields.io/badge/role-swcc.ansible__gitaas-blue.svg)](https://galaxy.ansible.com/swcc/ansible-gitaas)

Role to install git, as a server. Following the steps of <https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server>.

This role can also install a rootless docker daemon to the git user which will be able run software via simple `git push` (PaaS like) if the git project has a `docker-compose.yml` file.

See details of the operations running during the `post-receive` hook (after a `git push`) in the `templates/post-receive.j2` script.

## Example playbook

Basic example

```yaml
- hosts: gitservers
  roles:
    - role: swcc.gitaas
      vars:
        git_repos:
          - project1
          - project2
        git_authorized_keys:
          - "{{ lookup('file', 'public_keys/deploy-key.pub') }}"
        rootless_docker_enabled: true
```

## Role parameters

| Variable                  | Default    | Type      | Description                                                                                                            |
|---------------------------|------------|-----------|------------------------------------------------------------------------------------------------------------------------|
| `git_user`                | `git`      | `string`  | Unix user name to use on the server                                                                                    |
| `git_home`                | `/opt/git` | `string`  | Unix path of the home directory of the Git user                                                                        |
| `git_authorized_keys`     | `[]`       | `array`   | List of public ssh keys to allow for git operations                                                                    |
| `git_repos`               | `[]`       | `array`   | List of repositories to create                                                                                         |
| `rootless_docker_enabled` | `false`    | `boolean` | Whether to install Docker to run `post-receive` hook on `git push` to launch a `docker-compose` compatible application |

## Makefile for easier Ansible usage

I have written a small Makefile to make your future ansible runs easier. Don't hesitate to [check it out](https://github.com/paulRbr/ansible-makefile/).

Download the `*.deb` package from the github releases, install it and start using it with `ansible-make help`.

## License

GPLv3
