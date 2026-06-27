---
title: "Mettre en oeuvre un produit multi-tenant"
description: "Lorsqu'on cherche la rationalisation d'un service socle d'infrastructure ou d'outillage, on doit faire cohabiter plusieurs équipes sur ce même service, le multi-tenant. La spécialisation d'une équipe à ce service permet de définir les besoins et la configuration nécessaire aux utilisateurs."
tags:
  - paradigme
---

Le multi-tenant est un concept qui permet de partager les ressources et le
service entre plusieurs équipes ou services tout en maintenant leur isolation
respective.

La problématique des tenants est souvent observé dans les grosses structures >100
développeurs. On décide de mutualiser un service utilisé par les équipes
et on y met un responsable. Les développeurs ont déjà une application à
maintenir, s'occuper de tout un écosystème d'outils (sonarqube, nexus, vault,
dependency-track, keycloak, etc) devrait être leur dernier des soucis. De plus,
la dispersion dans la maintenance des outils peut entraîner une sous optimisation
des usages. Les équipes ne veulent plus la responsabilité du service et les coûts
associés. Mais en même temps, ils veulent accéder à toutes les fonctionnalités du
service.

## Qu'est ce que le multi-tenant et ses défis ?

Le multi-tenant est la volonté de faire cohabiter plusieurs équipes, périmètres sur
ne même solution technique. Typiquement, vous avez un serveur jenkins ou plusieurs
équipes peuvent créer des jobs en autonomie. Chaque équipe par leur usage peuvent
dégrader l'expérience de leur voisin soit en monopolisant des ressources à disposition
ou créé des régressions.

- Une équipe A spamme la queue des jobs à disposition, l'équipe B doit attendre X heures.
- L'équipe A a modifié une variable d'environnement que l'équipe B consomme causant une régression.

Lorsque vous réalisez une maintenance, une montée de version, c'est plusieurs équipes
que vous impactez à la fois. Le droit à l'erreur est donc plus lourd de conséquence.
Il faut donc mettre en place des mécanismes de contrôle et de surveillance pour éviter
les régressions et garantir la qualité du service.

Le paradigme du multi-tenant est donc une simplification pour les consommateurs.
En revanche, il y a un transfert de charge et de complexité pour l'équipe qui
gère le service.

En fonction de si vous décidez de partir d'un brownfield ou greenfield, la
satisfaction utilisateur dépendra de ce que vous leur retirez et ce que vous leur
permettrez par rapport aux usages de leur ancien modèle.

## Comparaison des différents modèles opérationnelles

Le multi-tenant peut sembler un choix évident à large échelle, en
réalité ce n'est pas automatique. Je distingue trois modèles qui
reflètent différents dispositifs sur différentes échelles d'organisation.

| organisation | avantages | inconvénients | questions ouvertes|
| ------------ | --------- | ------------- | ----------------- |
| mono-tenant | autonomie dans la personnalisation, pas de régression subites | nécessité d'une expertise pour des usages intensives et gestion de la maintenance, temps dédié | est-ce que mon expertise est perreine? est-ce que mon cout infra reservé est viable  ? |
| multi-tenant | partage des ressources infra, mutualisation des gestes de maintenance, expertise du service | contraintes dans l'usage, une équipe dédiée vecteur de coût, problèmes plus complexes avec risques de SPOF | est-ce que le service me permet de faire du multi-tenant? Est-ce que j'ai besoin d'une expertise ? Est-ce que j'ai une économie d'échelle ? Ex: je supporte +10 équipes. |
| SaaS | simplification facturation | pas d'expertise interne, pas de flexibilité, modèle difficile en air-gap | Est-ce que le modèle économique est pertinent ? Est-ce que mon contexte me l'autorise ? Ex: sensibilité code source |


## Définir les capacités du service

Pour implémenter un modèle Multi-Tenant, il faut envisager des aspects clés tels que:

1. **Isolation**: Assurer une séparation claire des données pour chaque tenant.
2. **Scalabilité**: La capacité à gérer l'augmentation de la charge due au nombre croissant d'utilisateurs ou services.
3. **Performance**: Optimiser les ressources et le traitement afin qu'un tenant n'affecte pas négativement un autre.
4. **Disponibilité**: Quel SLA vous garantissez.
5. **Personnalisation**: à quel degrès acceptez vous de soit fournir des fonctionnalités différentes ou laisser la main à chaque tenant pour modifier


## Leviers d'optimisation

### Expérience utilisateur

L'expérience des utilisateurs doit rester cohérente, quel que soit le tenant avec lequel ils interagissent. Lorsqu'un développeur réalise une mobilité de projet,
il doit retrouver la majorité du fonctionnement du service partagé sans revenir
sur une courbe d'apprentissage.

L'absence d'attrition des plus anciens
se fera par un haut niveau de disponibilité. Personne ne veut utiliser un service cassé 1 jour sur 2. Sinon un expert montera une alternative qui sera en concurrence. Une qualité de service incite à l'augmentation de nouveaux
utilisateurs qui veulent aussi leur part du gateau.

- [x] Assurer une interface uniforme.
- [x] Maintenir la qualité du service sans dégradation ou interruption (désiré ou non)
- [x] identifier des indicateurs SLI/SLO pour suivre la santé du service

### :lucide-wrench: Automatisation

Pour répondre à une montée en charge des tenants, l'automatisation est un passage
obligatoire. Si vous n'aviez sur votre plateforme que quelques projets à gérer et que le lendemain on
vous annonce l'arrivée de 1000 developpeurs, le clic clic va devenir compliqué
à assumer.
Il y a plusieurs avantages concrets à l'automatisation:

- peu de risque d'écart de configuration
- délai d'accostage grandement réduit
- normes et contraintes davantage accepté par les utilisateurs par l'exercice
- garantir la reconstruction sur un incident
- capacité à faire évoluer et migrer des normes ou nouvelles fonctionnalités à
 l'échelle
- une équipe SN3 qui peut porter des problématiques SN1/SN2 et prendre de l'information des usages au contact

Pour y arriver, il faudra se poser les bonnes questions dans la construction du service. Vous pouvez fournir et améliorer le service au fur et à mesure que les
utilisateurs atterissent accélérant votre boucle de retour d'expérience.

- [x] identifier toutes les fonctionnalités obligatoires et optionnels qu'un projet aurait besoin
- [x] Utiliser des outils d'automatisation comme Terraform, Ansible, ou les développer pour gérer le provisioning et configuration de chacun
- [x] Proposer un mode d'accostage automatisé en self-service
- [x] Documenter les fonctionnalités et normes implémentés auprès de vos utilisateurs
- [x] Identifier les canaux pour qu'une équipe puisse diagnostiquer et s'auto remédier en autonomie
- [x] Développer des tests adaptés à votre contexte

## Conclusion

Lorsqu'on évoque les gains, on parle souvent du coût opex des serveurs et
éventuellement d'une ressource en moins par équipe. En réalité, il y a un
transfert des coûts dans la construction d'une offre avec une double
facturation au démarrage.
En fonction de la taille, les gains financiers ne seront donc pas évidents.
En revanche, si tout se passe correctement, les gains indirectes vont très
vite être perceptibles. Les équipes consommatrices seront libérés d'une charge
de maintenance et la prise en compte de la sécurité sera beaucoup plus élevé. Les équipes auront un accès à une expertise avancée. Avec une standardisation, vous diminuez la montée en compétence dans
chaque équipe tout en profitant par design du meilleur des capacités du service.

Un mono-tenant peut évoluer en un multi-tenant. Soyez malin et appliquez au mieux
ces principes fondateurs par design.
