---
title: Documentation As Code
---

## L'usage des Wikis au sein des projets

En travaillant sur un projet, vous aurez tôt ou tard à affronter le problème
de la documentation pas à jour ou de la documentation manquante, souvent
perdue dans les limbes d'un répertoire partagé ou d'une GED.

Avec l'essor de l'agilité, une dérive fréquente a été l'inconsistance de la
documentation lorsque celle-ci n'est pas maintenue au fil de l'eau.

Pendant des années, j'étais grand fan de la solution Confluence pour son
approche Wiki. Diffuser la connaissance est une nécessité pour tendre vers
la standardisation et augmenter le niveau de connaissance des collaborateurs.

!!! quote "Accelerate State of DevOps 2023"
    In addition to improving technical capabilities, we
    found that quality documentation has a positive impact
    on an individual’s well-being: it reduces burnout,
    increases job satisfaction, and increases productivity.
    We found that some of this impact is because quality
    documentation increases knowledge sharing.

Mais à grande échelle, les Wikis montrent des limites :

- la recherche devient complexe,
- la responsabilité est diluée,
- les informations se dupliquent,
- les workflows diffèrent selon les personas (développeurs, métiers,
  fonctionnels),
- la publication en version reste souvent manuelle ou sous-optimale.

Par expérience, je considère qu'une page non mise à jour depuis plusieurs mois est souvent déjà obsolète.
Une documentation sur un produit rédigée par une équipe distante des
responsables du produit est souvent moins pertinente.

En développant des utilitaires, pour gagner en engagement, la documentation
est indispensable. Rapidement, l'évidence est de basculer sur la
**Documentations as Code** pour simplifier son propre flux de travail.


## Pourquoi Docs As Code ?

!!! quote "@Eric Holscher & the Write the Docs community"
    Documentation as Code (Docs as Code) refers to a philosophy that you
    should be writing documentation with the same tools as code:

    * Issue Trackers
    * Version Control (Git)
    * Plain Text Markup (Markdown, reStructuredText, Asciidoc)
    * Code Reviews
    * Automated Tests

Docs As Code est une approche qui traite la documentation comme du code :

- contenu stocké en texte brut dans un dépôt Git,
- versionné avec les mêmes règles que le code source,
- soumis à revue via merge requests / pull requests,
- publié automatiquement par des pipelines CI/CD.

### Ce que cela change

- cohérence : la documentation évolue avec le code et les sources,
- traçabilité : on retrouve l'historique exact des modifications,
- qualité : on peut appliquer des validations automatiques,
- collaboration : la documentation entre dans le même flux de travail que
  le développement.


## Quelle solution adaptée ?

Le choix d'une solution Docs As Code dépend du contexte du projet.

### Critères de sélection

- audience : documentation utilisateur, développeur, ou produit ?
- format : pages statiques, guides API, tutoriels, notes d'architecture ?
- complexité : besoin de recherche avancée, navigation multi-niveaux, ou
  simple site de docs.

### Solutions courantes

- `MkDocs` / `Zensical` : simple à configurer, bon pour les docs techniques et les guides
- `Sphinx` : puissant pour la documentation Python et les docs complexes.
- `Docusaurus` : bien adapté aux sites modernes React et à la documentation
  produit.
- `Hugo` / `Jekyll` : générateurs statiques flexibles pour des sites plus
  personnalisés.


### Outils complémentaires

- `Markdown` ou `reStructuredText` pour la rédaction,
- `Vale` / `markdownlint` / `remark` pour le linting,
- `htmlproofer` / `linkcheck` pour valider les liens,
- `pandoc` pour les conversions quand nécessaire.

## Mise en pratique

### Workflow typique

1. Créer ou modifier un fichier Markdown dans le dépôt `docs/`.
2. Ouvrir une merge request ou pull request.
3. Exécuter une pipeline CI qui :
   - valide le format Markdown,
   - vérifie les liens internes et externes,
   - génère une preview du site statique,
   - produit éventuellement un rapport de qualité.
4. Revoir la documentation comme du code.
5. Fusionner et publier automatiquement.

### Exemple simple

- dépôt : `mon-projet/`
- dossier docs : `docs/`
- génération : `mkdocs build`
- publication : GitHub Pages ou GitLab Pages

Avec un pipeline CI :

- `checkout`
- `pip install mkdocs mkdocs-material`
- `mkdocs build`
- `mkdocs serve` (preview locale) ou `mkdocs gh-deploy`

### Bonnes pratiques

- versionner la documentation avec la même branche que le code lorsque
  le contenu dépend directement d'une fonctionnalité,
- utiliser des templates et des modèles pour homogénéiser les pages,
- appliquer des règles de style et des validations automatiques,
- automatiser la publication pour éviter les oublis,
- garder la documentation près du code pour limiter les décalages.

## Références

- https://www.writethedocs.org/guide/docs-as-code/
- https://services.google.com/fh/files/misc/2023_final_report_sodr.pdf
