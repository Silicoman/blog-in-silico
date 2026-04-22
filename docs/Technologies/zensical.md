---
title: Zensical, Static Site Generator
description: tutoriel mise en oeuvre Zensical
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
En revanche, ils proposent du support commercial avec leur offre
[`Zensical Spark`](https://zensical.org/spark/tiers) pour
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
de l'entreprise Zensical. L'écosystème des SSG est déjà bien rempli avec par
exemple Astro, Hugo, Jerkyll ou Sphinx.
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
grande majorité des fonctionnalités de ce qu'on trouve en opensource.

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

A ce stade, vous avez votre première démo d'un site statique opérationnel en local. Vous pouvez modifier
les pages, elles seront rechargés dans le navigateur
à la volée.

```text
├── docs : Dossier source des pages Markdown (documentation à éditer).
|   ├── index.md : page d'accueil de votre documentation
|   └── markdown.md : page titre markdown (surcharger par title)
├── pyproject.toml : Configuration du projet Python (métadonnées et dépendances).
├── README.md : Présentation du projet, instructions d'installation et usage.
├── site : Site statique généré (HTML/CSS/JS — sortie de build) à ajouter en .gitignore
├── uv.lock : Lockfile géré par uv — enregistre versions/dépendances.
└── zensical.toml : Configuration spécifique à Zensical (paramètres du SSG/site).
```

!!! tip "groupe de dépendance par uv"
    Dans le cas où votre doc cohabite avec votre code,
    il est judicieux de déclarer la librairie dans un
    groupe tiers exemple : docs. Cela optimisera le
    temps de build et simplifiera l'expérience de
    démarrage de vos tech writers si vous utilisez des lib
    exotiques dépendantes du hardware. `uv add --group=docs zensical`

On peut déjà tirer une première conclusion, que la mise
en place est d'une simplicité déconcertante. Je peux modifier l'index.md et avoir un premiere boucle feedback.

### Fonctionnalités disponibles

#### zensical.toml

Le coeur des modifications de votre site vont être
centraliser dans le fichier `zensical.toml`.
Certaines configurations vont suivre la pratique de
`convention over configuration`. Un ensemble de fonctionnalité
est ainsi par défaut activé. Cela permet d'avoir
certains effets magique comme la détection automatique
des nouvelles pages ajoutées appréciable pour démarrer.

!!! warning "navigation des pages"
    Cependant, tôt ou tard, il faudra déclarer l'option
    nav pour gérer finement l'ordre d'affichage des pages.
    L'ordre alphabétique a des limites si vous ne l'avez
    pas pris en compte par design.

Vous avez un certains nombre de feature à activer/désactiver pour
modifier l'expérience du panneau de navigation. Vous pouvez intégrer
le toc dans votre navigation, intégré du tracking, déplacer en header
une partie de la navigation. Tout dépendra du contenu que vous voulez
mettre en forme.

Parmis les premières personnalisations que vous allez pouvoir
mettre en place c'est de modifier le theme à travers les couleurs
ou les fonts utilisés.

```text
[project.theme]
palette.primary = "orange"
palette.accent = "orange"
font.text = "Inter"
font.code = "JetBrains Mono"
```

Vous pouvez ajouter le dark mode. Pour aller plus loin, il est
toujours possible de [déclarer votre propre css](https://zensical.org/docs/setup/colors/#custom-color-schemes).

A terme, vous pourrez regarder la partie surcharge des pages html.

#### Contenu d'une page

Jusqu'à présent, vous avez probablement modifié des
Readme avec un rendu par github ou gitlab.

!!! warning "l'interprétation markdown"
    a savoir que le format markdown possède plusieurs
    spécifications. Un Readme github n'a pas le même
    comportement que sur gitlab. Une spécification
    CommonMark[^7] a émergé pour stopper les divergences.
    Des dissonances sont donc à prévoir si vous cherchez
    allier les deux mondes, lecture sur SSG et github.

[^7]: [A strongly defined, highly compatible specification of Markdown](https://commonmark.org/)

Lorsque vous rédigez une page, vous devez ajouter
une première section metadata, `Front matter`, de votre page.
Elle vous sera très pratique à long terme. Elle
vous permet de personnaliser l'expérience en désactivant les
fonctionnalités précedemment activer avec le mot clef `hide`.
Il n'est pas nécessaire d'activer un TOC quand la page html tient
sur un écran où que vous réalisiez une page d'accueil.

```markdown {linenums="1 1 2"}
---
title: my first page
description: my description page
---

## my chapter
(...)
```

On ne va pas détailler comment rédiger un markdown mais à savoir que
vous avez une prise en charge des formules mathématiques, des schémas
mermaid, des admonitions, code blocks, tableau, tableau à onglet, footnotes,
grilles, émojis, de preview de snippet, de l'html dans le markdown,
de glossaire. Une grande partie sont visibles sur la page de démo générée.
Certaines fonctions de [python markdown extensions](https://facelessuser.github.io/pymdown-extensions/) ne sont pas officielement pris en charge mais peuvent etre
fonctionnelles.
Vous pouvez documenter l'api de votre librairie avec [mkdocstrings](https://zensical.org/docs/setup/extensions/mkdocstrings/).

Ce qui intéressant à savoir c'est que vous pouvez implémenter un template
jinja et surcharger la page html.


### Déployer sur pages

Pour déployer rapidement, vous pouvez utiliser les services
à disposition sur github du free plan.
Votre dépôt devra être public et vous serez limité à un site[^8].

[^8]: [Découvrez les limites et limitations de GitHub Pages](https://docs.github.com/fr/pages/getting-started-with-github-pages/github-pages-limits)

=== "Github Pages"
    ```yaml
    name: Documentation
    on:
      push:
        branches:
          - main
    permissions:
      contents: read
      pages: write
      id-token: write
    jobs:
      deploy:
        environment:
          name: github-pages
          url: ${{ steps.deployment.outputs.page_url }}
        runs-on: ubuntu-latest
        steps:
          - uses: actions/configure-pages@v6
          - uses: actions/checkout@v5
          - uses: actions/setup-python@v5
            with:
              python-version: 3.13.x
          - uses: astral-sh/setup-uv@08807647e7069bb48b6ef5acd8ec9567f424441b
          - run: uv sync --no-dev
          - run: uv run zensical build --clean
          - uses: actions/upload-pages-artifact@v5
            with:
              path: site
          - uses: actions/deploy-pages@v5
            id: deployment
    ```

=== "GitLab Pages"
    ```yaml
    stages:
      - deploy
    pages-deploy:
      stage: deploy
      image: dhi.io/uv:0-debian13-dev
      script:
        - uv sync --no-dev
        - uv run zensical build --clean
      pages: true
        publish: site
      rules:
        - if: '$CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH'
    ```

!!! tip hébergement
    Pour utiliser d'autres services d'hébergement en free plan, vous pouvez
    consulter [readthedocs](https://docs.readthedocs.com/platform/stable/index.html)
    qui est une référence dans la communauté OpenSource.

## Evaluations

!!! info "fonctionnalités testées"
    A date, je n'ai pas testé les options comme l'analytics, versioning et
    comment system. Cette revue pourra à terme évoluer.

### Points forts

Ce que l'on apprécie de Zensical :

- un démarrage très rapide
- de nombreuse fonctionnalité disponible par défaut
- un rendu moderne sans rien faire
- une courbe d'apprentissage progressive pour monter en puissance
dans la personnalisation
- une documentation très complète
- le projet intègre [pymdown-extensions](https://facelessuser.github.io/pymdown-extensions/extensions) qui contient des options et exemples avancés

### Limitations

5 mois après l'annonce, Zensical possède suffisamment de fonctionnalité pour
migrer d'un mkdocs v1 (vanilla) et de moderniser sa documentation.

En revanche, pour les personnes qui profitaient de l'écosystème de
plugins de mkdocs, il est encore tôt pour couvrir 100% lors de la migration.

Pour certains de mes usages en entreprise, il me manque :

- [ ] la partie révision git avec les dates de dernière modification et d'auteur
- [ ] l'option commandline `--site-dir` est manquante pour faire une preview en CI. Il faudra jongler dans l'étape script du job.
- [ ] les raccourcis en footer des social cards n'est pas totalement intégré dans le toml

Il faudrait également qu'à terme, les fonctionnalités suivantes se debloquent :

- [ ] le moteur d'extension par plugin (pour que la communauté s'adapte/innove)
- [ ] le mode blog, liste des derniers articles par date de publication
- [ ] l'affichage des pages par tags
- [ ] mkdocstrings est encore partiel
- [ ] prise en charge CommonMark

Globalement, le résultat est plutôt bon. Pour un projet si jeune, je ne
vais pas chercher à comparer fonction par fonction avec d'autres SSG.
Je suis assez satisfait pour continuer à rédiger sur Zensical et porter
les migrations (légère) de mkdocs.
