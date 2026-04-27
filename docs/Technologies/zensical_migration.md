---
title: Migrer de MkDocs vers Zensical
description: "Tutoriel pour migrer une documentation MkDocs
(ou Material for MkDocs) vers le générateur statique Zensical."
tags:
    - docs
    - migration
---

Ce guide décrit les étapes pour migrer une documentation basée sur **MkDocs**
(ou **Material for MkDocs**) — `mkdocs.yml` + dossier `docs/` — vers **Zensical**.

!!! warning "Compatibilité de l'écosystème"
        Si vous utilisez intensivement des plugins MkDocs, il est possible que ce
        guide et l'état actuel de Zensical ne couvrent pas toutes les
        fonctionnalités. Procédez par étapes.

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

Une fois le rendu satisfaisant, convertissez progressivement la configuration
vers `zensical.toml` (format TOML), en regroupant les options sous
`[project]`, `[markdown]`, `[build]`, etc.

### Gestion des features et plugins

1. Plugins (section à supprimer)
        - La section `plugins` de MkDocs peut contenir des fonctions non prises
            en charge telles quelles. A valider dans la roadmap Zensical.

2. Configurations par défaut / extensions Markdown
        - MkDocs (Material) active certaines extensions Markdown. Zensical peut
            avoir une configuration différente : explicitez la liste
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

### Navigation

Deux options :

- Supprimer la section `nav` pour utiliser une navigation générée dynamiquement
    depuis l'arborescence `docs/`.
- Conserver et expliciter la hiérarchie (comme dans MkDocs). Si vos chemins
    étaient relatifs avec un préfixe `./`, supprimez ce préfixe pour éviter des
    404.

### Personnalisation du thème et assets

- `theme` (logo, favicon, palette) → options sous `[project.theme]`.
- `extra_css`, `extra_javascript` → copiez les assets dans `assets/` et
    référencez-les depuis la configuration ou via des templates.

Adapter templates et overrides
- Si vous avez des overrides Jinja2 (`overrides/`), identifiez les templates
    modifiés et portez-les dans le dossier de templates de Zensical
    (généralement `templates/` ou `theme/overrides`).
- Testez chaque template après `zensical build` pour corriger chemins et blocs.


## Exemple minimal de `zensical.toml` (indicatif)

Le contenu ci‑dessous est un exemple minimaliste à adapter selon vos besoins
et la version de Zensical :

```toml
# zensical.toml — exemple indicatif
[project]
name = "Mon site"
description = "Documentation"
[project.theme]
name = "modern"
logo = "assets/images/logo.svg"
favicon = "assets/images/favicon.ico"

[[pages]]
path = "index.md"
title = "Accueil"

[build]
output_dir = "site"

[markdown]
extensions = ["abbr","admonition","attr_list","def_list","footnotes","md_in_html","toc"]
```

## Tester et déploiement

Remplacez dans vos workflows (ex. GitHub Actions) les étapes `pip install mkdocs`
et `mkdocs build` par une séquence équivalente :

```yaml
- name: Build documentation
    run: |
        pip install zensical
        zensical build
```

## Ressources

- [Extensions prises en charge (Zensical)](https://zensical.org/docs/setup/extensions/)
- Documentation Zensical : https://zensical.org/docs/
