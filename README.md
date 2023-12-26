# Ansible role [ecr_container_build](https://galaxy.ansible.com/ui/standalone/roles/buluma/ecr_container_build/documentation)

ECR docker image build and push management role.

|GitHub|Version|Issues|Pull Requests|Downloads|
|------|-------|------|-------------|---------|
|[![github](https://github.com/buluma/ansible-role-ecr_container_build/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-ecr_container_build/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-ecr_container_build.svg)](https://github.com/buluma/ansible-role-ecr_container_build/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-ecr_container_build.svg)](https://github.com/buluma/ansible-role-ecr_container_build/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-ecr_container_build.svg)](https://github.com/buluma/ansible-role-ecr_container_build/pulls/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/ecr_container_build)](https://galaxy.ansible.com/ui/standalone/roles/buluma/ecr_container_build/documentation)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-ecr_container_build/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true

  vars:
    ecr_image_src_dir: "/tmp/test-container/"
    ecr_image_name: test-alpine
    ecr_image_buildargs: {test_var: 'ansible'}
    ecr_push: true
    docker_install_compose: false
    pip_install_packages:
      - docker

  pre_tasks:
    - name: Copy Docker build source dir into test server.
      copy:  # noqa 208
        src: "{{ item }}"
        dest: /tmp
      with_items:
        - test-container
        - test-container-buildargs

  roles:
    - role: buluma.ecr_container_build
      vars:
        ecr_image_src_dir: "/tmp/test-container/"
        ecr_image_name: test-alpine
        ecr_push: false

    - role: buluma.ecr_container_build
      vars:
        ecr_image_src_dir: "/tmp/test-container-buildargs/"
        ecr_image_name: test-alpine-buildargs
        ecr_image_buildargs: {test_var: 'ansible'}
        ecr_push: false
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-ecr_container_build/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: buluma.bootstrap
    - role: buluma.pip
      pip_install_packages:
        - docker
        - awscli
    - role: buluma.docker
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-ecr_container_build/blob/master/defaults/main.yml):

```yaml
---
# defaults file for ecr_container_build

ecr_image_src_dir: ../my-project
ecr_image_name: namespace/my-project

ecr_image_buildargs: {}

# You can add one or more tags.
ecr_image_tags: ['latest']

# Set this to true if you need to pull from ECR for the image build.
ecr_login_required: false

# Whether to push the built image to ECR.
ecr_push: true

# AWS account details for ECR.
ecr_region: us-east-1
ecr_account_id: '123456789012'
ecr_url: "{{ ecr_account_id }}.dkr.ecr.{{ ecr_region }}.amazonaws.com"
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-ecr_container_build/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.pip](https://galaxy.ansible.com/buluma/pip)|[![Ansible Molecule](https://github.com/buluma/ansible-role-pip/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-pip/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-pip.svg)](https://github.com/shadowwalker/ansible-role-pip)|
|[buluma.docker](https://galaxy.ansible.com/buluma/docker)|[![Ansible Molecule](https://github.com/buluma/ansible-role-docker/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-docker/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-docker.svg)](https://github.com/shadowwalker/ansible-role-docker)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-ecr_container_build/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/repository/docker/buluma/enterpriselinux/general)|all|
|[Fedora](https://hub.docker.com/repository/docker/buluma/fedora/general)|all|
|[Debian](https://hub.docker.com/repository/docker/buluma/debian/general)|all|
|[Ubuntu](https://hub.docker.com/repository/docker/buluma/ubuntu/general)|all|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-ecr_container_build/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-ecr_container_build/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-ecr_container_build/blob/master/LICENSE)

## [Author Information](#author-information)

[Shadow Walker](https://buluma.github.io/)


Template inspired by [Robert de Bock](https://github.com/robertdebock)
