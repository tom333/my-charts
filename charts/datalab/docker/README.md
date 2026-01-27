# MLflow Docker Image avec support PostgreSQL

Cette image étend l'image officielle MLflow pour ajouter le support de PostgreSQL et S3.

## Image pré-construite (Recommandé)

L'image est automatiquement construite et publiée sur GitHub Container Registry lors de modifications du Dockerfile.

```yaml
mlflow:
  image:
    repository: ghcr.io/YOUR_GITHUB_USERNAME/mlflow-postgresql
    tag: v2.18.0  # ou 'latest' pour la dernière version
```

**Note:** Remplacez `YOUR_GITHUB_USERNAME` par votre nom d'utilisateur GitHub.

## Construction manuelle de l'image

Si vous souhaitez construire l'image manuellement :

```bash
# Construire l'image
docker build -t your-registry/mlflow-postgresql:v2.18.0 \
  --build-arg MLFLOW_VERSION=v2.18.0 \
  -f docker/Dockerfile .

# Publier l'image
docker push your-registry/mlflow-postgresql:v2.18.0
```

## Dépendances installées

- `psycopg2-binary` : Driver PostgreSQL pour SQLAlchemy
- `boto3` : Client AWS SDK pour accès S3/RustFS

## Utilisation

L'image est automatiquement disponible sur GitHub Container Registry.

Mettre à jour les valeurs dans `values.yaml` :

```yaml
mlflow:
  image:
    repository: ghcr.io/YOUR_GITHUB_USERNAME/mlflow-postgresql
    tag: v2.18.0  # ou 'latest'
```

## Déclenchement automatique

Le workflow GitHub Actions se déclenche automatiquement lors de :
- Modifications du fichier `charts/datalab/docker/Dockerfile`
- Modifications du workflow `.github/workflows/build-mlflow-image.yml`

L'image est construite pour les architectures :
- `linux/amd64`
- `linux/arm64`
