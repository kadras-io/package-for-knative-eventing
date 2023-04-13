# Knative Eventing

![Test Workflow](https://github.com/kadras-io/package-for-knative-eventing/actions/workflows/test.yml/badge.svg)
![Release Workflow](https://github.com/kadras-io/package-for-knative-eventing/actions/workflows/release.yml/badge.svg)
[![The SLSA Level 3 badge](https://slsa.dev/images/gh-badge-level3.svg)](https://slsa.dev/spec/v0.1/levels)
[![The Apache 2.0 license badge](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Follow us on Twitter](https://img.shields.io/static/v1?label=Twitter&message=Follow&color=1DA1F2)](https://twitter.com/kadrasIO)

A Carvel package for [Knative Eventing](https://knative.dev/docs/eventing), a cloud-native solution for routing events from event producers to sinks, enabling developers to use an event-driven architecture with their applications.

## 🚀&nbsp; Getting Started

### Prerequisites

* Kubernetes 1.24+
* Carvel [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI.
* Carvel [kapp-controller](https://carvel.dev/kapp-controller) deployed in your Kubernetes cluster. You can install it with Carvel [`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

  ```shell
  kapp deploy -a kapp-controller -y \
    -f https://github.com/carvel-dev/kapp-controller/releases/latest/download/release.yml
  ```

### Installation

Add the Kadras [package repository](https://github.com/kadras-io/kadras-packages) to your Kubernetes cluster:

  ```shell
  kubectl create namespace kadras-packages
  kctrl package repository add -r kadras-packages \
    --url ghcr.io/kadras-io/kadras-packages \
    -n kadras-packages
  ```

<details><summary>Installation without package repository</summary>
The recommended way of installing the Knative Eventing package is via the Kadras <a href="https://github.com/kadras-io/kadras-packages">package repository</a>. If you prefer not using the repository, you can add the package definition directly using <a href="https://carvel.dev/kapp/docs/latest/install"><code>kapp</code></a> or <code>kubectl</code>.

  ```shell
  kubectl create namespace kadras-packages
  kapp deploy -a knative-eventing-package -n kadras-packages -y \
    -f https://github.com/kadras-io/package-for-knative-eventing/releases/latest/download/metadata.yml \
    -f https://github.com/kadras-io/package-for-knative-eventing/releases/latest/download/package.yml
  ```
</details>

Install the Knative Eventing package:

  ```shell
  kctrl package install -i knative-eventing \
    -p knative-eventing.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-packages
  ```

> **Note**
> You can find the `${VERSION}` value by retrieving the list of package versions available in the Kadras package repository installed on your cluster.
> 
>   ```shell
>   kctrl package available list -p knative-eventing.packages.kadras.io -n kadras-packages
>   ```

Verify the installed packages and their status:

  ```shell
  kctrl package installed list -n kadras-packages
  ```

## 📙&nbsp; Documentation

Documentation, tutorials and examples for this package are available in the [docs](docs) folder.
For documentation specific to Knative Eventing, check out [knative.dev](https://knative.dev/docs/eventing).

## 🎯&nbsp; Configuration

The Knative Eventing package can be customized via a `values.yml` file.

  ```yaml
  default_broker:
    enabled: true
  ```

Reference the `values.yml` file from the `kctrl` command when installing or upgrading the package.

  ```shell
  kctrl package install -i knative-eventing \
    -p knative-eventing.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-packages \
    --values-file values.yml
  ```

### Values

The Knative Eventing package has the following configurable properties.

<details><summary>Configurable properties</summary>

| Config | Default | Description |
|-------|-------------------|-------------|
| `default_broker.enabled` | `false` | Enable the in-memory default broker. |
| `default_broker.namespace` | `default` | The namespace where to create the in-memory default broker. |

</details>

## 🛡️&nbsp; Security

The security process for reporting vulnerabilities is described in [SECURITY.md](SECURITY.md).

## 🖊️&nbsp; License

This project is licensed under the **Apache License 2.0**. See [LICENSE](LICENSE) for more information.
