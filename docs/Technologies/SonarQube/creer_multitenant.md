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

L'idée est donc de centraliser ces coûts en créant une instance SonarQube multi-tenant. Cette approche permet
A partir du principe
