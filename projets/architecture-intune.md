---
title: Refonte de l'architecture de gestion Intune
---

# Refonte de l'architecture de gestion Intune

## Contexte

À mon arrivée, la solution Entra ID venait d'être déployée et l'équipe en place gérait les stratégies via Intune Education, ce qui générait de nombreux conflits ainsi qu'une duplication importante des groupes. L'ensemble des groupes fonctionnait en mode affecté manuel, sur un parc de plus de 500 terminaux répartis sur deux établissements.

Les stratégies étaient appliquées depuis Intune Education (Groupes > Paramètres de l'appareil Windows), soit sur l'ensemble du parc, soit via de grands groupes parapluies remontant des sous-groupes manuels par filière (administratif, pédagogique), à l'exception des enseignants, gérés depuis un groupe manuel dédié.

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

Séparation des stratégies en deux catégories : les stratégies "immuables" (ex. blocage des jeux), déployées une fois pour toutes sur les grands groupes ("Tous les appareils"), et les stratégies "spécifiques", ciblées par filtres plutôt que par groupes dédiés.

Premier chantier : revoir la convention de nommage des appareils, condition préalable indispensable à la mise en place de règles d'appartenance dynamique fiables. Les groupes ont ensuite été progressivement basculés du mode affecté manuel vers le mode dynamique. En parallèle, les stratégies ont été migrées d'Intune Education vers Intune Admin, conformément aux recommandations Microsoft : les stratégies de configuration sont désormais déployées via des filtres dynamiques, tandis que les applications sont assignées via des groupes dynamiques.

Mise en place d'une convention de nommage `[Machines ciblées] Paramètre configuré` (ex. `[ADMIN] Credential Guard`, `[PROF] Wifi Profs`) pour identifier en un coup d'œil quelles machines sont concernées par chaque stratégie.

Migration progressive des groupes (TECHNO, STI, etc.) d'un mode affecté manuel vers un mode dynamique, avec des règles d'appartenance précises (`deviceTrustType`, `displayName`), après avoir identifié et corrigé un problème de doublons lié aux appareils "registered" vs "joined" dans Entra ID.

Conception de 15 filtres dynamiques couvrant l'ensemble du parc, combinant type d'appareil, site et type d'utilisateur ciblé (ex. Appareils MR ELV EXM, Appareils SC ELV PRF), avec le type de jointure Entra systématiquement précisé pour éviter les erreurs de ciblage. Ces filtres sont devenus le mécanisme central de ciblage des stratégies spécifiques.

Exploration des Ensembles de stratégies pour tenter de fiabiliser le pré-provisionnement Autopilot. Après mise en œuvre, le résultat s'est révélé peu concluant au regard de la complexité ajoutée : retour à une architecture reposant uniquement sur les filtres, jugée plus simple à maintenir et à faire évoluer.

---

## Résultats

- Réduction du nombre de stratégies Intune Admin d'environ 60 à une vingtaine, avant remontée progressive à 40 stratégies avec l'ajout de nouveaux besoins
- Élimination des conflits récurrents (fond d'écran de verrouillage, PC partagé/OneDrive, Wi-Fi)
- Élimination des doublons de machines dans les groupes dynamiques
- Ciblage des stratégies spécifiques fiabilisé et simplifié grâce aux 15 filtres dynamiques

---

## Compétences démontrées

- Microsoft Intune
- Microsoft Entra ID
- Groupes dynamiques
- Filtres Intune
- Windows Autopilot
- Architecture de gestion de parc
