# Knative Eventing

<a href="https://slsa.dev/spec/v0.1/levels"><img src="https://slsa.dev/images/gh-badge-level3.svg" alt="The SLSA Level 3 badge"></a>

This project provides a [Carvel package](https://carvel.dev/kapp-controller/docs/latest/packaging) for [Knative Eventing](https://knative.dev/docs/eventing), a solution for routing events from event producers to sinks, enabling developers to use an event-driven architecture with their applications.

## Prerequisites

* Kubernetes 1.24+
* Carvel [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI.
* Carvel [kapp-controller](https://carvel.dev/kapp-controller) deployed in your Kubernetes cluster. You can install it with Carvel [`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

  ```shell
  kapp deploy -a kapp-controller -y \
    -f https://github.com/vmware-tanzu/carvel-kapp-controller/releases/latest/download/release.yml
  ```

## Installation

First, add the [Kadras package repository](https://github.com/kadras-io/kadras-packages) to your Kubernetes cluster.

  ```shell
  kubectl create namespace kadras-packages
  kctrl package repository add -r kadras-repo \
    --url ghcr.io/kadras-io/kadras-packages \
    -n kadras-packages
  ```

Then, install the Knative Eventing package.

  ```shell
  kctrl package install -i knative-eventing \
    -p knative-eventing.packages.kadras.io \
    -v 1.8.4+kadras.1 \
    -n kadras-packages
  ```

### Verification

You can verify the list of installed Carvel packages and their status.

  ```shell
  kctrl package installed list -n kadras-packages
  ```

### Version

You can get the list of Knative Eventing versions available in the Kadras package repository.

  ```shell
  kctrl package available list -p knative-eventing.packages.kadras.io -n kadras-packages
  ```

## Configuration

The Knative Eventing package has the following configurable properties.

| Config | Default | Description |
|-------|-------------------|-------------|
| `default_broker.enabled` | `false` | Enable the in-memory default broker. |
| `default_broker.namespace` | `default` | The namespace where to create the in-memory default broker. |

You can define your configuration in a `values.yml` file.

  ```yaml
  default_broker:
    enabled: false
    namespace: default
  ```

Then, reference it from the `kctrl` command when installing or upgrading the package.

  ```shell
  kctrl package install -i knative-eventing \
    -p knative-eventing.packages.kadras.io \
    -v 1.8.4+kadras.1 \
    -n kadras-packages \
    --values-file values.yml
  ```

## Upgrading

You can upgrade an existing package to a newer version using `kctrl`.

  ```shell
  kctrl package installed update -i knative-eventing \
    -v <new-version> \
    -n kadras-packages
  ```

You can also update an existing package with a newer `values.yml` file.

  ```shell
  kctrl package installed update -i knative-eventing \
    -n kadras-packages \
    --values-file values.yml
  ```

## Other

The recommended way of installing the Knative Eventing package is via the [Kadras package repository](https://github.com/kadras-io/kadras-packages). If you prefer not using the repository, you can install the package by creating the necessary Carvel `PackageMetadata` and `Package` resources directly using [`kapp`](https://carvel.dev/kapp/docs/latest/install) or `kubectl`.

  ```shell
  kubectl create namespace kadras-packages
  kapp deploy -a knative-eventing-package -n kadras-packages -y \
    -f https://github.com/kadras-io/package-for-knative-eventing/releases/latest/download/metadata.yml \
    -f https://github.com/kadras-io/package-for-knative-eventing/releases/latest/download/package.yml
  ```

## Support and Documentation

For support and documentation specific to Knative Eventing, check out [knative.dev](https://knative.dev).

## Supply Chain Security

This project is compliant with level 3 of the [SLSA Framework](https://slsa.dev).

<img src="https://slsa.dev/images/SLSA-Badge-full-level3.svg" alt="The SLSA Level 3 badge" width=200>
