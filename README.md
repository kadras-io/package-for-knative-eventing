# Knative Eventing

This project provides a [Carvel package](https://carvel.dev/kapp-controller/docs/latest/packaging) for [Knative Eventing](https://knative.dev/docs/eventing), a solution for routing events from event producers to sinks, enabling developers to use an event-driven architecture with their applications.

## Components

* Knative Eventing

## Prerequisites

* Install the [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI to manage Carvel packages in a convenient way.
* Ensure [kapp-controller](https://carvel.dev/kapp-controller) is deployed in your Kubernetes cluster. You can do that with Carvel
[`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

```shell
kapp deploy -a kapp-controller -y \
  -f https://github.com/vmware-tanzu/carvel-kapp-controller/releases/latest/download/release.yml
```

## Installation

You can install the Knative Eventing package directly or rely on the [Kadras package repository](https://github.com/arktonix/carvel-packages)
(recommended choice).

Follow the [instructions](https://github.com/arktonix/carvel-packages) to add the Kadras package repository to your Kubernetes cluster.

If you don't want to use the Kadras package repository, you can create the necessary `PackageMetadata` and
`Package` resources for the Knative Eventing package directly.

```shell
kubectl create namespace carvel-packages
kapp deploy -a knative-eventing-package -n carvel-packages -y \
    -f https://github.com/arktonix/package-for-knative-eventing/releases/latest/download/metadata.yml \
    -f https://github.com/arktonix/package-for-knative-eventing/releases/latest/download/package.yml
```

Either way, you can then install the Knative Eventing package using [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl).

```shell
kctrl package install -i knative-eventing \
    -p knative-eventing.packages.kadras.io \
    -v 1.8.0 \
    -n carvel-packages
```

You can retrieve the list of available versions with the following command.

```shell
kctrl package available list -p knative-eventing.packages.kadras.io
```

You can check the list of installed packages and their status as follows.

```shell
kctrl package installed list -n carvel-packages
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
    -v 1.8.0 \
    -n carvel-packages \
    --values-file values.yml
```

## Documentation

For documentation specific to Knative Eventing, check out [knative.dev](https://knative.dev).

## Supply Chain Security

This project is compliant with level 2 of the [SLSA Framework](https://slsa.dev).

<img src="https://slsa.dev/images/SLSA-Badge-full-level2.svg" alt="The SLSA Level 2 badge" width=200>
