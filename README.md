![image](./docs/_static/mantl-logo.png)

# Overview

[![Join the chat at https://gitter.im/mantl/mantl](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/mantl/mantl?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Build Status](https://img.shields.io/travis/mantl/mantl.svg)](https://travis-ci.org/mantl/mantl)

Mantl is a modern, batteries included platform for rapidly deploying globally
distributed services

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-generate-toc again -->
**Table of Contents**

- [Overview](#overview)
    - [Features](#features)
        - [Core Components](#core-components)
        - [Addons](#addons)
        - [Goals](#goals)
        - [Architecture](#architecture)
            - [Control Nodes](#control-nodes)
            - [Agent Nodes](#agent-nodes)
            - [Edge Nodes](#edge-nodes)
    - [Getting Started](#getting-started)
        - [Software Requirements](#software-requirements)
        - [Deploying on multiple servers](#deploying-on-multiple-servers)
    - [Documentation](#documentation)
    - [Roadmap](#roadmap)
        - [Mesos Frameworks](#mesos-frameworks)
        - [Security](#security)
        - [Operations](#operations)
        - [Supported Platforms](#supported-platforms)
            - [Community Supported Platforms](#community-supported-platforms)
    - [Development](#development)
    - [Getting Support](#getting-support)
    - [License](#license)

<!-- markdown-toc end -->


## Features

### Core Components
* [Consul](http://consul.io) for service discovery
* [Vault](http://vaultproject.io) for managing secrets
* [Mesos](http://mesos.apache.org) cluster manager for efficient resource
  isolation and sharing across distributed services
* [Marathon](https://mesosphere.github.io/marathon) for cluster management of
  long running containerized services
* [Kubernetes](http://kubernetes.io/) for managing, organizing, and scheduling
  containers
* [Terraform](https://terraform.io) deployment to multiple cloud providers
* [Docker](http://docker.io) container runtime
* [Traefik](https://traefik.github.io/) for proxying external traffic
* [mesos-consul](https://github.com/CiscoCloud/mesos-consul) populating Consul
  service discovery with Mesos tasks
* [Mantl API](https://github.com/CiscoCloud/mantl-api) easily install supported
  Mesos frameworks on Mantl
* [Mantl UI](https://github.com/CiscoCloud/nginx-mantlui) a beautiful
  administrative interface to Mantl

### Addons
* [ELK Stack](https://www.elastic.co/webinars/introduction-elk-stack) for log
  collection and analysis
     -  [Logstash](https://github.com/elastic/logstash) for log forwarding
* [GlusterFS](http://www.gluster.org/) for container volume storage
* [Docker Swarm](https://www.docker.com/products/docker-swarm/) for clustering
  Docker daemons between networked hosts
* [etcd](https://github.com/coreos/etcd), distributed key-value store for Calico
* [Calico](http://www.projectcalico.org), a new kind of virtual network
* [collectd](https://collectd.org/) for metrics collection
* [Chronos](https://mesos.github.io/chronos/) a distributed task scheduler
* [Kong](http://getkong.org) for managing APIs

See the `addons/` directory for the most up-to-date information.

### Goals
* Security
* High availability
* Rapid immutable deployment (with Terraform + Packer)

### Architecture

The base platform contains control nodes that manage the cluster and any number
of agent nodes. Containers automatically register themselves into DNS so
that other services can locate them.

![mantl-diagram](docs/_static/mantl-diagram.png)

#### Control Nodes

The control nodes manage a single datacenter. Each control node runs Consul for
service discovery, Mesos and Kubernetes leaders for resource scheduling and
Mesos frameworks like Marathon.

The Consul Ansible role will automatically bootstrap and join multiple Consul
nodes. The Mesos role will provision highly-availabile Mesos and ZooKeeper
environments when more than one node is provisioned.

#### Agent Nodes

Agent nodes launch containers and other Mesos- or Kubernetes-based workloads.

#### Edge Nodes

Edge nodes are responsible for proxying external traffic into services running
in the cluster.

## Getting Started

All development is done on the `master` branch. Tested, stable versions are
identified via git tags. To get started, you can clone or fork this repo:

```
git clone https://github.com/mantl/mantl.git
```

To use a stable version, use `git tag` to list the stable versions:

```
git tag
0.1.0
0.2.0
...
1.2.0


git checkout 1.2.0
```

A Vagrantfile is provided that provisions everything on a few VMs. To run,
first ensure that your system has at least 2GB of RAM free, then just:

```
vagrant up
```

Note:
* There is no support for Windows at this time, however support is planned.
* Use the latest version of Vagrant for best results. Version 1.8 is required.
* There is no support for the VMware Fusion Vagrant provider; hence your
  provider is set to Virtualbox in your Vagrantfile.

### Software Requirements

The only requirements for running Mantl are working installations of Terraform
and Ansible (or Vagrant, if you're deploying to VMs). See the "Development"
sections for requirements for developing Mantl.

### Deploying on multiple servers

Please refer to the [Getting Started
Guide](http://docs.mantl.io/en/latest/getting_started/index.html), which covers
cloud deployments.

## Documentation

All documentation is located at
[http://docs.mantl.io](http://docs.mantl.io/en/latest).

To build the documentation locally, run:

```
sudo pip install -r requirements.txt
cd docs
make html
```

### Additional Documentation

- [CONTRIBUTING.md](CONTRIBUTING.md) - Guidelines for contributing to Mantl
- [DEVELOPMENT.md](DEVELOPMENT.md) - Detailed development guide
- [UPGRADE.md](UPGRADE.md) - Guide for upgrading between versions
- [CHANGES.md](CHANGES.md) - Recent changes to documentation and tooling
- [code-of-conduct.md](code-of-conduct.md) - Code of conduct for the Mantl community

## Roadmap

### Mesos Frameworks

- [x] Marathon
- [x] Kafka
- [ ] Riak
- [x] Cassandra
- [x] Elasticsearch
- [x] HDFS
- [ ] Spark
- [ ] Storm
- [x] Chronos
- [x] MemSQL

Note: The most up-to-date list of Mesos frameworks that are known to work with
Mantl is always in the [mantl-universe
repo](https://github.com/CiscoCloud/mantl-universe).

### Security

- [x] Manage Linux user accounts
- [x] Authentication and authorization for Consul
- [x] Authentication and authorization for Mesos
- [x] Authentication and authorization for Marathon
- [x] Application load balancer (based on [Traefik](https://traefik.github.io/))
- [x] Application dynamic firewalls (using consul template)

### Operations

- [x] Logging (with the ELK stack)
- [x] Metrics (with the collectd addon)
- [ ] In-service upgrade with rollback
- [ ] Autoscaling of worker nodes
- [ ] Self maintaining system (log rotation, etc)
- [ ] Self healing system (automatic failed instance replacement, etc)

### Supported Platforms

- [x] [Amazon Web Services](https://aws.amazon.com/)
- [x] [CenturyLinkCloud](https://www.ctl.io)
- [x] [Cisco Cloud Services](http://www.cisco.com/c/en/us/solutions/cloud/overview.html)
- [x] [Cisco MetaCloud](http://www.cisco.com/c/en/us/products/cloud-systems-management/metapod/index.html)
- [x] [DigitalOcean](https://www.digitalocean.com/)
- [x] [Joyent Triton](https://www.joyent.com/)
- [x] [Google Compute Engine](https://cloud.google.com/compute/)
- [x] [OpenStack](http://www.openstack.org/)
- [x] [Vagrant](https://www.vagrantup.com/) (Linux/OSX + VirtualBox)
- [ ] [Apache CloudStack](https://cloudstack.apache.org/)
- [ ] [Cisco Unified Computing System](http://www.cisco.com/c/en/us/products/servers-unified-computing/index.html)
- [ ] [Microsoft Azure](https://azure.microsoft.com/en-us/?b=16.17)
- [ ] Vagrant (Windows + VirtualBox)

#### Community Supported Platforms

- [x] Bare Metal
- [x] [IBM Softlayer](http://www.softlayer.com/)
- [x] [VMware vSphere](http://www.vmware.com/products/vsphere/)

Please see [milestones](https://github.com/mantl/mantl/milestones) for
more details on the roadmap.

## Development

If you're interested in contributing to the project, install
[Terraform](https://www.terraform.io/downloads.html) and the Python modules
listed in `requirements.txt` and follow the Getting Started instructions.

### Development Tools

Several tools are available to help with development:

* **Makefile**: Run common development tasks
  ```
  make help        # Show available commands
  make test        # Run tests
  make lint        # Check code quality
  make docs        # Build documentation
  ```

* **Scripts**: Helper scripts in the `scripts` directory
  ```
  ./scripts/lint.sh   # Run linting tools
  ./scripts/test.sh   # Run tests with coverage
  ```

* **Documentation**: To build the docs, run:
  ```
  cd docs
  make html
  ```
  The docs will be output to `_build/html`.

See the [CONTRIBUTING.md](CONTRIBUTING.md) and [DEVELOPMENT.md](DEVELOPMENT.md) files for more detailed information about the development process.

Good issues to start with are marked with the [low hanging
fruit](https://github.com/mantl/mantl/issues?q=is%3Aopen+is%3Aissue+label%3A%22low+hanging+fruit%22)
tag.

To keep your fork up to date.

### 1. Clone your fork:

    git clone git@github.com:YOUR-USERNAME/mantl.git

### 2. Add remote from original repository in your forked repository: 

    cd into/cloned/fork-repo
    git remote add upstream git://github.com/mantl/mantl.git
    git fetch upstream

### 3. Updating your fork from original repo to keep up with their changes:

    git pull upstream master
    
## Getting Support

If you encounter any issues, please open a [Github
Issue](https://github.com/CiscoCloud/mantl) against the project. We review
issues daily.

We also have a [gitter chat room](https://gitter.im/CiscoCloud/mantl). Drop by
and ask any questions you might have. We'd be happy to walk you through your
first deployment.

[Cisco Intercloud Services](https://developer.cisco.com/cloud) provides support
for OpenStack based deployments of Mantl.

## License

Copyright © 2015 Cisco Systems, Inc.

Licensed under the [Apache License, Version
2.0](http://www.apache.org/licenses/LICENSE-2.0) (the "License").

Unless required by applicable law or agreed to in writing, software distributed
under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
CONDITIONS OF ANY KIND, either express or implied. See the License for the
specific language governing permissions and limitations under the License.
