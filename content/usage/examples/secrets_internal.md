---
title: "Internal Secrets"
toc: true
weight: 1
description: >
  Example Pipeline with internal secrets
---

Example [Yaml](https://yaml.org/spec/) configuration for a project requiring a secrets to be used within a step

## Scenario

User is looking to create a pipeline that can inject configuration that can not be placed into a Yaml file. A simple example would be producing a Docker image with username and password.

{{% alert title="Note:" color="primary" %}}
It is assumed you have created secrets `docker_username` and `docker_password` in the web interface or [CLI](/docs/reference/cli/).
{{% /alert %}}

The examples show a pipeline using repository secrets. Vela contains three secret types: repository, organization, and shared. For examples on organization and shared, please see the [secret concepts](/docs/tour/secrets/) documentation.

### Steps

The following [pipeline concepts](/docs/tour/) are being used in the pipeline below:

* [Steps](/docs/tour/steps/)
  * [Image](/docs/tour/image/)
  * [Pull](/docs/tour/image/)
  * [Secrets](/docs/tour/secrets/)
  * [Parameters](/docs/tour/plugins/)
* [Secrets](/docs/tour/secrets/)

The following [Vela plugins](/docs/tour/) are being used in the pipeline below:

* [Docker](/docs/plugins/registry/pipeline/docker/)

{{% alert title="Note:" color="primary" %}}
Pipeline must be stored in base of repository as `.vela.yml` or `.vela.yaml`

It is recommended to pin `image:` versions for production pipelines
{{% /alert %}}

```yaml
version: "1"

steps:
  - name: publish hello world
    image: target/vela-docker:latest
    pull: always
    secrets: [ docker_username, docker_password ]
    parameters:
      registry: index.docker.io
      repo: index.docker.io/vela/hello-world


secrets:
  - name: docker_username
    key: vela/hello-world/docker_username
    engine: native
    type: repo

  - name: docker_password
    key: vela/hello-world/docker_password
    engine: native
    type: repo
```

### Stages

The following [pipeline concepts](/docs/tour/) are being used in the pipeline below:

* [Stages](/docs/tour/stages/)
  * [Steps](/docs/tour/steps/)
  * [Image](/docs/tour/image/)
  * [Pull](/docs/tour/image/)
  * [Secrets](/docs/tour/secrets/)
  * [Parameters](/docs/tour/plugins/)
* [Secrets](/docs/tour/secrets/)

The following [Vela plugins](/docs/tour/) are being used in the pipeline below:

* [Docker](/docs/plugins/registry/pipeline/docker/)

{{% alert title="Note:" color="primary" %}}
Pipeline must be stored in base of repository as `.vela.yml` or `.vela.yaml`

It is recommended to pin `image:` versions for production pipelines
{{% /alert %}}

```yaml
version: "1"

stages:
  docker:
    steps:
      - name: publish hello world
        image: target/vela-docker:latest
        pull: always
        secrets: [ docker_username, docker_password ]
        parameters:
          registry: index.docker.io
          repo: index.docker.io/vela/hello-world

secrets:
  - name: docker_username
    key: vela/hello-world/docker_username
    engine: native
    type: repo

  - name: docker_password
    key: vela/hello-world/docker_password
    engine: native
    type: repo
```
