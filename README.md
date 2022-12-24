# Namespace Setup

<a href="https://slsa.dev/spec/v0.1/levels"><img src="https://slsa.dev/images/gh-badge-level3.svg" alt="The SLSA Level 3 badge"></a>

This project provides a [Carvel package](https://carvel.dev/kapp-controller/docs/latest/packaging) for setting up namespaces with the necessary RBAC and Secrets to work with the [Kadras application platform](https://github.com/arktonix/kadras-application-platform).

## Prerequisites

* Kubernetes 1.24+
* Carvel [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI.
* Carvel [kapp-controller](https://carvel.dev/kapp-controller) deployed in your Kubernetes cluster. You can install it with Carvel [`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

  ```shell
  kapp deploy -a kapp-controller -y \
    -f https://github.com/vmware-tanzu/carvel-kapp-controller/releases/latest/download/release.yml
  ```

## Dependencies

Namespace Setup requires the Secretgen Controller package to be already installed in the cluster. You can install it from the [Kadras package repository](https://github.com/arktonix/kadras-packages).

## Installation

First, add the [Kadras package repository](https://github.com/arktonix/kadras-packages) to your Kubernetes cluster.

  ```shell
  kubectl create namespace kadras-packages
  kctrl package repository add -r kadras-repo \
    --url ghcr.io/arktonix/kadras-packages \
    -n kadras-packages
  ```

Then, install the Namespace Setup package.

  ```shell
  kctrl package install -i namespace-setup \
    -p namespace-setup.packages.kadras.io \
    -v 0.2.0 \
    -n kadras-packages
  ```

### Verification

You can verify the list of installed Carvel packages and their status.

  ```shell
  kctrl package installed list -n kadras-packages
  ```

### Version

You can get the list of Namespace Setup versions available in the Kadras package repository.

  ```shell
  kctrl package available list -p namespace-setup.packages.kadras.io -n kadras-packages
  ```

## Configuration

The Namespace Setup package has the following configurable properties.

| Config | Default | Description |
|-------|-------------------|-------------|
| `namespaces` | `[]` | Configuration for developer namespaces. |
| `registry.server` | `""` | The OCI registry server where the Kadras packages are stored. |
| `registry.username` | `""` | The username for the OCI registry server where the Kadras packages are stored. |
| `registry.password` | `""` | The password for the OCI registry server where the Kadras packages are stored. |
| `gitops.server` | `https://github.com` | The Git server hosting the Git repositories used in the GitOps workflows. |
| `gitops.username` | `""` | The username for the Git server used in the GitOps workflows. |
| `gitops.password` | `""` | The password for the Git server used in the GitOps workflows. |

You can define your configuration in a `values.yml` file.

  ```yaml
  namespaces:
    - name: ""
      exists: ""

  registry:
    server: ""
    username: ""
    password: ""

  gitops:
    server: https://github.com
    username: ""
    password: ""
  ```

Then, reference it from the `kctrl` command when installing or upgrading the package.

  ```shell
  kctrl package install -i namespace-setup \
    -p namespace-setup.packages.kadras.io \
    -v 0.2.0 \
    -n kadras-packages \
    --values-file values.yml
  ```

## Upgrading

You can upgrade an existing package to a newer version using `kctrl`.

  ```shell
  kctrl package installed update -i namespace-setup \
    -v <new-version> \
    -n kadras-packages
  ```

You can also update an existing package with a newer `values.yml` file.

  ```shell
  kctrl package installed update -i namespace-setup \
    -n kadras-packages \
    --values-file values.yml
  ```

## Other

The recommended way of installing the Namespace Setup package is via the [Kadras package repository](https://github.com/arktonix/kadras-packages). If you prefer not using the repository, you can install the package by creating the necessary Carvel `PackageMetadata` and `Package` resources directly using [`kapp`](https://carvel.dev/kapp/docs/latest/install) or `kubectl`.

  ```shell
  kubectl create namespace kadras-packages
  kapp deploy -a namespace-setup-package -n kadras-packages -y \
    -f https://github.com/arktonix/namespace-setup/releases/latest/download/metadata.yml \
    -f https://github.com/arktonix/namespace-setup/releases/latest/download/package.yml
  ```

## References

This package is inspired by the [setup](https://github.com/vrabbi/tap-oss/tree/main/packages/dev-ns-preperation) included in an example of Tanzu Application Platform OSS stack.

## Supply Chain Security

This project is compliant with level 3 of the [SLSA Framework](https://slsa.dev).

<img src="https://slsa.dev/images/SLSA-Badge-full-level3.svg" alt="The SLSA Level 3 badge" width=200>
