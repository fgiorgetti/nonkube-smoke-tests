
# Skupper V2 nonkube smoke tests Ansible Project

[![CI](https://github.com/fgiorgetti/nonkube-smoke-tests/actions/workflows/tests.yml/badge.svg)](https://github.com/fgiorgetti/nonkube-smoke-tests/actions/workflows/tests.yml)

Skupper V2 nonkube smoke tests using Ansible.
Runs the hello-world example using only nonkube platforms.

The execution is meant to cover the following matrix when running locally (using inventory-local):

|Command     |OS        |User    |Platform|
|------------|----------|--------|--------|
|bootstrap   |fedora39  |rootless|podman  |
|bootstrap   |fedora39  |rootless|docker  |
|bootstrap   |fedora39  |rootless|systemd |
|bootstrap   |fedora39  |root    |podman  |
|bootstrap   |fedora39  |root    |docker  |
|bootstrap   |fedora39  |root    |systemd |
|bootstrap   |rhel9     |rootless|podman  |
|bootstrap   |rhel9     |rootless|docker  |
|bootstrap   |rhel9     |rootless|systemd |
|bootstrap   |rhel9     |root    |podman  |
|bootstrap   |rhel9     |root    |docker  |
|bootstrap   |rhel9     |root    |systemd |
|bootstrap   |ubuntu2404|rootless|podman  |
|bootstrap   |ubuntu2404|rootless|docker  |
|bootstrap   |ubuntu2404|rootless|systemd |
|bootstrap   |ubuntu2404|root    |podman  |
|bootstrap   |ubuntu2404|root    |docker  |
|bootstrap   |ubuntu2404|root    |systemd |
|bootstrap.sh|fedora39  |rootless|podman  |
|bootstrap.sh|fedora39  |rootless|docker  |
|bootstrap.sh|fedora39  |rootless|systemd |
|bootstrap.sh|fedora39  |root    |podman  |
|bootstrap.sh|fedora39  |root    |docker  |
|bootstrap.sh|fedora39  |root    |systemd |
|bootstrap.sh|rhel9     |rootless|podman  |
|bootstrap.sh|rhel9     |rootless|docker  |
|bootstrap.sh|rhel9     |rootless|systemd |
|bootstrap.sh|rhel9     |root    |podman  |
|bootstrap.sh|rhel9     |root    |docker  |
|bootstrap.sh|rhel9     |root    |systemd |
|bootstrap.sh|ubuntu2404|rootless|podman  |
|bootstrap.sh|ubuntu2404|rootless|docker  |
|bootstrap.sh|ubuntu2404|rootless|systemd |
|bootstrap.sh|ubuntu2404|root    |podman  |
|bootstrap.sh|ubuntu2404|root    |docker  |
|bootstrap.sh|ubuntu2404|root    |systemd |

Through the CI a smaller matrix (using inventory):

|Commands    |OS        |User    |Platform|
|------------|----------|--------|--------|
|bootstrap   |ubuntu2404|rootless|podman  |
|bootstrap   |ubuntu2404|rootless|docker  |
|bootstrap   |ubuntu2404|rootless|systemd |
|bootstrap.sh|ubuntu2404|rootless|podman  |
|bootstrap.sh|ubuntu2404|rootless|docker  |
|bootstrap.sh|ubuntu2404|rootless|systemd |

**Notes:**

* The bootstrap binary and shell script must be built locally and placed under the following
  directory before the playbooks can be executed:
  `collections/ansible_collections/fgiorgetti/nonkube_smoke_tests/roles/run/files/commands/`

* The remove.sh shell script must be placed under the following
  directory before the playbooks can be executed:
  `collections/ansible_collections/fgiorgetti/nonkube_smoke_tests/roles/cleanup/files/commands/`

* All hosts must be updated accordingly under `inventory/hosts.yml`

* The participant hosts must have the following tools installed and ready to use:
  * Podman (does not run in rootful mode as there are some issues managing podman containers using sudo)
  * Docker (non root user must have permission to manage containers)
  * Skupper router (built from sources or installed using RPM)
  * Your user must be able to log in as both root and non root user against all hosts
  * Rootful tests disabled in CI as we are unable to write to /usr and /etc

## Included content/ Directory Structure

```
.
├── ansible.cfg
├── ansible-navigator.log
├── ansible-navigator.yml
├── collections
│   ├── ansible_collections
│   │   └── fgiorgetti
│   │       └── nonkube_smoke_tests
│   │           ├── README.md
│   │           └── roles
│   │               ├── cleanup
│   │               │   ├── files
│   │               │   │   └── commands
│   │               │   │       └── remove.sh
│   │               │   └── tasks
│   │               │       ├── customresources-cleanup.yml
│   │               │       ├── main.yml
│   │               │       ├── remove.yml
│   │               │       └── workloads-cleanup.yml
│   │               └── run
│   │                   ├── files
│   │                   │   ├── commands
│   │                   │   │   ├── bootstrap
│   │                   │   │   └── bootstrap.sh
│   │                   │   └── hello-world
│   │                   │       ├── east
│   │                   │       │   ├── connector.yaml
│   │                   │       │   ├── link-go-west.yaml
│   │                   │       │   └── site.yaml
│   │                   │       └── west
│   │                   │           ├── listener.yaml
│   │                   │           ├── routeraccess.yaml
│   │                   │           └── site.yaml
│   │                   ├── README.md
│   │                   └── tasks
│   │                       ├── bootstrap.yml
│   │                       ├── containerengine.yml
│   │                       ├── customresources.yml
│   │                       ├── main.yml
│   │                       ├── validate.yml
│   │                       └── workloads-setup.yml
│   └── requirements.yml
├── devfile.yaml
├── inventory
│   ├── group_vars
│   │   ├── all.yml
│   │   ├── rootful.yml
│   │   └── rootless.yml
│   ├── hosts.yml
│   └── host_vars
│       └── ci.yml
├── inventory-local
│   ├── group_vars
│   │   ├── all.yml
│   │   ├── rootful.yml
│   │   └── rootless.yml
│   ├── hosts.yml
│   └── host_vars
│       ├── fedora.yml
│       ├── rhel.yml
│       └── ubuntu.yml
├── README.md
├── smoke_tests_cleanup.yml
├── smoke_tests_rootful.yml
├── smoke_tests_rootless.yml
└── smoke_tests.yml
```

## Compatible with Ansible-lint

Tested with ansible-lint >=24.2.0 releases and the current development version of ansible-core.
