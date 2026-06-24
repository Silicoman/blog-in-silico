---
title: "Mettre en oeuvre un produit multi-tenant"
description: "Lorsqu'on cherche la rationalisation d'un service socle d'infrastructure ou d'outillage, on doit faire cohabiter plusieurs équipes sur ce même service, le multi-tenant. La spécialisation d'une équipe à ce service permet de définir les besoins et la configuration nécessaire aux utilisateurs."
tags:
  - paradigme
---

La problématique des tenants est souvent observé dans les grosses structures >100 développeurs. On décide de mutualiser un service utilisé par les équipes et on y met un responsable. Les développeurs ont déjà une application à maintenir, s'occuper d'un gitlab, sonarqube, nexus, vault, dependency-track, keycloak devrait être leur dernier des soucis. De plus, la dispersion dans la maintenance des outils peut entraîner une sous optimisation des usages. Les équipes ne veulent plus la responsabilité du service et les coûts associés. Mais en même temps, ils veulent accéder à toutes les fonctionnalités du service.

## Qu'est ce que le multi-tenant ?

Le multi-tenant est la volonté de faire cohabiter plusieurs équipes, périmètres sur une même solution technique. Typiquement, vous avez un serveur jenkins ou plusieurs équipes peuvent créer des jobs en autonomie. Chaque équipe par leur usage peuvent dégrader l' expérience de leur voisin soit en monopolisant des ressources à disposition ou créé des régressions. 
Une équipe A spamme la queue des jobs à disposition, l'équipe B doit attendre X heures. L'équipe A a modifié une variable d'environnement que l'équipe B consomme.

Lorsque vous réalisez une maintenance, une montée de version, c'est plusieurs équipes que vous impactez à la fois.

Le paradigme du multi-tenant est donc une simplification des consommateurs qui est déporté par l'équipe tenant.

En fonction de si vous décidez de partir d'un brownfield ou greenfield, la satisfaction utilisateur dépendra de ce que vous leur retirez et ce que vous leur permettrez par rapport aux usages de leur ancien modèle.

## Comparaison des différents modèles opérationnelles

Le multi-tenant peut sembler un choix évident à large échelle, en réalité ce n'est pas automatique. Je distingue trois modèles qui reflètent différents dispositifs sur différentes échelles d'organisation.

| organisation | avantages | inconvénients | questions ouvertes|
mono-tenant | autonomie dans la personnalisation, pas de régression subites | nécessité d'une expertise pour des usages intensives et gestion de la maintenance, temps dédié | est-ce que mon expertise est perreine? est-ce que mon cout infra reservé est viable  ? |
multi-tenant | partage des ressources infra, mutualisation des gestes de maintenance, expertise du service | contraintes dans l'usage, une équipe dédiée coûte | est-ce que le service me permet de faire du multi-tenant? est-ce que j'ai une économie d'échelle ? je supporte +10 équipes. |
SaaS | paye | pas d'expertise interne, pas de flexibilité| est-ce que le modèle économique est pertinent




## Définir les capacités du service


## Leviers d'optimisation

### Expérience utilisateur

### Automatisation et extension

