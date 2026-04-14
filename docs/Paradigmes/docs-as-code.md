
## Documentation As Code 

Docs As Code est un exemple puissant de ce que signifie appliquer DevOps à la documentation.

Cette pratique consiste à traiter la documentation comme du code :

- stockée dans le même dépôt que l’application ou le service,
- versionnée avec Git,
- revue via des merges requests ou pull requests,
- publiée automatiquement par des pipelines CI/CD.

### Pourquoi Docs As Code ?

- cohérence : la documentation évolue avec le code,
- traçabilité : on retrouve facilement qui a modifié quoi et pourquoi,
- qualité : les mêmes contrôles que pour le code peuvent s’appliquer (spelling, liens, mise en forme),
- collaboration : les rédacteurs et développeurs peuvent collaborer dans le même workflow.

### Exemple de chaîne Docs As Code

1. Un contributeur modifie un fichier Markdown `docs/Paradigmes/index.md`.
2. Une pipeline CI exécute la validation du format, la génération de la preview et les vérifications de liens.
3. La documentation est revue puis fusionnée.
4. Le contenu est publié automatiquement sur le site.
