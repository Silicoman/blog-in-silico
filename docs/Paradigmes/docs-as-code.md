---
title: Documentation As Code
---

## L'usage des Wikis au sein des projets

En travaillant sur un projet, vous aurez tôt ou tard à affronter le problème
de la documentation pas à jour ou de la documentation manquante, souvent
perdue dans les limbes d'un répertoire partagé ou d'une GED.

Avec l'essor de l'agilité, une dérive fréquente a été l'inconsistance de la
documentation lorsque celle-ci n'est pas maintenue au fil de l'eau. Si les tests étaient le sacrifié sur le totem de la production alors la qualité de la documentation n'imaginez même pas. Entre une spécification fournie par une MOA et une implémentation qui colle avec la réalité du terrain, vous venez de dériver la connaissance de vos business owners.

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
**Documentation as Code** pour simplifier son propre flux de travail.
Il est plus simple de consulter la documentation dans le dépôt git pour retrouver la bonne info pour mes developpeurs que de parcourir du code. Il arrive même qu'un développeur vous réalise une pull request pour corriger une info.

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

Le monde de l'Open Source nage avec aisance sur ce paradigme. La présence d'une documentation en ligne
est souvent un facteur de maturité du produit. Il est plus agréable de parcourir un site web, que des fichiers markdown sur github.

### IA générative

La documentation a porté d'ia agentique devrait plier l'argumentation. En rapprochant la doc du code vous allez vous mettre en capacité de donner du contexte pertinent à votre agent. Définissez des conventions d'arborescence et avec les bonnes règles d'instruction, vos requêtes devraient être optimisés en nombre de token.

Si vous n'êtes pas du genre locace ou un peu trop enjouée, l'IA restructura votre propos avec une touche professionnel.

Vous pourriez faire la même chose avec un mcp dédié a votre wiki (s'il en existe un). L'avantage c'est que vous n'aurez pas a le configurer et vous limiterez le nombre de requêtes nécessaire à son fonctionnement.

### Ce que cela change

On le devinait par l'approche as code mais concrètement on se donne les moyens d'améliorer son produit par design.

- cohérence : la documentation évolue avec le code et les sources,
- traçabilité : on retrouve l'historique exact des modifications,
- qualité : on peut appliquer des validations automatiques,
- collaboration : la documentation entre dans le même flux de travail que
  le développement.

Des frameworks, utilitaires existent dans la mise en oeuvre. Parfois, vous n'êtes qu'à 3 commits de passer du brouillon à l'état de l'art.


## Quelle solution adaptée ?

Le choix d'une solution Docs As Code est multi-factorielle.

* Quels sont mes compétences à disposition dans l'équipe?
* quels sont les fonctionnalités obligatoires (recherche, navigation, docstring, ...) ?
* Est-ce que je pars déjà d'un existant ?
* Quels types d'informations ai-je besoin de mettre en forme (blog, swagger, notes d'architecture, guides)?

### Solutions courantes
 
Lorsque vous démarrez de nul part. Il faut rester modeste pour avoir des gains immédiat. Une solution mettant en forme des fichiers markdown peut largement suffire avant d'explorer des alternatives. Votre README.md aura des compagnons de route et il sera plus facile d'accompagner un collectif.

Personnellement, j'ai jeté mon dévolu pour mkdcos v1 vanilla, material4mkdocs puis zensical. En quelques heures,vous avez un site statique opérationnel avec des fonctionnalités avancées. Le faible investissement m'a donné un droit à l'erreur que je n'ai pas consommé.

Cependant de puissantes solutions existent : 

- `MkDocs` / `Zensical` : simple à configurer, bon pour les docs techniques et les guides
- `Sphinx` : puissant pour la documentation Python et les docs complexes.
- `Docusaurus` : bien adapté aux sites modernes React et à la documentation
  produit.
- `Hugo` / `Jekyll` : générateurs statiques flexibles pour des sites plus
  personnalisés.


### Outils complémentaires 

Pour parfaire votre pipeline, vous pouvez considérer les utilitaires :
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


### Bonnes pratiques

- versionner la documentation avec la même branche que le code lorsque
  le contenu dépend directement d'une fonctionnalité,
- utiliser des templates et des modèles pour homogénéiser les pages,
- appliquer des règles de style et des validations automatiques,
- automatiser la publication par défaut,
- garder la documentation près du code pour limiter les décalages.

## Références

- https://www.writethedocs.org/guide/docs-as-code/
- https://services.google.com/fh/files/misc/2023_final_report_sodr.pdf
