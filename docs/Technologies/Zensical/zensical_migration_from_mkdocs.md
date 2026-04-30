---
title: Migrer de MkDocs vers Zensical
description: "Tutoriel pour migrer une documentation MkDocs
(ou Material for MkDocs) vers le générateur statique Zensical."
tags:
    - docs
    - migration
    - tutoriel
---

Ce guide décrit les étapes pour migrer une documentation basée sur **MkDocs**
(ou **Material for MkDocs**) — `mkdocs.yml` + dossier `docs/` — vers **Zensical**.

Zensical a dans son backlog la mise en oeuvre d'un outil de migration. Cependant,
il y a un peu de travail. Si vous désirez dès à présent basculer sur Zensical,
la suite devrait vous donner les grandes lignes.

!!! warning "Compatibilité de l'écosystème"
    Si vous utilisez intensivement des plugins MkDocs, il est possible que ce
    guide et l'état actuel de Zensical ne couvrent pas toutes les
    fonctionnalités. Procédez par étapes. La compatibilité complète viendra
    avec le temps et est explicitement annoncé par Zensical[^0].
    En revanche, je vous conseille de regarder les forks
    [ProperDocs]((https://properdocs.org/))
    (par [*oprypin*](https://github.com/oprypin) un ancien mainteneur de MkDocs)
    et [MaterialX](https://jaywhj.github.io/mkdocs-materialx/)
    (par un outsider [*jaywhj*](https://github.com/jaywhj)). Leur pérennité
    n'est pas garanti car la communauté.

[^0]: [Compatibilité MkDocs vers Zensical](https://zensical.org/compatibility/)

!!! tips "De nombreux projets ont déjà migrés"
    Suite au message d'alerte de depréciation de Material for MkDocs dans les
    logs de build, de nombreux projets ont migrés depuis mars sur Zensical.
    La migration est relativement simple pour certains comme sur netbox[^10].
    Ce guide est construit à partir de mon expérience et des reviews des
    projets.

[^10]: [Merge Request de migration de netbox vers zensical](https://github.com/netbox-community/netbox/pull/21742/changes)

## Migrer les dépendances

Si votre projet utilisait un `requirements.txt`, vous pouvez supprimer les
dépendances spécifiques à MkDocs. Si vous utilisez un gestionnaire de
dépendances comme `uv` (optionnel), vous pouvez exécuter :

```bash
# avec uv (exemple)
uv remove mkdocs
uv remove mkdocs-material
uv add zensical --group=docs
uv sync

# ou sans uv :
pip install zensical
```

!!! info "Dépendances implicites"
    La distribution `zensical` embarque ou requiert certaines extensions
    Markdown courantes. Elle gère ainsi la compatibilité des versions des
    extensions. Consultez la documentation officielle[^1] pour la
    liste des extensions prises en charge et leur configuration par défaut.

[^1]: [Extensions prises en charge (Zensical)](https://zensical.org/docs/setup/extensions/)

## Portage des configurations

!!! tip "zensical.toml et mkdocs.yml"
    Zensical sait interpréter de base un `mkdocs.yml` — c'est un bon point de
    départ pour vérifier le rendu avant de convertir définitivement la
    configuration en `zensical.toml`.

Pendant la phase de migration, effectuez les modifications dans `mkdocs.yml`
et vérifiez le rendu localement :

```bash
# servir localement (si zensical est installé)
zensical serve

# ou via uv
uv run zensical serve

# puis ouvrir http://localhost:8000
```

### Gestion des features et plugins

1. Plugins (section à supprimer)
    - La section `plugins` de MkDocs peut contenir des fonctions non prises
            en charge telles quelles. A valider dans la roadmap Zensical.

2. Configurations par défaut / extensions Markdown
    - MkDocs (Material) active quelques extensions. Zensical peut
            avoir une configuration différente et active massivement les
            fonctionnalités: explicitez la liste
            d'extensions nécessaires et fusionnez-la avec les valeurs par défaut.

Exemple (à fusionner avec celui de votre projet) :

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

### Gestion de la navigation

Deux options :

- Supprimer la section `nav` pour utiliser une navigation générée dynamiquement
    depuis l'arborescence `docs/`.
- Conserver et expliciter la hiérarchie (comme dans MkDocs). Si vos chemins
    étaient relatifs avec un préfixe `./`, supprimez ce préfixe pour éviter des
    erreurs 404.

### Personnalisation du thème et assets

Vous désirez peut être utiliser le nouveau thème natif Zensical.

```
theme:
  variant: modern
```

Avec le nouveau thème, si vous utilisez des css personnalisés, il faudra faire
une relecture de la cohérence avec le nouveau thème.

## Tester et déploiement

Remplacez dans vos workflows (ex. GitHub Actions) les étapes `pip install mkdocs`
et `mkdocs build` par une séquence équivalente :

```yaml
- name: Build documentation
    run: |
        uv sync
        uv run zensical build --config-file mkdocs.yml --strict
```

## Conclusion

En fonction de la complexité de la personnalisation, il faudra probablement
investir un peu plus au cas par cas que ce guide.

Une fois le rendu satisfaisant, vous pourrez convertir la configuration `mkdocs.yml`
vers `zensical.toml` (format TOML), en regroupant les options sous
`[project]`, `[markdown]`, etc. Vous couperez symboliquement les ponts avec mkdocs.
