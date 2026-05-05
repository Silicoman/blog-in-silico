---
icon: lucide/bolt
title: La convention plutôt que la configuration
description: Favoriser des conventions pour réduire la configuration manuelle et améliorer l'expérience des utilisateurs.

---

## :lucide-earth: Une pratique omniprésente

 Si *Convention Over Configuration* (CoC) vous est étranger,
 vous l'utilisez probablement au quotidien.
 Les conventions sont des règles ou des dispositions par défaut qui évitent
 des configurations répétitives. Lorsqu'une convention est respectée, il
 n'est souvent pas nécessaire de personnaliser la configuration : les
 outils savent agir automatiquement.

 Par exemple, sur les systèmes Debian/Ubuntu, les sources APT[^1] se placent
 dans le répertoire `/etc/apt/sources.list.d/`. Ce répertoire est prévisible : le répertoire `/etc` contient toutes les configurations de votre système. Aucune configuration APT n'a besoin d'être modifiée en cas d'ajout ou de suppression de sources. La convention permet d'éviter la configuration.

[^1]: [APT sources — page Debian (sources.list)](https://wiki.debian.org/SourcesList)

Lorsque vous ouvrez un projet dans votre IDE, l'IDE active des extensions, et vous
soumet des actions. Ces actions correspondent à votre code source grâce aux
conventions de configuration.
Un plugin de tests peut intégrer tout le cycle de tests si, par exemple, dans un projet utilisant *pytest*, vous respectez la convention de nommage des fichiers `test_*.py`.

Choisir de ne pas suivre ces conventions se fait à vos risques : vous augmentez la
charge cognitive des utilisateurs qui devront chercher où se trouve le fichier
de configuration attendu, de configurer leurs outils.

## :lucide-git-merge: Aux origines

Il s'agit d'une pratique dérivée du principe du moindre étonnement (principle of least astonishment), formulé en 1972. Un système doit se comporter comme la majorité des utilisateurs l'attend et ne doit pas provoquer d'étonnement ni de surcharge cognitive.

!!! quote "Bergeron, R.D.; Gannon, J.D.; Shecter, D.P.; Tompa, F.W.; Dam, A. Van (1972). "Systems Programming Languages". Advances in Computers."
    For those parts of the system which cannot be adjusted to the peculiarities of the user, the designers of a systems programming language should obey the "Law of Least Astonishment." In short, this law states that every construct in the system should behave exactly as its syntax suggests. Widely accepted conventions should be followed whenever possible, and exceptions to previously established rules of the language should be minimal.

En 2004, David Heinemeier Hansson popularise le concept de CoC avec *Ruby on Rails*.

!!! quote "The Rails Doctrine @David Heinemeier Hansson[^11]"
    Le transfert de la configuration à la convention nous libère non seulement de la discussion, mais offre également un champ luxuriant permettant de développer des abstractions plus profondes. Si nous pouvons compter sur un mapping de la classe Person à la table des personnes, nous pouvons utiliser cette même inflexion pour mapper une association déclarée comme has_many :people à la recherche d’une classe Person. Le pouvoir des bonnes conventions est qu’elles rapportent des dividendes dans un large éventail d’utilisations.

    Mais en plus des gains de productivité pour les experts, les conventions réduisent également les obstacles à l’entrée pour les débutants. Dans Rails, il existe de nombreuses conventions qu’un débutant n’a même pas besoin de connaître, mais dont ils peuvent bénéficier simplement en les ignorant. Il est possible de créer de bonnes applications sans savoir pourquoi tout est comme cela et fonctionne.

[^11]: [Doctrine de Ruby On rails par David Heinemeier Hansson](https://rubyonrails.org/doctrine/fr)

Les objets sont automatiquement conçus avec des relations implicites dans le nommage,
par exemple entre une classe et sa table en base de données. L'idée à retenir
est que vous n'avez pas à configurer ou à créer des éléments, ni à vous poser
des questions si vous respectez les conventions du framework.

### :lucide-toolbox: Un paradigme bien illustré par Maven

!!! quote "Maven Documentation[^10]"
    While Maven takes an opinionated approach to project layout, some projects may not fit with this structure for historical reasons. While Maven is designed to be flexible to the needs of different projects, it cannot cater to every situation without compromising its objectives.

[^10]: [Qu'est-ce que Maven, un outil implémentant les meilleurs pratiques](https://maven.apache.org/what-is-maven.html#providing-guidelines-for-best-practices-development)

Maven, en tant qu'outil de gestion de projet et de construction, illustre bien ce concept : la configuration d'un projet s'appuie sur des conventions de structure.[^5] Par exemple, un projet Maven typique attend :

 - un fichier `pom.xml` à la racine décrivant les configurations du projet ;
 - le code source Java dans `src/main/java` ;
 - les tests dans `src/test/java` ;
 - les résultats de compilation et des tests dans le répertoire `target` ;
 - les fichiers de ressources dans `src/main/resources` ou `src/test/resources`.

[^5]: [Maven — Standard Directory Layout](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html)

 Respecter ces conventions simplifie l'expérience des développeurs et facilite
 l'intégration continue ainsi que le travail sur les postes de développement.

## Du concept à la mise en œuvre

 Si vous développez des utilitaires, réfléchissez à la configuration minimale
 d'installation nécessaire pour une prise en main rapide : existe-t-il déjà
 une convention que vous pouvez réutiliser ?

 Cela peut sembler anodin, mais le temps nécessaire pour rendre un outil
 opérationnel dans un environnement d'entreprise peut varier de quelques
 minutes à plusieurs jours, surtout pour des profils débutants.

### Les variables d'environnement

 Les variables d'environnement sont omniprésentes : elles peuvent sembler
 magiques mais simplifient souvent l'usage des outils. Il existe déjà des
 conventions bien établies qu'il vaut mieux réutiliser.

 Par exemple, si votre utilitaire `mymagictool` gère la coloration des logs,
 il est préférable d'honorer la convention `NO_COLOR` [^20] plutôt que
 d'inventer `MYMAGICTOOL_COLOR` ou `IS_COLOR`. Pour l'authentification GitLab,
 la variable `GITLAB_TOKEN` est déjà utilisée par de nombreux outils
 (gitlabform[^21], etc.).

!!! quote "NO_COLOR"
    Command-line software which adds ANSI color to its output by default should check for a NO_COLOR environment variable that, when present and not an empty string (regardless of its value), prevents the addition of ANSI color.

[^21]: [Gitlabform configuration à l'aide de GITLAB_URL et GITLAB_TOKEN](https://gitlabform.github.io/gitlabform/reference/#minimal-working-config)
[^20]: [NO_COLOR](https://no-color.org/)

### :lucide-folder-tree: Les chemins et noms

La normalisation `etc`, `bin`, `lib`, `var` existe pour une bonne raison. Elle est prévisible et autoporteuse d'information. Si vous distribuez votre solution, structurez votre archive avec des répertoires typés.

### :lucide-file-code: Fichier de configuration

Si vous développez des utilitaires, vous aurez probablement des mécanismes de
chargement de configuration. Les propriétés doivent également être normalisées.
Cela peut sembler trivial. Si vous utilisez (et je vous le recommande) le
versionnage sémantique (Semantic Versioning) dans le contrat de configuration,
utilisez la clé `version` et non une clé exotique. Il est plus facile pour un
utilisateur de se remémorer des mots qu'il voit fréquemment dans votre
documentation ou dans un exemple, au moment de l'implémentation.

## :lucide-scissors: Le courage de pivoter

 Lorsque les contraintes changent, il faut savoir pivoter et revoir les
 conventions adoptées — retirer une mauvaise convention tôt est préférable
 à la maintenir et complexifier le système.

Pour conduire le changement, il faudra parfois gérer l'ancienne et la
nouvelle méthode en ajoutant des mécanismes d'avertissement pour l'utilisateur
(logs, messages d'erreur, commentaires, CHANGELOG, etc.). `NO_COLOR` et
`MYMAGICTOOL_COLOR` devront coexister pendant un certain temps. C'est le
rythme de vos releases qui définira le bon moment pour nettoyer le code ou
vos serveurs.

 Lorsque les contraintes changent, il faut savoir pivoter et revoir les conventions adoptées — retirer une mauvaise convention tôt est préférable à la maintenir et complexifier le système.

 Appliquer ce paradigme demande de la vigilance. Il est facile d'adopter la
 première solution trouvée sur Stack Overflow ou proposée par une IA,
 mais il est plus difficile de mettre en place une vraie bonne pratique.
 Prendre le temps de consulter la documentation officielle et de formuler
 des conventions claires finit par payer.

 Le respect des conventions favorise la cohérence et la maintenabilité du code.
 Toutefois, gardez à l'esprit les risques : les conventions peuvent masquer du
 comportement "magique" et rendre le débogage plus difficile si elles ne sont pas
 documentées. Pour tirer parti des conventions tout en restant pragmatique :

 - documentez clairement vos conventions ;
 - fournissez des valeurs par défaut raisonnables ;
 - prévoyez des mécanismes simples pour outrepasser la convention lorsque nécessaire.

 Si vous êtes product manager, ne vous contentez pas d'affirmer que "ça marche" :
 vérifiez que l'expérience utilisateur est réellement optimisée. La configuration
 augmente la barrière à l'entrée et peut affecter la rétention des utilisateurs.
 Les impatients seront vos meilleurs porte-paroles.
