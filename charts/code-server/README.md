# Code-Server Chart

This Helm chart is an umbrella chart that deploys the following applications:

1. **Code-Server** - A cloud-based IDE
2. **Qdrant** - A vector database for AI applications
3. **Ollama** - A tool for running large language models locally

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+

## Installing the Chart

To install the chart with the release name `my-code-server`:

```bash
helm install my-code-server .
```

## Uninstalling the Chart

To uninstall/delete the `my-code-server` deployment:

```bash
helm delete my-code-server
```

## Configuration

The following table lists the configurable parameters of the umbrella chart and their default values.

### Code-Server parameters

| Parameter | Description | Default |
| --------- | ----------- | ------- |
| `code-server.image.repository` | Code-Server image repository | `tom333/coder` |
| `code-server.image.tag` | Code-Server image tag | `latest` |
| `code-server.ingress.enabled` | Enable ingress for Code-Server | `true` |
| `code-server.ingress.hosts` | Hosts for Code-Server ingress | `code.tgu.ovh` |
| `code-server.existingSecret` | Existing secret for sensitive data | `code-server-secret` |

### Qdrant parameters

| Parameter | Description | Default |
| --------- | ----------- | ------- |
| `qdrant.enabled` | Enable Qdrant deployment | `true` |
| `qdrant.apikey.enabled` | Enable API key for Qdrant | `true` |
| `qdrant.apikey.existingSecret` | Existing secret for API key | `qdrant-apikey` |
| `qdrant.apikey.existingSecretKey` | Key in the secret for API key | `apikey` |
| `qdrant.ingress.enabled` | Enable ingress for Qdrant | `true` |
| `qdrant.ingress.hosts` | Hosts for Qdrant ingress | `qdrant.tgu.ovh` |

### Ollama parameters

| Parameter | Description | Default |
| --------- | ----------- | ------- |
| `ollama.enabled` | Enable Ollama deployment | `true` |
| `ollama.ingress.enabled` | Enable ingress for Ollama | `true` |
| `ollama.ingress.hosts` | Hosts for Ollama ingress | `ollama.tgu.ovh` |

## Examples

### Using existing secrets

To use existing secrets for sensitive data, create the secrets in Kubernetes and reference them in the values:

```yaml
code-server:
  existingSecret: code-server-secret

qdrant:
  apikey:
    existingSecret: qdrant-apikey
    existingSecretKey: apikey
```

Then deploy the chart with:

```bash
helm install my-code-server . -f values.yaml
```

For more details on configuring each individual chart, please refer to their respective documentation.