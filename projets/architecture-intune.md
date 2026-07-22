---
title: Refonte de l'architecture de gestion Intune
---

# Refonte de l'architecture de gestion Intune

<figure>
  <img src="{{ site.baseurl }}/assets/images/architecture-intune/avant-apres.png" alt="Schéma avant/après de l'architecture de gestion Intune">
  <figcaption>Passage d'une gestion via Intune Education et groupes assignés à une gestion via Intune Admin, filtres dynamiques et groupes dynamiques</figcaption>
</figure>

---

## Contexte

Avec la croissance du parc (plus de 780 terminaux répartis sur deux établissements), la gestion historique par groupes affectés manuellement montrait ses limites : délais de propagation des stratégies, perte de visibilité sur "qui est configuré comment", multiplication des groupes redondants.

À mon arrivée, la solution Entra ID venait d'être déployée et l'équipe en place gérait les stratégies via Intune Education, ce qui générait de nombreux conflits ainsi qu'une duplication importante des groupes. L'ensemble des groupes fonctionnait en mode affecté manuel.

Les stratégies étaient appliquées depuis Intune Education (Groupes > Paramètres de l'appareil Windows), soit sur l'ensemble du parc, soit via de grands groupes parapluies remontant des sous-groupes manuels par filière (administratif, pédagogique), à l'exception des enseignants, gérés depuis un groupe manuel dédié.

<figure>
  <img src="{{ site.baseurl }}/assets/images/architecture-intune/ancien-ciblage.png" alt="Ancien ciblage des stratégies via Intune Education">
  <figcaption>Ancien mode de ciblage : application globale ou via des groupes parapluies remontant des sous-groupes manuels</figcaption>
</figure>

On observait notamment des conflits récurrents sur les stratégies de fond d'écran de verrouillage, de PC partagé (désynchronisation OneDrive) et de configuration Wi-Fi, plusieurs stratégies équivalentes ou contradictoires étant appliquées aux mêmes appareils.

---

## Objectifs

- Simplifier et fiabiliser l'architecture de gestion du parc
- Réduire la dépendance aux groupes au profit des filtres
- Garder une visibilité claire sur le ciblage de chaque stratégie

---

## Technologies utilisées

- Microsoft Intune
- Microsoft Entra ID
- Groupes dynamiques
- Filtres Intune
- Windows Autopilot

---

## Réalisation

Premier chantier : revoir la convention de nommage des appareils, condition préalable indispensable à la mise en place de règles d'appartenance dynamique fiables.

<figure>
  <img src="{{ site.baseurl }}/assets/images/architecture-intune/ancien-nommage-pedago.png" alt="Ancienne convention de nommage des appareils">
  <figcaption>Ancien système de nommage, informel et propre à chaque filière</figcaption>
</figure>

<figure>
  <img src="{{ site.baseurl }}/assets/images/architecture-intune/nommage-appareils.png" alt="Convention de nommage des appareils actuel">
  <figcaption>Nouvelle convention de nommage, structurée et cohérente</figcaption>
</figure>

Migration progressive des groupes (TECHNO, STI, etc.) d'un mode affecté manuel vers un mode dynamique, avec des règles d'appartenance précises (`deviceTrustType`, `displayName`), après avoir identifié et corrigé un problème de doublons lié aux appareils "registered" vs "joined" dans Entra ID.

<figure>
  <img src="{{ site.baseurl }}/assets/images/architecture-intune/groupes-dynamiques.png" alt="Liste des groupes Entra ID dynamiques">
  <figcaption>Liste des groupes de sécurité dynamiques créés pour la gestion du parc</figcaption>
</figure>

<figure>
  <img src="{{ site.baseurl }}/assets/images/architecture-intune/groupes-dynamiques-regles.png" alt="Règle d'appartenance dynamique d'un groupe">
  <figcaption>Exemple de règle d'appartenance dynamique (type de jointure Entra + convention de nommage)</figcaption>
</figure>

Conception de 15 filtres dynamiques couvrant l'ensemble du parc, combinant type d'appareil, site et type d'utilisateur ciblé (ex. `Appareils MR ELV EXM`, `Appareils SC ELV PRF`), avec le type de jointure Entra systématiquement précisé pour éviter les erreurs de ciblage. Ces filtres sont devenus le mécanisme central de ciblage des stratégies spécifiques.

<figure>
  <img src="{{ site.baseurl }}/assets/images/architecture-intune/filtres-liste.png" alt="Vue d'ensemble des filtres d'attribution">
  <figcaption>Vue d'ensemble des 15 filtres d'attribution dynamiques</figcaption>
</figure>

<figure>
  <img src="{{ site.baseurl }}/assets/images/architecture-intune/filtre-regle-detail.png" alt="Détail de la règle d'un filtre combiné">
  <figcaption>Détail de la syntaxe d'un filtre combiné, type de jointure Entra précisé</figcaption>
</figure>

Mise en place d'une convention de nommage `[Machines ciblées] Paramètre configuré` (ex. `[ADMIN] Credential Guard`, `[PROF] Wifi Profs`) pour identifier en un coup d'œil quelles machines sont concernées par chaque stratégie.

<figure>
  <img src="{{ site.baseurl }}/assets/images/architecture-intune/convention-strategies.png" alt="Liste des stratégies avec convention de nommage">
  <figcaption>Stratégies de configuration respectant la convention de nommage `[Machines ciblées] Paramètre`</figcaption>
</figure>

Exploration des Ensembles de stratégies pour tenter de fiabiliser le pré-provisionnement Autopilot. Après mise en œuvre, le résultat s'est révélé peu concluant au regard de la complexité ajoutée : retour à une architecture reposant uniquement sur les filtres, jugée plus simple à maintenir et à faire évoluer.

<figure>
  <img src="{{ site.baseurl }}/assets/images/architecture-intune/assignation-filtre.png" alt="Assignation d'une stratégie via filtre dynamique">
  <figcaption>Assignation d'une stratégie de configuration via un filtre dynamique</figcaption>
</figure>

<figure>
  <img src="{{ site.baseurl }}/assets/images/architecture-intune/assignation-groupe.png" alt="Assignation d'une application via groupe dynamique">
  <figcaption>Assignation d'une application via un groupe dynamique, en complément du ciblage des stratégies par filtre</figcaption>
</figure>

---

## Résultats

- Réduction du nombre de stratégies Intune Admin d'environ 60 à une vingtaine, avant remontée progressive à 40 stratégies avec l'ajout de nouveaux besoins
- Élimination des conflits récurrents (fond d'écran de verrouillage, PC partagé/OneDrive, Wi-Fi)
- Élimination des doublons de machines dans les groupes dynamiques
- Ciblage des stratégies spécifiques fiabilisé et simplifié grâce aux 15 filtres dynamiques
- Architecture finale volontairement simplifiée après arbitrage entre deux approches

---

## Compétences démontrées

- Microsoft Intune
- Microsoft Entra ID
- Groupes dynamiques
- Filtres Intune
- Windows Autopilot
- Architecture de gestion de parc
