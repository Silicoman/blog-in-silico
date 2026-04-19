---
title: Zensical, Static Site Generator
tags:
    - docs
    - SSG
---

Zensical est un site static generator (SSG). Il vous permet de générer un site
statique à partir de fichier markdown. Il devrait à terme prendre en charge
le standard CommonMark. L'équipe derrière le projet possède une dizaine
d'années d'expérience sur les problématiques de SSG. Combiné avec un
développement en Rust, l'équipe a toutes les chances de mettre en oeuvre
un moteur robuste et efficace.

## A l'origine de Zensical

En novembre 2025[^1] l'auteur [Martin Donath](https://github.com/squidfunk)
de `material for mkdocs` annonce la sortie de Zensical.
Suite aux déboires de la maintenance de `mkdocs`[^2]. Les développeurs de
material y ont vu une opportunité de s'affranchir du moteur `mkdocs`
afin de lever toutes les contraintes qu'ils subissaient jusqu'à présent.
Il est né du besoin d'offrir une expérience de documentation moderne et
cohérente — comparable à celle fournie par Material for MkDocs — sans
rester lié à l'écosystème MkDocs. Cependant, afin de financer leur travail,
ils proposent du support commercial avec leur offre `Zensical Spark` pour
faciliter la migration.

!!! quote
    Our new approach distills a decade of experience maintaining Material for MkDocs, allowing us to professionalize and scale our efforts from a small team to a fully organized operation, while keeping the software free and open to everyone.

[^1]: [Goodbye-github from material for mkdocs](https://squidfunk.github.io/mkdocs-material/blog/2025/11/18/goodbye-github-discussions/)
[^2]: [Collapse of mkdocs](https://fpgmaas.com/blog/collapse-of-mkdocs)

## Viabilité

### Commaunauté

Le projet `material for mkdocs` possède une grosse communauté. Le projet `Zensical`
s'appuie sur cette base communautaire en facilitant la migration de mkdocs vers ce
nouveau site générateur. Ce choix stratégique permet d'éviter une fuite de la
communauté vers d'autres sites générateurs. Globalement des différentes discussions
des projets devant migrer et de l'historique des *stars* github, la communauté
à l'air de suivre.

- __Historique des Stars de `material for mkdocs` et `Zensical` avec une échelle log__

    ---
    [![Star History Chart par log scale](https://api.star-history.com/chart?repos=zensical/zensical%2Csquidfunk/mkdocs-material&type=date&logscale&legend=top-left)](https://www.star-history.com/?repos=zensical%2Fzensical%2Csquidfunk%2Fmkdocs-material&type=date&logscale=&legend=top-left)

### Licence OpenSource

Au première abord, on pourrait douter sur la question du modèle économique
de l'entreprise Zensical.
Cependant, à la différence d'une startup qui débarque de nul part,
Martin Donath a maintenu pendant 10 ans en licence MIT son projet material
à travers des dons (200K/an[^3] à la fin).
Zensical semble suivre le même état d'esprit. De la roadmap, Zensical cherche
a mettre en oeuvre toutes les fonctionnalités présentes dans l'écosystème
`mkdocs` et `material for mkdocs`. De mon avis, la stratégie est d'assécher
les forks potentiels et l'éclatement de l'écosystème. L'équipe a recruté
le développeur de mkdocstring[^4] pour porter le plugin critique.
A partir de cette base solide, il est
possible que Zensical passe avec un modèle Open Core si le financement
ne suit pas suffisamment. Ils pourraient pivoter en se financant auprès des
acteurs économiques par le développement d'extension payante
pour des intégrations avec des solutions maison.

Le sujet est à surveiller mais je suis plutôt confiant que dans 6-12 mois,
la licence sera toujours permissive et que l'on est à disposition la très
grande majorité des fonctionnalités.

[^3]: [Interview Martin Donath](https://talkpython.fm/episodes/show/542/zensical-a-modern-static-site-generator)
[^4]: [Equipe Zensical](https://zensical.org/about/team/)

## Démarrer un projet avec Zensical

### Créons un projet de zéro avec uv

!!! tip "uv"
    Afin d'avoir un venv dynamique, de gérer les dépendances, je
    détaille le mode opératoire avec **uv**. Vous pouvez reproduire
    avec **pip**.[^5]

[^5]: [Guide installation Zensical](https://zensical.org/docs/get-started/#install-with-pip-linux)

1. créons le squelette d'un projet (on supprime le main.py)
```shell
mkdir mydemo;
cd mydemo;
uv init --name mydemo;
rm main.py
```
2. Installer Zensical:
```shell
uv add zensical
uv sync
```

3. Initialiser `docs` et tester un site d'exemple :
```sh
uv run zensical new
uv run zensical serve  # --port 8000
```
4. Vérifier la sortie dans votre navigateur [localhost:8000](http://localhost:8000)

A ce stade, vous avez votre première démo d'un site statique opérationnel.

- **docs**: Dossier source des pages Markdown (documentation à éditer).
- **pyproject.toml**: Configuration du projet Python (métadonnées et dépendances).
- **README.md**: Présentation du projet, instructions d'installation et usage.
- **site**: Site statique généré (HTML/CSS/JS — sortie de build) à ajouter en .gitignore
- **uv.lock**: Lockfile géré par uv — enregistre versions/dépendances.
- **zensical.toml**: Configuration spécifique à Zensical (paramètres du SSG/site).


### Fonctionnalités disponibles

- Création et rendu d'une page simple
- Ajout et optimisation d'images
- Gestion des métadonnées (tags, catégories, taxonomies)
- Templates, inclusions et layouts
- Rebuild incrémental / mode watch

## Points forts (exemples)
- Simplicité d'usage pour les rédacteurs
- Intégration des bonnes pratiques UX héritées de Material
- Convention over configuration (si applicable)

## Limitations
- Écosystème de plugins plus réduit que les grands SSG
- Fonctionnalités spécifiques manquantes pour sites très complexes


## Annexes
- Liens : dépôt officiel, documentation, changelog.
