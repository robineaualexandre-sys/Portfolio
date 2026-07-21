---
title: Gouvernance Microsoft Teams et cycle de vie des identités
---

# Gouvernance Microsoft Teams et cycle de vie des identités

## Contexte

Avec plusieurs centaines de comptes utilisateurs et de nombreux Teams créés par les élèves et les enseignants, le risque d'accumulation de ressources orphelines (Teams sans propriétaire, comptes et appareils inactifs) rendait nécessaire la mise en place d'une gouvernance active.

---

## Objectifs

- Garder un environnement Microsoft 365 propre et maîtrisé dans la durée
- Standardiser la création des Teams pédagogiques
- Automatiser le nettoyage des ressources devenues obsolètes

---

## Technologies utilisées

- Microsoft Entra ID
- Microsoft Teams
- Microsoft Graph PowerShell

---

## Réalisation

Étude d'une solution (KoXo) permettant d'assigner un modèle de Teams par classe en amont, pour standardiser la création plutôt que de laisser chaque enseignant partir d'une base différente.

Mise en place d'une réflexion sur l'expiration des groupes Microsoft 365, pour identifier et supprimer les Teams créés par les élèves n'ayant plus de propriétaire actif.

Face à environ 5500 entrées d'appareils dans Entra ID (contre 500 dans Intune), mise en place d'une procédure de désactivation puis suppression progressive des appareils inactifs, en s'appuyant sur les cmdlets Microsoft Graph (`Update-MgDevice`, `Remove-MgDevice`) et sur une période de grâce avant suppression définitive.

---

## Résultats

- Réduction du volume de ressources orphelines dans le tenant
- Standardisation de la création des Teams pédagogiques
- Procédure de nettoyage documentée et reproductible pour les appareils obsolètes

---

## Compétences démontrées

- Microsoft Entra ID
- Microsoft Teams
- Microsoft Graph PowerShell
- Gouvernance M365
