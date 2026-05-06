---
title: Scaffolding avec copier
description: "générer des nouveaux projets à partir d'archetype à l'aide de copier. Accélère l'initialisation
et vous donne les conditions pour les mettre à jour en masse."
---

## Qu'est-ce que copier ?

`copier` est un outil de scaffolding (génération de projet) léger et moderne basé sur des templates Jinja2. Il permet de générer rapidement un nouveau projet à partir d'un modèle (template) et de conserver la capacité de le mettre à jour ensuite lorsque le template évolue.

## Pourquoi utiliser `copier` ?

- **Simplicité** : les templates sont des fichiers texte utilisant Jinja2, faciles à lire et éditer.
- **Mise à jour** : possibilité d'appliquer ensuite les évolutions du template sur les projets existants.
- **Flexible** : supporte les templates locaux, distants ou dans des dépôts Git.

## Installation

Installez `copier` dans votre environnement Python :

```bash
pip install copier
```

Vous pouvez aussi l'installer avec `pipx` si vous préférez isoler l'exécutable :

```bash
pipx install copier
```

## Génération — Exemple rapide

Générer un projet à partir d'un template local :

```bash
copier copy /chemin/vers/mon-template ./nouveau-projet
```

Générer un projet à partir d'un template distant (ex. dépôt Git) :

```bash
copier copy https://github.com/mon-org/mon-template.git ./nouveau-projet
```

Lancez `copier --help` ou `copier copy --help` pour voir les options disponibles.

## Structure typique d'un template

Un template `copier` est simplement un dossier contenant des fichiers et, optionnellement, un fichier de configuration qui décrit les variables demandées à l'utilisateur.

Exemple minimal :

```
my-template/
├── copier.yml         # (optionnel) description des variables et valeurs par défaut
├── README.md          # peut contenir {{ project_name }} (Jinja2)
└── {{ project_slug }}/
		└── main.py
```

Les fichiers du template peuvent contenir des expressions Jinja2, par exemple : `{{ project_name }}`.

## Définir des variables (aperçu)

Le template peut déclarer un petit fichier YAML (par exemple `copier.yml`) pour fournir des valeurs par défaut et des aides pour les questions posées pendant la génération. Exemple simple :

```yaml
# copier.yml (exemple)
project_name:
	default: MonProjet
	help: "Nom affiché du projet"
project_slug:
	default: mon-projet
	help: "Nom du dossier / slug utilisé pour le package"
```

Pendant la génération, `copier` vous posera ces questions et utilisera les réponses pour rendre les fichiers du template.

## Mise à jour d'un projet généré

Une des forces de `copier` est la capacité à appliquer des changements faits au template sur un projet déjà généré (workflow de mise à jour). Consultez la commande d'update dans la documentation officielle pour gérer les conflits et valider les changements avant application.

## Bonnes pratiques

- Gardez les templates petits et ciblés (un template = un type de projet).
- Documentez les variables dans `copier.yml` pour faciliter l'adoption.
- Testez la génération dans un répertoire temporaire avant de l'utiliser en production.

## Ressources

- Documentation officielle de `copier` (site du projet) — recherchez « copier scaffold ».
- Exemples de templates sur GitHub — cherchez "copier template".

---

Si vous voulez, j'ajoute un exemple de template minimal prêt à l'emploi et une section pas-à-pas avec captures d'écran.
