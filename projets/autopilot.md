---
title: Windows Autopilot | Portfolio
---

# Windows Autopilot

## Contexte

À mon arrivée, la préparation des postes reposait sur un déploiement d'images Acronis propagées via des disques durs externes, branchés manuellement sur chaque appareil. La mise en service nécessitait ensuite une connexion sur un compte administrateur local (non supprimé après coup), un renommage manuel de l'appareil, un redémarrage, une reconnexion, puis un rattachement au tenant via un compte Microsoft professionnel — une méthode chronophage et porteuse d'une dette de sécurité silencieuse.

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

Mise en place d'une procédure reproductible d'intégration à Autopilot via l'OOBE (clé USB de préparation, script d'enrôlement PowerShell, vérification de l'affectation au profil de déploiement, suivi de la synchronisation et de l'état "En attente"), en remplacement complet du clonage d'image et de la préparation manuelle.

Distinction entre deux logiques de groupes : les groupes Autopilot, alimentés dès l'enrôlement via `devicePhysicalIds` (étiquette de commande), utilisés uniquement pour la phase de déploiement initial ; et les groupes de production, alimentés ensuite via `deviceTrustType` et `displayName`, pour la gestion courante du poste.

Conception de trois profils de déploiement différenciés selon la typologie de poste : les postes Pédagogiques en mode automatique et compte standard sans affectation utilisateur ; les postes Professeurs et Administratifs en mode géré par utilisateur avec compte administrateur, l'affectation utilisateur n'étant activée que pour les postes Administratifs.

---

## Résultats

- Suppression du recours aux images Acronis et aux disques durs externes pour la préparation des postes
- Élimination du compte administrateur local resté actif après imagerie — fermeture d'une dette de sécurité qui n'était plus tracée
- Rattachement au tenant Entra ID automatisé dès le déballage, sans étape manuelle de renommage ou de reconnexion
- Autopilot propagé à l'ensemble du parc, profils en production
- Configuration adaptée automatiquement au type d'usage

---

## Compétences démontrées

- Windows Autopilot
- Microsoft Intune
- Microsoft Entra ID
- Groupes dynamiques
- Industrialisation du déploiement de parc
- Réduction de la dette de sécurité
