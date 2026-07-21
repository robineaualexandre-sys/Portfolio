---
title: Windows Autopilot
---

# Windows Autopilot

## Contexte

À mon arrivée, la préparation des postes reposait sur un déploiement d'images Acronis propagées via des disques durs externes...

![Schéma avant/après du processus de déploiement]({{ site.baseurl }}/assets/images/autopilot/schema-avant-apres.png)
*Passage d'un déploiement manuel par imagerie à un provisioning zero-touch*

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

Mise en place de plusieurs profils de déploiement selon la typologie de poste (pédagogique, professeurs/administratifs, administratifs)...

![Liste des profils de déploiement Autopilot]({{ site.baseurl }}/assets/images/autopilot/profils-deploiement-liste.png)
*Une partie des profils de déploiement configurés dans Intune*

Deux modes ont été utilisés selon le contexte : le mode automatique pour les postes sans utilisateur affecté, et le mode utilisateur assigné pour les postes nécessitant un compte standard...

![Profil Autopilot en mode automatique]({{ site.baseurl }}/assets/images/autopilot/profil-autopilot-automatique.png)
*Configuration en mode automatique, sans compte utilisateur affecté*

![Profil Autopilot en mode utilisateur assigné]({{ site.baseurl }}/assets/images/autopilot/profil-autopilot-utilisateur-assigne.png)
*Configuration en mode utilisateur assigné, avec compte standard*

L'assignation des profils repose sur une distinction claire entre les groupes dynamiques Autopilot, alimentés dès l'enrôlement via `devicePhysicalIds`, et les groupes dynamiques de production...

![Groupe dynamique Autopilot]({{ site.baseurl }}/assets/images/autopilot/groupe-dynamique-autopilot.png)
*Groupe dynamique dédié à l'assignation du profil Autopilot*

![Règle d'affectation du groupe dynamique Autopilot]({{ site.baseurl }}/assets/images/autopilot/regle-groupe-autopilot.png)
*Règle basée sur `devicePhysicalIds`, alimentée dès l'enrôlement*

![Groupe dynamique de production]({{ site.baseurl }}/assets/images/autopilot/groupe-dynamique-production.png)
*Groupe dynamique utilisé une fois le poste provisionné*

![Règle d'affectation du groupe dynamique de production]({{ site.baseurl }}/assets/images/autopilot/regle-groupe-production.png)
*Règle basée sur les attributs classiques de l'appareil (nom, appartenance Entra ID...)*

---

## Résultats

- Suppression du recours aux images Acronis...
- Élimination du compte administrateur local...
- Rattachement au tenant Entra ID automatisé...
- Autopilot propagé à l'ensemble du parc, profils en production
- Configuration adaptée automatiquement au type d'usage

![Liste des appareils Autopilot enregistrés]({{ site.baseurl }}/assets/images/autopilot/appareils-autopilot-liste.png)
*Vue des appareils avec leur profil affecté et leur état de synchronisation*

---

## Compétences démontrées

- Windows Autopilot
- Microsoft Intune
- Microsoft Entra ID
- Groupes dynamiques
- Industrialisation du déploiement de parc
- Réduction de la dette de sécurité
