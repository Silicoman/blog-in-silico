---
title: "SonarQube multi-tenant"
description: "Comment créer un SonarQube multi-tenant"
hide:
    - toc
icon: lucide/server
---

## Pourquoi un SonarQube multi-tenant ?

Les équipes de developpement ont besoin d'un outil de qualité de code
pour analyser leurs projets. SonarQube est un outil populaire pour cette tâche.

La version SonarQube Community Edition est gratuite. Les équipes déploient
chacune une instance SonarQube pour analyser leurs projets. Cependant, cette
approche peut devenir coûteuse en termes de ressources et de maintenance.
On observe souvent des instances non mis à jour, des problèmes de sécurité.

!!! warning "Community Edition"
    La version Community Edition de SonarQube est gratuite mais possède
    des limitations comme la parallélisation. Pour bénéficier de fonctionnalités
    avancées, il est nécessaire de passer à une version sous licence.
    Cependant, vous pouvez provisionner plusieurs instances SonarQube Community Edition
    pour vos équipes pour limiter l'effet noisy neighbor en ayant une isolation
    plus élevée. Mais cela peut devenir coûteux en termes de ressources et
    de maintenance.

L'idée est donc de centraliser ces coûts en créant une instance SonarQube multi-tenant au niveau applicatif.


## Fonctionnalité d'un tenant

Il faut définir dans premier quels sont les besoins.
On catégorise les fonctionnalités par priorité et par
le niveau de configuration avec 4 niveaux.

- Fonction à usage autonome 
- Fonction avec permission possible
- Fonction nécessitant un rôle admin
- Fonction qu'on veut durcir au niveau de l'instance

Prenons quelques exemples :

| fonction | priorité | niveau d'autorisation|
| création projet | critique | autonome |
| création quality gate | haute | permission mais risque noisy neighbor|
| modifier quality gate | haute | permission possible |
| création/maj groupe | critique| admin |
| création template permission| critique | admin |
| tags sur les projets | nice-to-have| autonome + durcissement|

On identifie avec les utilisateurs les fonctionnalités qu'il faut absolument automatisé pour accueillir les premiers tenants. On en profite pour se projeter pour voir ce qu'il faudra faire. La plupart des fonctions pourront temporairement être réalisé à la main par l'équipe mais devront suivre la future convention en attendant. 

!!! warning "consulté les utilisateurs les mains dans les poches"
   Vous devez connaître votre produit et présenter en séance des scénarios sur des fonctionnalités qui pourraient être un facteur de tension. Exemple : comment allez vous géré un mécanisme d'authentification si le prérequis est d'avoir un vpn qui nécessite 2 validations. Proposez des hypothèses et ralliez les à votre cause.



