# Installation de Packages Pip avec Init Container

Ce chart Jupyter supporte l'installation automatique de packages pip via un Init Container.

## Fonctionnement

1. **Init Container** : S'exécute avant le conteneur principal pour installer les packages
2. **Volume partagé** : Les packages sont installés dans un volume partagé entre l'init container et le conteneur principal
3. **PYTHONPATH** : Le conteneur principal est configuré pour utiliser les packages installés

## Configuration

### Activation de base

```yaml
pipPackages:
  enabled: true
  packages:
    - pandas
    - numpy
    - matplotlib
```

### Configuration avancée

```yaml
pipPackages:
  enabled: true
  packages:
    - pandas==2.0.3
    - numpy==1.24.3
    - scikit-learn>=1.3.0
  upgradeStrategy: "--upgrade --no-cache-dir"
  persistInstallation: true  # Persiste les packages entre les redémarrages
  storageSize: "2Gi"         # Taille du volume pour la persistance
```

## Options disponibles

- `enabled`: Active/désactive l'installation de packages
- `packages`: Liste des packages à installer (supporte les versions)
- `upgradeStrategy`: Options pip (par défaut: `--upgrade --no-cache-dir`)
- `persistInstallation`: Persiste les packages dans un PVC
- `storageSize`: Taille du volume de persistance

## Avantages de cette approche

1. **Séparation des responsabilités** : L'installation se fait avant le démarrage de Jupyter
2. **Robustesse** : Si l'installation échoue, le pod ne démarre pas
3. **Performance** : Avec la persistance, les packages ne sont installés qu'une fois
4. **Flexibilité** : Support des versions exactes et des contraintes
5. **Logs séparés** : Les logs d'installation sont visibles dans l'init container

## Utilisation

1. Configurez votre `values.yaml` avec les packages désirés
2. Déployez le chart : `helm upgrade --install jupyter ./jupyter`
3. Vérifiez l'installation dans les logs de l'init container :
   ```bash
   kubectl logs <pod-name> -c pip-installer
   ```

## Exemple complet

Voir `examples/values-with-pip-packages.yaml` pour un exemple complet avec des packages de data science.

## Dépannage

- **Init container en erreur** : Vérifiez les logs avec `kubectl logs <pod> -c pip-installer`
- **Packages non trouvés** : Vérifiez que `PYTHONPATH` est correctement configuré
- **Permissions** : Assurez-vous que l'utilisateur peut écrire dans `/shared-packages`