---
icon: lucide/file-text
title: Documentation As Code
tags:
    - docs
---

## :lucide-book-text: L'usage des Wikis au sein des projets

En travaillant sur un projet, vous aurez tôt ou tard à affronter le problème
de la documentation. Soit elle n'est pas à jour, soit manquante, souvent
perdue dans les limbes d'un répertoire partagé ou d'une GED/Sharepoint.

Avec l'essor de l'agilité, une dérive fréquente a été l'inconsistance de la
documentation lorsque celle-ci n'est pas maintenue au fil de l'eau. Si les
tests étaient le sacrifié sur le totem de la production alors la qualité de
la documentation n'imaginez même pas. Entre une spécification fournie par une
MOA et une implémentation qui colle avec la réalité du terrain, vous venez
de dériver la connaissance de vos business owners.

Pendant des années, j'étais grand fan de la solution Confluence pour son
approche Wiki. Diffuser la connaissance est une nécessité pour tendre vers
la standardisation et augmenter le niveau de connaissance des collaborateurs.

!!! quote "Accelerate State of DevOps 2023[^1]"
    In addition to improving technical capabilities, we
    found that quality documentation has a positive impact
    on an individual’s well-being: it reduces burnout,
    increases job satisfaction, and increases productivity.
    We found that some of this impact is because quality
    documentation increases knowledge sharing.

[^1]: [DORA accelerate state of devops 2023 Report](https://services.google.com/fh/files/misc/2023_final_report_sodr.pdf)

Mais à grande échelle, les Wikis montrent des limites :

- la recherche devient complexe,
- la responsabilité est diluée,
- les informations se dupliquent,
- les workflows diffèrent selon les personas (développeurs, métiers,
  fonctionnels),
- la publication en version reste souvent manuelle ou sous-optimale.

Par expérience, je considère qu'une page non mise à jour depuis plusieurs mois
est souvent déjà obsolète.
Une documentation sur un produit rédigée par une équipe distante des
responsables du produit est souvent moins pertinente.

En développant des utilitaires, pour gagner en engagement, la documentation
est indispensable. Rapidement, l'évidence est de basculer sur la
**Documentation as Code** pour simplifier son propre flux de travail.
Il est plus simple de consulter la documentation dans le dépôt git pour
retrouver la bonne info pour mes developpeurs que de parcourir du code.
Il arrive même qu'un développeur vous réalise une pull request pour corriger
une info.

## :lucide-telescope: Pourquoi Docs As Code ?

!!! quote "@Eric Holscher & the Write the Docs community [^2]"
    Documentation as Code (Docs as Code) refers to a philosophy that you
    should be writing documentation with the same tools as code:

    * Issue Trackers
    * Version Control (Git)
    * Plain Text Markup (Markdown, reStructuredText, Asciidoc)
    * Code Reviews
    * Automated Tests

[^2]: une communauté internationale intéressée par l'art de la documentation
    [WriteTheDocs.org](https://www.writethedocs.org/guide/docs-as-code)

Docs As Code est une approche qui traite la documentation comme du code :

- contenu stocké en texte brut dans un dépôt Git,
- versionné avec les mêmes règles que le code source,
- soumis à revue via merge requests / pull requests,
- publié automatiquement par des pipelines CI/CD.

!!! success  "Regard vers l'Open Source"
    Le monde de l'Open Source nage avec aisance sur ce paradigme.
    La présence d'une documentation en ligne
    est souvent un facteur de maturité du produit. Il est plus agréable
    de parcourir un site web, que des fichiers markdown sur Github.
    Si on regarde par exemple le projet Gitlabform[^3], malgré sa faible
    notoriété avec <500 stars, possède une documentation très complète
    qui démontre sa pertinence.

[^3]: le [projet Gitlabform](https://gitlabform.github.io/gitlabform/) est
    un utilitaire réalisant de la configuration as code des projets
    GitLab.

### :lucide-brain-circuit: L'essor de l'IA générative

!!! quote "L'IA un levier pour produire la documentation[^4]"
    Dans l'ensemble, les tendances observées ici sont très favorables à l'IA.
    Voici les principaux enseignements (...):
    une augmentation de 7,5 % de la qualité de la documentation

[^4]: [DORA accelerate state of devops 2024 Report](https://dora.dev/research/2024/dora-report/2024-dora-accelerate-state-of-devops-report-fr.pdf)

La documentation a porté d'ia agentique devrait plier l'argumentation.
En rapprochant la doc du code vous allez vous mettre en capacité de
donner du contexte pertinent à votre agent. Définissez des conventions
d'arborescence et avec les bonnes règles d'instruction, vos requêtes
devraient être optimisés en nombre de token.

Si vous n'êtes pas du genre locace ou un peu trop enjouée, l'IA restructura
votre propos avec une touche professionnelle.

Vous pourriez faire la même chose avec un mcp dédié a votre wiki
(s'il en existe un). L'avantage c'est que vous n'aurez pas a le configurer
et vous limiterez le nombre de requêtes nécessaire à son fonctionnement.

!!! warning "Tiered documentation[^5]"
    Il existe un courant de pensée qu'il faut distinguer la documentation
    dédiée aux humains et celles aux agents pour optimiser l'efficience
    du contexte. C'est à mettre en oeuvre avec parcimonie au risque de
    doubler le contexte et de créer de l'obsolescence documentaire. Vous
    ne devriez vous y préoccuper qu'en ayant atteint les limites de la
    modélisation en place.

[^5]: [Documentation Hierarchisée, un article cio-online](https://www.cio-online.com/actualites/lire-developpement-l-ia-generative-aussi-a-besoin-d-une-bonne-documentation-du-code-15427.html)

### :lucide-split: Ce que cela change

On le devinait par l'approche *As Code* mais concrètement on se donne
les moyens d'améliorer son produit par design.

<div class="grid cards" markdown>

- :lucide-blocks: __Cohérence__

    ---
    la documentation évolue avec le code et les sources

- :lucide-videotape: __Tracabilité__

    ---
    on retrouve l'historique exact des modifications

- :lucide-trending-up: __Qualité__

    ---
    on peut appliquer des validations automatiques

- :material-account-group-outline: __Collaboration__

    ---
    la documentation entre dans le même flux de travail que
    le développement
</div>

Des frameworks, utilitaires existent dans la mise en oeuvre.
Parfois, vous n'êtes qu'à 3 commits de passer du brouillon à l'état de l'art.

## :lucide-library: Quelle solution adaptée ?

Le choix d'une solution est multi-factorielle.

* Quels sont mes compétences à disposition dans l'équipe ?
* quels sont les fonctionnalités obligatoires (recherche, navigation, docstring, ...) ?
* Est-ce que je pars déjà d'un existant ?
* Quels types d'informations ai-je besoin de mettre en forme (swagger, notes d'architecture, guides) ?


### Solutions courantes *Static Site Generator* (SSG)

Lorsque vous démarrez de nul part, il faut rester modeste pour obtenir des gains immédiats.
Une solution qui met en forme des fichiers Markdown peut largement suffire
avant d'explorer des alternatives. Votre `README.md` aura des compagnons de
route et il sera plus facile d'accompagner un collectif.

Personnellement, j'ai commencé par [MkDocs (vanilla)](https://www.mkdocs.org/)
puis [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
et [Zensical](https://zensical.org/) : en quelques heures vous avez un site statique opérationnel
avec des fonctionnalités avancées. Le faible investissement permet d'itérer
rapidement.

!!! danger "Avenir de l'éco-système mkdocs[^6]"
    *Mkdocs v2* est en cours de developpement et est break-change.
    *Material for Mkdocs* a décidé de tourner la page et de créer *Zensical[^7]*.
    Cependant *Zensical* n'a pas encore porté toutes les fonctionnalités
    au 04/2026. Si vous êtes dans ce cas de figure, vous n'êtes pas seul
    à évaluer des migrations[^8].

[^6]: [La chute de mkdocs](https://fpgmaas.com/blog/collapse-of-mkdocs/)
[^7]: [Annonce de la création Zensical](https://squidfunk.github.io/mkdocs-material/blog/archive/2025/)
[^8]: [Etude migration depuis mkdocs du projet ddev](https://github.com/ddev/ddev/issues/8216)

Voici un tableau comparatif des solutions de *Static Site Generator* les plus courantes :

=== "[MkDocs](https://www.mkdocs.org/) / [Zensical](https://zensical.org)"
    | Format | Avantages | Limites | Cas d'usage |
    |:---|:---|:---|:---|
    | Markdown | Simple à configurer; rapide; bonne navigation et recherche; faible coût d'entrée | Moins adapté aux documentations très spécialisées; fonctionnalités avancées via plugins | Guides techniques, petites et moyennes documentations, sites de projet |

=== "[Sphinx](https://www.sphinx-doc.org/en/master/)"
    | Format | Avantages | Limites | Cas d'usage |
    |:---|:---|:---|:---|
    | reStructuredText & Markdown par MyST | Puissant pour API Python; `autodoc`, cross-references, gestion de versions | Courbe d'apprentissage; configuration plus lourde; reST par défaut | Bibliothèques Python, documentation technique approfondie |

=== "[Docusaurus](https://docusaurus.io/fr/)"
    | Format | Avantages | Limites | Cas d'usage |
    |:---|:---|:---|:---|
    | Markdown & MDX | UI moderne; support MDX; versioning intégré; écosystème de plugins | Plus lourd; nécessite JS/React; bundling et maintenance frontend | Documentation produit, sites avec blog et marketing |

=== "[Hugo](https://gohugo.io/)"
    | Format | Avantages | Limites | Cas d'usage |
    |:---|:---|:---|:---|
    | markdown | Très flexibles; rapides; templating performant | Templating et configuration peuvent devenir complexes; écosystème dépendant du langage | Sites personnalisés, blogs, documentations sur mesure |


### :lucide-wrench: Outils complémentaires

Pour parfaire votre pipeline, vous pouvez considérer les utilitaires :

=== "Linting"
    | Nom | Type | A savoir |
    |:---|:---|:---|
    | [`Vale`](https://vale.sh/docs) | client | couvre tout format, forte capacité de configuration mais complexe |
    | [`markdownlintcli2`](https://github.com/DavidAnson/markdownlint-cli2) | npm lib | linting Markdown/CommonMark |
    | [`remark`](https://github.com/remarkjs/remark) | npm lib | complexe outils à base de plugins |
    | [`mdformat`](https://mdformat.readthedocs.io/en/stable/index.html) | python lib | CommonMark compliant Markdown formatter |

=== "Validation liens"
    | Nom | Type | A savoir |
    |:---|:---|:---|
    | [`htmlproofer`](https://github.com/gjtorikian/html-proofer) | ruby | support HTML |
    | [`linkcheck`](https://github.com/filiph/linkcheck) | client | support HTML mais aucune release depuis 2023|
    | [`lychee`](https://github.com/lycheeverse/lychee) | client | support HMTL et markdown |

=== "Conversion format"
    | Nom | Type | A savoir |
    |:---|:---|:---|
    | [`pandoc`](https://pandoc.org/) | client | convertisseur dans tout format |
    | [`mark`](https://github.com/kovetskiy/mark) | client | synchro markdown vers confluence |

!!! warning "Utilisation des linters/formatters"
    Les linters peuvent être incompatible avec le format demandé
    par le SSG. Pour chaque linter, il faut identifier soit un plugin
    dédié soit les règles personnalisées à appliquer.

!!! tip "Combiner docs as code + Wiki"
    Au niveau de l'organisation, vous désirez combiner l'usage d'un
    wiki d'entreprise et les capacités de l'approche docs-as-code.
    Vous pouvez utiliser des utilitaires pour synchroniser vos pages
    markdown ou page html vers les pages associées du Confluence.

## :lucide-biceps-flexed: Fonctionnement

### Structure

Dans votre dépôt git, la convention est de créer un répertoire
`docs` à la racine.

### Processus

Le nouveau processus de rédaction de la documentation est le
suivant :

``` mermaid
graph
  A[Créer/Modifier un Markdown docs dans le dépôt] --> B;
  B[Ouvrir une pull request] --> C;
  C[Execution auto pipeline CI] --> D1;
  C --> D2;
  D1[Job: Validation synthaxe markdown] --> E;
  D2[Job: Verification des liens] --> E;
  E[Job: déploie une preview de la docs] --> F;
  F[Revue/validation par un collaborateur de la pull request] --> G;
  G[Merge et publication automatique sur Github/Gitlab Pages]
```

!!! tip "bonnes pratiques du quotidien"
    - versionner la documentation avec la même branche que le code lorsque
    le contenu dépend directement d'une fonctionnalité,
    - utiliser des templates et des modèles pour homogénéiser les pages,
    - automatiser la publication par défaut,
    - garder la documentation près du code pour limiter les décalages.
    - si vous ne modifiez que la doc utiliser la convention nommage de branche
    en **"docs/"** et vos titres de pull requests et commits par **"docs:"**

## Resources

Pour explorer davantage l'art de la documentation,
je vous recommande de parcourir [writethedocs.](https://www.writethedocs.org/guide)
writethedocs aggrége de nombreux guides, il y aura surement des réponses à
vos interrogations.
