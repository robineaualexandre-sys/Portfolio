---
title: Windows Autopilot
---

{% include site-style.html %}

# Windows Autopilot

## Contexte

À l'arrivée, la préparation des postes reposait sur un déploiement d'images Acronis propagées via des disques durs externes…

<figure>
  <img src="{{ site.baseurl }}/assets/images/autopilot/schema-avant-apres.png" alt="Schéma avant/après du processus de déploiement">
  <figcaption>Passage d'un déploiement manuel par imagerie à un provisioning zero-touch</figcaption>
</figure>

---

## Objectifs

- Remplacer le clonage d'image par un déploiement zero-touch
- Supprimer la dette de sécurité liée aux comptes administrateurs locaux d'imagerie
- Adapter automatiquement la configuration selon le type de poste

---

## Technologies utilisées

- Windows Autopilot
- Microsoft Intune
- Microsoft Entra ID
- Groupes dynamiques

---

## Réalisation

Mise en place de plusieurs profils de déploiement selon la typologie de poste (pédagogique, professeurs/administratifs, administratifs)…

<figure>
  <img src="{{ site.baseurl }}/assets/images/autopilot/profils-deploiement-liste.png" alt="Liste des profils de déploiement Autopilot">
  <figcaption>Une partie des profils de déploiement configurés dans Intune</figcaption>
</figure>

Deux modes ont été utilisés selon le contexte : le mode automatique pour les postes sans utilisateur affecté, et le mode utilisateur assigné pour les postes nécessitant un compte standard ou administrateur…

<figure>
  <img src="{{ site.baseurl }}/assets/images/autopilot/profil-autopilot-automatique.png" alt="Profil Autopilot en mode automatique">
  <figcaption>Configuration en mode automatique, sans compte utilisateur affecté</figcaption>
</figure>

<figure>
  <img src="{{ site.baseurl }}/assets/images/autopilot/profil-autopilot-utilisateur-assigne.png" alt="Profil Autopilot en mode utilisateur assigné">
  <figcaption>Configuration en mode utilisateur assigné, avec compte administrateur</figcaption>
</figure>

L'assignation des profils repose sur une distinction claire entre les groupes dynamiques Autopilot, alimentés dès l'enrôlement via `devicePhysicalIds`, et les groupes dynamiques de production…

<figure>
  <img src="{{ site.baseurl }}/assets/images/autopilot/groupe-dynamique-autopilot.png" alt="Groupe dynamique Autopilot">
  <figcaption>Groupe dynamique dédié à l'assignation du profil Autopilot</figcaption>
</figure>

<figure>
  <img src="{{ site.baseurl }}/assets/images/autopilot/regle-groupe-autopilot.png" alt="Règle d'affectation du groupe dynamique Autopilot">
  <figcaption>Règle basée sur `devicePhysicalIds`, alimentée dès l'enrôlement</figcaption>
</figure>

<figure>
  <img src="{{ site.baseurl }}/assets/images/autopilot/groupe-dynamique-production.png" alt="Groupe dynamique de production">
  <figcaption>Groupe dynamique utilisé une fois le poste provisionné</figcaption>
</figure>

<figure>
  <img src="{{ site.baseurl }}/assets/images/autopilot/regle-groupe-production.png" alt="Règle d'affectation du groupe dynamique de production">
  <figcaption>Règle basée sur les attributs classiques de l'appareil (nom, appartenance Entra ID…)</figcaption>
</figure>

---

## Résultats

- Suppression du recours aux images Acronis…
- Élimination du compte administrateur local…
- Rattachement au tenant Entra ID automatisé…
- Autopilot propagé à l'ensemble du parc, profils en production
- Configuration adaptée automatiquement au type d'usage

<figure>
  <img src="{{ site.baseurl }}/assets/images/autopilot/appareils-autopilot-liste.png" alt="Liste des appareils Autopilot enregistrés">
  <figcaption>Vue des appareils avec leur profil affecté et leur état de synchronisation</figcaption>
</figure>

---

## Compétences démontrées

- Windows Autopilot
- Microsoft Intune
- Microsoft Entra ID
- Groupes dynamiques
- Industrialisation du déploiement de parc
- Réduction de la dette de sécurité
