> # ⚠️ Deprecation notice
>
> As of 1.2.0 the Redis operator comes with a [Kustomize setup](https://github.com/spotahome/redis-operator#using-kustomize) based on this one.
> Therefore, there is no need to maintain this repository anymore.

# Kustomize deployment manifests for [spotahome/redis-operator](https://github.com/spotahome/redis-operator)

This repo contains manifests to install [spotahome/redis-operator](https://github.com/spotahome/redis-operator) using [Kustomize](https://kustomize.io/).


## Installation

To install the latest version of the operator, run the following command:
```shell
kustomize build github.com/sagikazarmark/kustomize-spotahome-redis-operator/overlays/default | kubectl apply -f -
```

The _default_ version installs the operator and every other manifest it needs for running (roles, service account, etc).

There are two additional versions (overlays) bundled with this repository:

- _minimal_ only installs the operator itself without components like RBAC or Service Account
- _full_ installs additional components, like a Prometheus ServiceMonitor


## Repo structure

The repository uses [kustomize components](https://kubectl.docs.kubernetes.io/guides/config_management/components/)
to provide a flexible way to install the operator.

- **base**: Base manifests for the operator.
- **components**: Additional components to improve the installation of redis-operator.
- **overlays**: Final, installable manifests. (See the list of "versions" above)

```
├── base
├── components
│   ├── monitoring
│   ├── rbac
│   ├── rbac-full
│   ├── resources
│   └── version
└── overlays
    ├── minimal
    ├── default
    └── full
```

## Create your own version

You can easily create your own overlay by reusing the components in this repository.
All you need is your own `kustomization.yaml` file referencing components and/or overlays in this repository:

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

namespace: redis-operator

commonLabels:
    foo: bar

resources:
  - github.com/sagikazarmark/kustomize-spotahome-redis-operator/overlays/full
```


## Attributions

Some of the manifests in this repository are based on the original project's [examples](https://github.com/spotahome/redis-operator/tree/master/example).


## License

The project is licensed under the [MIT License](LICENSE).
