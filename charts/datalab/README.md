# Datalab Helm Chart

This Helm chart deploys a complete data science environment including:

1. **MLflow** - For experiment tracking and model registry
2. **MinIO** - For object storage
3. **MySQL** - For ZenML metadata storage
4. **ZenML** - For MLOps pipelines

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-datalab`:

```bash
helm install my-datalab .
```

The command deploys Datalab on the Kubernetes cluster with the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the `my-datalab` deployment:

```bash
helm delete my-datalab
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Common parameters

| Name                | Description                                     | Value           |
| ------------------- | ----------------------------------------------- | --------------- |
| `replicaCount`      | Number of replicas to deploy                    | `1`             |

### MinIO parameters

| Name                | Description                                     | Value           |
| ------------------- | ----------------------------------------------- | --------------- |
| `minio.replicas`    | Number of MinIO replicas                        | `1`             |
| `minio.mode`        | MinIO mode (standalone/distributed)             | `standalone`    |
| `minio.persistence.size` | Size of the PVC for MinIO                  | `10Gi`          |
| `minio.ingress.enabled` | Enable ingress for MinIO                    | `true`          |
| `minio.ingress.hosts` | Hosts for MinIO ingress                       | `minio.tgu.ovh` |

### MLflow parameters

| Name                | Description                                     | Value           |
| ------------------- | ----------------------------------------------- | --------------- |
| `mlflow.tracking.auth.enabled` | Enable authentication for MLflow     | `false`         |
| `mlflow.tracking.ingress.enabled` | Enable ingress for MLflow         | `true`          |
| `mlflow.tracking.ingress.hostname` | Hostname for MLflow ingress      | `mlflow.tgu.ovh`|

### MySQL parameters

| Name                | Description                                     | Value           |
| ------------------- | ----------------------------------------------- | --------------- |
| `mysql.auth.database` | MySQL database name                           | `zenml`         |
| `mysql.auth.username` | MySQL username                                | `zenml`         |
| `mysql.auth.password` | MySQL password                                | `zenml`         |
| `mysql.auth.rootPassword` | MySQL root password                         | `password`      |

### ZenML parameters

| Name                | Description                                     | Value           |
| ------------------- | ----------------------------------------------- | --------------- |
| `zenml.zenml.database.url` | Database URL for ZenML                     | `mysql://zenml:zenml@lab-mysql.ia-lab.svc.cluster.local:3306/zenml` |
| `zenml.zenml.ingress.enabled` | Enable ingress for ZenML                | `true`          |
| `zenml.zenml.ingress.host` | Host for ZenML ingress                     | `zenml.tgu.ovh` |

### Persistence parameters

| Name                | Description                                     | Value           |
| ------------------- | ----------------------------------------------- | --------------- |
| `persistence.size`  | Size of the PVC                                 | `10Gi`          |
| `persistence.accessMode` | Access mode for the PVC                    | `ReadWriteOnce` |

## Configuration and installation details

### Persistence

Persistent volumes are created for MinIO and other services that require data persistence. The size and access mode can be configured through the `persistence` section in the values file.

### Secrets

Secrets are managed through the values file. Passwords and other sensitive information should be properly secured and not committed to version control.

## Upgrading

To upgrade the chart with the release name `my-datalab`:

```bash
helm upgrade my-datalab .
```

## Troubleshooting

If you encounter any issues, you can check the logs of the various pods:

```bash
kubectl logs -l app.kubernetes.io/name=minio -n <namespace>
kubectl logs -l app.kubernetes.io/name=mlflow -n <namespace>
kubectl logs -l app.kubernetes.io/name=mysql -n <namespace>
kubectl logs -l app.kubernetes.io/name=zenml -n <namespace>
```

## License

This chart is licensed under the [GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html).