# Jupyter Helm Chart

This chart deploys a Jupyter Notebook/Lab instance on a Kubernetes cluster using the Helm package manager.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-jupyter`:

```bash
helm install my-jupyter .
```

The command deploys Jupyter on the Kubernetes cluster with the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the `my-jupyter` deployment:

```bash
helm delete my-jupyter
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Common parameters

| Name                | Description                                     | Value           |
| ------------------- | ----------------------------------------------- | --------------- |
| `namespace`         | Namespace for the deployment                    | `datalab`       |
| `replicaCount`      | Number of replicas to deploy                    | `1`             |

### Jupyter parameters

| Name                | Description                                     | Value           |
| ------------------- | ----------------------------------------------- | --------------- |
| `image.repository`  | Jupyter image repository                        | `jupyter/minimal-notebook` |
| `image.pullPolicy`  | Image pull policy                               | `IfNotPresent`  |
| `image.tag`         | Jupyter image tag                               | `latest`        |
| `jupyterToken`      | Jupyter access token                            | `""`            |

### Service parameters

| Name                | Description                                     | Value           |
| ------------------- | ----------------------------------------------- | --------------- |
| `service.type`      | Kubernetes Service type                         | `ClusterIP`     |
| `service.port`      | Service HTTP port                               | `8888`          |

### Ingress parameters

| Name                | Description                                     | Value           |
| ------------------- | ----------------------------------------------- | --------------- |
| `ingress.enabled`   | Enable ingress                                  | `false`         |
| `ingress.className` | Ingress class name                              | `""`            |
| `ingress.hosts`     | List of hosts                                   | `[]`            |
| `ingress.tls`       | TLS configuration                               | `[]`            |

### Persistence parameters

| Name                | Description                                     | Value           |
| ------------------- | ----------------------------------------------- | --------------- |
| `persistence.enabled` | Enable persistence                             | `true`          |
| `persistence.size`  | Size of the PVC                                 | `10Gi`          |
| `persistence.storageClass` | Storage class for the PVC                  | `""`            |
| `persistence.accessMode` | Access mode for the PVC                     | `ReadWriteOnce` |

### Resource parameters

| Name                | Description                                     | Value           |
| ------------------- | ----------------------------------------------- | --------------- |
| `resources.requests.cpu` | CPU request                                 | `250m`          |
| `resources.requests.memory` | Memory request                           | `500Mi`         |
| `resources.limits.cpu` | CPU limit                                     | `1000m`         |
| `resources.limits.memory` | Memory limit                             | `1Gi`           |

## Configuration and installation details

### Ingress

This chart provides support for Ingress resource. If you have an available Ingress Controller such as Nginx or Traefik you may want to set `ingress.enabled` to `true` and choose `ingress.className`. Then, you should set the `ingress.hosts` with one or more hosts, for example:

```yaml
ingress:
  enabled: true
  className: "nginx"
  hosts:
    - host: jupyter.example.com
      paths:
        - path: /
          pathType: Prefix
```

### TLS

If your cluster allows automatic creation/config of TLS certificates you can configure the ingress to secure it using TLS. Make sure to update the hosts in the `ingress.tls` section with your domain:

```yaml
ingress:
  enabled: true
  className: "nginx"
  hosts:
    - host: jupyter.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - hosts:
        - jupyter.example.com
      secretName: jupyter-tls
```

### Persistence

The Jupyter image stores data in the `/home/jovyan` directory of the container. By default, a PersistentVolumeClaim is created and mounted into that directory. To disable this behavior, you can change the `persistence.enabled` to `false`.

A default `10Gi` Persistent Volume Claim is created. This is configurable via the `persistence.size` value. If your cluster has dynamic volume provisioning, you don't need to worry about manually creating the volume. If you're using a custom storage class, specify it with the `persistence.storageClass` parameter.

## Upgrading

To upgrade the chart with the release name `my-jupyter`:

```bash
helm upgrade my-jupyter .
```

## Troubleshooting

If you encounter any issues, you can check the logs of the Jupyter pod:

```bash
kubectl logs -l app.kubernetes.io/name=jupyter -n <namespace>
```

## License

Copyright © 2023 My Kluster

This chart is licensed under the [MIT License](https://opensource.org/licenses/MIT).
