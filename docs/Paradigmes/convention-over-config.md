---
title: conventions plutôt que des configurations
---

## une pratique omnisciente

Si vous ignorez le terme, vous l'utilisez probablement
au quotidien. Les conventions sont des configurations
qui fonctionnent comme par magie. Si on applique tel
design, alors on a rien a personnaliser, nos outils 
sauront quoi faire.
Typiquement, lorsque vous travaillez sur des configurations 
d'un système d'exploitation, vous pouvez avoir des chargements dynamique de fichier parce que vous avez créer un fichier dans tel répertoire qui est chargé automatiquement. La convention pour ajouter un nouveau registre apt dans Ubuntu est /etc/apt.sources.list.d/ .

### un paradigme imprégné dans maven

Maven m'a ouvert les yeux sur ce concept. Toute la 
philosophie de configuration d'un projet porté par le pom est conceptualiser autour des conventions d'arborescence. 

- j'attends un pom.xml a la racine
- j'attends un répertoire nommé `src` à compiler par java
- j'attends un répertoire nommé `tests` pour lancer des tests par java
- j'attends un répertoire `resources` pour stocker mes fichiers non compilable

Et je pourrais continuer longtemps à énumérer. Ce qu'il faut retenir, c'est que pour simplifier l'expérience des développeurs, il faut normer. Si on respecte une norme logique, alors on ne devrait pas à s'inquiéter de la configuration à mettre en oeuvre que ça soit en intégration continue ou sur votre poste de travail.

## du concept à la mise en oeuvre

Il est possible que vous soyez amenée à développer des petits utilitaires. Pour simplifier la prise en main,
vous devez réfléchir à quel configuration d'installation,
de démarrage j'ai besoin. Existe-il une convention ?

### les variables d'environnement

Les variables d'environnement sont devenus omniscientes.
Elle peuvent être source de frustration pour son côté
magique et en même temps peuvent vous simplifier la vie.

Il existe probablement des noms de variables qui existent déjà qui reflète le comportement attendu.

Prenons un exemple.
Si votre utilitaire mymagictool gère la coloration des logs, ça ne sert a rien d'inventer la variable d'environnement MYMAGICTOOL_COLOR. Il existe par convention la variable NO_COLOR.

Votre utilitaire gère l'authentification gitlab. Vous pouvez utiliser la convention GITLAB_TOKEN. Cette norme est utilisé par plein d'outils manipulant Gitlab (renovate, gitlabform, ...)