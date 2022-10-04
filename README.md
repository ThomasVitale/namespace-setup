# Namespace Setup

This project provides a [Carvel package](https://carvel.dev/kapp-controller/docs/latest/packaging) for setting up namespaces with the necessary RBAC and Secrets to work with the platform.

## Components

* namespace-setup

## Prerequisites

* Install the [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI to manage Carvel packages in a convenient way.
* Ensure [kapp-controller](https://carvel.dev/kapp-controller) is deployed in your Kubernetes cluster. You can do that with Carvel
[`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

```shell
kapp deploy -a kapp-controller -y \
  -f https://github.com/vmware-tanzu/carvel-kapp-controller/releases/latest/download/release.yml
```

## Dependencies

Namespace Setup requires the Secretgen Controller package to be already installed in the cluster. You can install it from the [Kadras package repository](https://github.com/arktonix/kadras-packages).

## Installation

You can install the Namespace Setup package directly or rely on the [Kadras package repository](https://github.com/arktonix/kadras-packages)
(recommended choice).

Follow the [instructions](https://github.com/arktonix/kadras-packages) to add the Kadras package repository to your Kubernetes cluster.

If you don't want to use the Kadras package repository, you can create the necessary `PackageMetadata` and
`Package` resources for the Namespace Setup package directly.

```shell
kubectl create namespace carvel-packages
kapp deploy -a namespace-setup-package -n carvel-packages -y \
    -f https://github.com/arktonix/namespace-setup/releases/latest/download/metadata.yml \
    -f https://github.com/arktonix/namespace-setup/releases/latest/download/package.yml
```

Either way, you can then install the Namespace Setup package using [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl).

```shell
kctrl package install -i namespace-setup \
    -p namespace-setup.packages.kadras.io \
    -v 0.1.0 \
    -n carvel-packages
```

You can retrieve the list of available versions with the following command.

```shell
kctrl package available list -p namespace-setup.packages.kadras.io
```

You can check the list of installed packages and their status as follows.

```shell
kctrl package installed list -n carvel-packages
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
    -v 0.1.0 \
    -n carvel-packages \
    --values-file values.yml
```

## Documentation

For documentation specific to Cartographer, check out [cartographer.sh](https://cartographer.sh).

## References

This package is inspired by:
* the [examples](https://github.com/vmware-tanzu/cartographer/tree/main/examples) in the Cartographer project;
* the [Cartographer Catalog](https://github.com/vmware-tanzu/cartographer-catalog) package used in Tanzu Community Edition;
* the [set of blueprints](https://github.com/vrabbi/tap-oss/tree/main/packages/ootb-supply-chains) included in an example of Tanzu Application Platform OSS stack.

## Supply Chain Security

This project is compliant with level 2 of the [SLSA Framework](https://slsa.dev).

<img src="https://slsa.dev/images/SLSA-Badge-full-level2.svg" alt="The SLSA Level 2 badge" width=200>
