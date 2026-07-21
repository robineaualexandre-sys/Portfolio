---
title: Refonte de l'architecture de gestion Intune
---

{% include site-style.html %}


# Refonte de l'architecture de gestion Intune

## Contexte

Avec la croissance du parc (plus de 780 terminaux répartis sur deux établissements), la gestion historique par groupes affectés manuellement montrait ses limites : délais de propagation des stratégies, perte de visibilité sur "qui est configuré comment", multiplication des groupes redondants.

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

Séparation des stratégies en deux catégories : les stratégies "immuables" (ex. blocage des jeux), déployées une fois pour toutes sur les grands groupes ("Tous les appareils"), et les stratégies "spécifiques", ciblées par filtres plutôt que par groupes dédiés.

Mise en place d'une convention de nommage `[Machines ciblées] Paramètre configuré` (ex. `[ADMIN] Credential Guard`, `[PROF] Wifi Profs`) pour identifier en un coup d'œil quelles machines sont concernées par chaque stratégie.

Migration progressive des groupes (TECHNO, STI, etc.) d'un mode affecté manuel vers un mode dynamique, avec des règles d'appartenance précises (`deviceTrustType`, `displayName`), après avoir identifié et corrigé un problème de doublons lié aux appareils "registered" vs "joined" dans Entra ID.

Conception de trois filtres couvrant l'ensemble du parc (Professeurs, Pédagogique, Administratif), basés sur les conventions de nommage des machines et sur des catégories d'appareils, avec le type de jointure Entra systématiquement précisé pour éviter les erreurs de ciblage. Ces filtres sont devenus le mécanisme central de ciblage des stratégies spécifiques.

Exploration des Ensembles de stratégies pour tenter de fiabiliser le pré-provisionnement Autopilot. Après mise en œuvre, le résultat s'est révélé peu concluant au regard de la complexité ajoutée : retour à une architecture reposant uniquement sur les filtres, jugée plus simple à maintenir et à faire évoluer.

---

## Résultats

- Réduction du nombre de groupes nécessaires à la gestion du parc
- Élimination des doublons de machines dans les groupes dynamiques
- Ciblage des stratégies spécifiques fiabilisé et simplifié grâce aux filtres
- Architecture finale volontairement simplifiée après arbitrage entre deux approches

---

## Compétences démontrées

- Microsoft Intune
- Microsoft Entra ID
- Groupes dynamiques
- Filtres Intune
- Windows Autopilot
- Architecture de gestion de parc
