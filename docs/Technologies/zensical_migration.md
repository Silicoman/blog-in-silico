---
title: Migrer de Mkdocs vers Zensical
description: "un tutoriel pour réaliser vos migrations de mkdocs vanilla,
material for mkdocs vers le site static generator Zensical"
tags:
    - docs
    - migration
---


Ce mode opératoire décrit les grandes lignes pour migrer une documentation
basée sur **MkDocs** ou **Material For MkDocs** (`mkdocs.yml` +
dossier `docs/`) vers **Zensical**.

!!! warning "compatibilités de l'écoystème"
    Si vous avez des usages poussés des plugins MkDocs, il
    est probable que ce guide et l'état actuel de Zensical ne
    remplisse pas 100% des capacités.

## Migrer les dépendances

Si jusqu'à présent, vous utilisiez un `requirements.txt`, vous pouvez supprimer
l'ensemble des dépendances déclarées pour MkDocs.
Dans le cas de uv, vous pouvez executer `uv remove mkdocs` pour que l'uv.lock
soit mise à jour.

!!! info "Des dépendances désormais implicites"
    La librairie Zensical possède en dépendance des extensions python.
    Elle gère ainsi la compatibilité des versions des librairies
    supportées présentées. Vous n'avez donc plus à vous soucier des éléments
    nécessaire décrite dans la documentation Zensical[^1].

[^1]: [Extensions prise en charge officiellement](https://zensical.org/docs/setup/extensions/)
Vous pouvez ajouter la librairie `zensical` avec le choix que vous désirez.

```bash
uv add zensical --group=docs;
uv sync;
```

## Portage des configurations

!!! tip "zensical.toml et mkdocs.yml"
    Zensical garantit une compatibilité dans l'interprétation du fichier
    `mkdocs.yml`. Il s'agit d'un bon point de départ.

L'ensemble des modifications doivent être réalisé sur le fichier
`mkdocs.yml`. Le portage au format toml pourra être réaliser une fois
que la migration soit satisfaisante.

A tout moment vous pouvez vérifier l'état des logs et du rendu par execution de
`zensical serve` ou `uv run zensical serve`.
Puis consulter [localhost](http://localhost:8000).

### Gestion des features et plugins

1. Supprimer la section `plugins`
    Cette fonction n'est pas encore présente dans Zensical et certaines
    fonctions sont natives à Zensical

2. Explicite les configurations preset
    Etant donné que MkDocs avait très peu de markdown_extensions actifs
    par défaut, il est probable que vous ayez ajouter des configurations.
    Vous devrez alors expliciter l'ensemble des configurations.
    Fusionnez l'ensemble des fonctionnalités par défaut[^2] ci-dessous avec
    vos déclarations spécifiques.

    [^2]: [configuration par défaut des extensions](https://zensical.org/docs/setup/extensions/#default-configuration)

```yaml
markdown_extensions:
  - abbr
  - admonition
  - attr_list
  - def_list
  - footnotes
  - md_in_html
  - toc:
      permalink: true
  - pymdownx.arithmatex:
      generic: true
  - pymdownx.betterem
  - pymdownx.caret
  - pymdownx.details
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
  - pymdownx.highlight:
      anchor_linenums: true
      line_spans: __span
      pygments_lang_class: true
  - pymdownx.inlinehilite
  - pymdownx.keys
  - pymdownx.mark
  - pymdownx.smartsymbols
  - pymdownx.superfences:
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format
  - pymdownx.tabbed:
      alternate_style: true
      combine_header_slug: true
  - pymdownx.tasklist:
      custom_checkbox: true
  - pymdownx.tilde
```

### Navigations

Vous avez plusieurs choix possibles.
Vous pouvez supprimer la section nav pour une prise en compte
dynamique la navigation en fonction de l'arborescence docs.
Vous pouvez expliciter la hiérarchie comme dans MkDocs. Il faudra
supprimer les `./` en début de chemin de chaque page si vous avez des 404.


### Personnalisation thème

- `theme` (logo, favicon, palette) -> options sous `[project.theme]`.
- `extra_css`, `extra_javascript` -> copier les assets puis référencer via
  la configuration de Zensical (ou via templates).



Adapter templates et overrides

- Si vous avez un thème Material avec overrides (`overrides/`), identifiez les
  templates Jinja2 modifiés et portez-les dans le dossier de templates Zensical
  (souvent `templates/` ou `theme/overrides`).
- Testez chaque template après build pour corriger les chemins et les blocs
  personnalisés.

## Tester et Déploiement

- Remplacez dans vos workflows (ex: GitHub Actions) l'étape `pip install mkdocs`
  et `mkdocs build` par `pip install -r requirements.txt` (ou `pip install zensical`)
  puis `zensical build` (ou la commande d'export correspondante).
