---
title: Scaffolding avec Copier
description: "Générer de nouveaux projets à partir d'un archétype avec Copier — accélère l'initialisation et facilite les mises à jour."
tags:
    - scaffolding
---

J'ai l'habitude de générer plein de projet applicatif. L'incovénient est que
je me retrouve à toujours faire les mêmes manipulations pour générer un
squelette fonctionnel. C'est encore plus vrai dans un éco-système air-gap
d'entreprise. Cet article s'intéresse à un outil permettant
de construire un archetype (*template*) à l'aide de `copier` pour simplifier
cette étape.

## Qu'est-ce que copier ?

<img src="https://raw.githubusercontent.com/copier-org/copier/refs/heads/master/img/logo.svg" alt="Copier Logo" style="float:left; margin:0 1rem 1rem 0; object-fit:cover;" />

[`copier` est un outil de scaffolding](https://copier.readthedocs.io/en/stable/) (génération de projet) léger et moderne basé sur des templates Jinja2. Il permet de générer rapidement un projet à partir d'un modèle (*template*) et d'appliquer ensuite les évolutions du template sur les projets existants. de première abord, l'outil
présente quelques caractéristiques notables:

- **Simplicité** : les templates sont des fichiers texte utilisant Jinja2, faciles à lire et à éditer.
- **Mise à jour** : workflow d'`update` intégré pour propager les changements du template.
- **Flexible** : supporte les templates locaux, les dépôts Git et les archives distantes.

### Comparaison de l'écosystème des scaffolders

Pour comprendre l'intérêt de `copier`, on résume ci-dessous les outils de scaffolding
les plus courants, pour vous aider à choisir selon vos besoins.

| Outil | Moteur / format | Mises à jour | Écosystème | Idéal pour |
|---|---|---:|---|---|
| **Copier** | Jinja2, fichiers texte | Oui — workflow d'update intégré | Croissant (templates Python & génériques) | Projets maintenables où appliquer des évolutions de template |
| **Cookiecutter** | Jinja2 | Non (génération one‑shot) | Très large (templates Python) | Génération rapide de projets et nombreux templates existants |
| **Yeoman** | EJS / générateurs Node.js | Variable, selon le générateur | Large (JS/Frontend) | Scaffolding d'apps web / écosystème JavaScript |
| **degit / git clone** | — (copie brute) | Non | N/A | Copier un starter sans templating ni questions |


- __Historique des Stars de `Cookiecutter`, `Copier`, `Yeoman`__

    ---
	[![Star History Chart](https://api.star-history.com/chart?repos=copier-org/copier%2Ccookiecutter/cookiecutter%2Cyeoman/yeoman&type=date&legend=top-left)](https://www.star-history.com/?repos=copier-org%2Fcopier%2Ccookiecutter%2Fcookiecutter%2Cyeoman%2Fyeoman&type=date&legend=top-left)

On peut résumé le choix du bon outil pour votre quotidien avec les arguments suivants :

- Choisissez **Copier** si vous souhaitez pouvoir propager des corrections et améliorations du template vers les projets déjà créés.
- Choisissez **Cookiecutter** pour une simplicité maximale et une vaste bibliothèque de templates prêts à l'emploi.
- Choisissez **Yeoman** pour des generators complexes dans l'écosystème JavaScript.

Étant donné l'intérêt pour la maintenance des projets et la capacité à appliquer des mises à jour, l'attention se porte naturellement sur `copier`.

## Utiliser Copier

### Prérequis

Installez `copier` dans votre environnement Python :

```bash
pip install copier
```
### Génération d'un exemple rapide

Générer un projet à partir d'un template local :

```bash
copier copy /chemin/vers/mon-template ./nouveau-projet
```

Générer un projet à partir d'un template distant (ex. dépôt Git) :

```bash
copier copy https://github.com/Silicoman/copier-uv-python-project.git ./nouveau-projet
```

Lancez `copier --help` ou `copier copy --help` pour voir les options disponibles.

Le fichier [copier-uv-python-project/copier.yml](copier-uv-python-project/copier.yml)
dans ce dépôt contient des variables et le `_subdirectory` utilisé par le template.

## Créer son template de scaffolding

Maintenant on va essayer de créer son propre template pour personnaliser notre
expérience et identifier les capacités de `copier`.

### Structure typique d'un template

!!! Remarque sur les extensions
	Les fichiers de template sont interprété par l'extension `.jinja`. Cependant,
	vous pouvez utiliser n'importe quelle extension comme `.j2` en surchargeant
	le suffixe des templates Jinja2 utilisé. Il faudra définir :
	`_templates_suffix: .j2`. Copier applique le rendu Jinja2
	au contenu des fichiers et peut aussi utiliser des expressions Jinja dans
	les noms de fichiers.

Un template `copier` est simplement un dossier contenant des fichiers et,
optionnellement, un fichier de configuration (`copier.yml`) qui décrit les
variables demandées par des questions  à l'utilisateur et d'autres métadonnées.

Exemple d'une structure :

```
my-template/
├── copier.yml         # (optionnel) description des variables et valeurs par défaut
├── README.md
└── {{ project_slug }}/
	├── src/
	│   └── main.py
	├── tests/
	├── .gitignore
	├── README.md.jinja # peut contenir {{ project_name }} (Jinja2)
	└── {% if use_precommit %}.pre-commit-config.yaml{% endif %}
```

Les fichiers du template peuvent contenir des expressions Jinja2
(ex. `{{ project_name }}`). Il est aussi possible de rendre conditionnel
certains fichiers[^8] ou de nommer des fichiers avec des expressions Jinja.
Dans cette exemple, l'existence du fichier `.pre-commit-config.yaml` est
conditionné par le booleen `use_precommit` que l'on déclarera dans `copier.yml`.

[^8]: [Conditionner les fichiers](https://copier.readthedocs.io/en/stable/configuring/#conditional-files-and-directories)

### Définir des variables simples

Le template peut déclarer un petit fichier YAML (par exemple `copier.yml`) pour fournir des valeurs par défaut et des aides pour les questions posées pendant la génération. Exemple simple :

```yaml
# copier.yml (exemple)
project_name:
  default: "Mon Projet"
  type: str
  help: "Nom affiché du projet"
project_slug:
  default: mon-projet
  type: str
  help: "Nom du dossier / slug utilisé pour le package"
use_precommit:
	default: true
	type: bool
	help: "Voulez-vous pre-commit?"
```

Pendant la génération, `copier` vous posera ces questions et utilisera les réponses pour rendre les fichiers du template.

### Variables avancées

Explorons les options avancées des variables pour simplifier et durcir notre
template copier.
Dans l'exemple ci-dessous, on rajoute des variables calculées, une question
à choix et des validations.

```yaml
# copier.yml (exemple)
project_name:
  default: MonProjet
  type: str
  help: "Nom affiché du projet"

project_slug:
  default: mon-projet
  type: str
  default: "{% project_name | lower | replace(' ', '-') %}"
  validator: >-
    {% if not project_slug %}
    project_slug est requis.
    {% elif not (project_slug | regex_search('^[A-Za-z1-9-]+$')) %}
    project_slug doit contenir uniquement des lettres (A-Z, a-z), des chiffres 1-9 et des tirets.
    {% endif %}
  when: false # Pas de question, valeur calculée

python_version:
  type: str
  help: What Python version do you want to use?
  default:
  choices:
    - "3.11"
	- "3.12"
	- "3.13"
	- "3.14"

copyright_year:
  type: str
  default: "{{ copyright_year | default('%Y' | strftime) }}"
  when: false
```

!!! tips "Aller plus loin"
	il est possible de faire du multi-choix, des choix dynamiques[^9],
	d'inclure des questions depuis un autre fichier[^10]. Certaines
	expressions jinja typiquement dans des noms de fichier peuvent
	devenir imbuvables ou répétés à plusieurs endroits. Pour simplifier,
	vous pouvez inclure des expressions.[^11]

[^9]: [questions avec des choix avancées](https://copier.readthedocs.io/en/stable/configuring/#advanced-prompt-formatting)
[^10]: [inclusion de questions depuis d'autres yaml](https://copier.readthedocs.io/en/stable/configuring/#include-other-yaml-files)
[^11]: [importer des expressions jinjas](https://copier.readthedocs.io/en/stable/configuring/#importing-jinja-templates-and-macros)

## Mise à jour d'un projet généré


Une des forces de `copier` est la capacité à appliquer des changements faits au template sur un projet déjà généré (workflow d'`update`).

Commandes de base :

```bash
# Générer (une première fois)
copier copy /chemin/vers/template ./mon-projet

# Appliquer les mises à jour du template sur le projet existant
copier update ./mon-projet
```

Flux d'exemple pour une mise à jour :

1. Modifier et committer des changements dans le template (par ex. corriger un fichier de configuration, ajouter une nouvelle option).
2. Dans le projet généré, exécuter `copier update ./mon-projet`.
3. Copier affichera un aperçu des changements et proposera d'accepter/ignorer/éditer les modifications. Résolvez les conflits si nécessaire.

Pour l'automatisation, il est possible de réutiliser un fichier d'answers ou d'alimenter les variables en JSON/YAML depuis la ligne de commande (voir `copier --help`).

## Conclusion

`copier` est relativement récent par rapport à ces alternatives. Il existe [quelques bugs
principalement sur la partie `update`](https://github.com/copier-org/copier/issues). C'est la fonctionnalité phare de `copier` mais qui est la
plus difficile à implémenter. Cela nécessite un contrat de qui fait quoi entre le projet généré
et le template pour éviter au maximum des conflits. Si vous automatisez les `update`, il faudra
bien réfléchir comment résoudre les conflits. Une solution simple serait d'ouvrir une pull request
avec les fichiers en conflit pour


Quelques remarques pour bien démarrer :

- Gardez les templates petits et ciblés (un template = un type de projet).
- Documentez les variables dans `copier.yml` pour faciliter l'adoption.
- Testez la génération dans un répertoire temporaire avant de l'utiliser en production.
- Versionnez vos templates (git tags) pour contrôler les mises à jour sur les projets existants.
- Fournissez un fichier d'exemples `answers` pour les utilisateurs qui veulent automatiser la génération.

## Références

- [Documentation officielle de `copier`](https://copier.readthedocs.io/en/stable/)
- [Exemple d'une source d'inspiration d'un template copier `uv`](https://github.com/lukin0110/uv-copier/tree/main)

- Exemple local de template utilisé dans ce workspace : [copier-uv-python-project/copier.yml](copier-uv-python-project/copier.yml)

---
