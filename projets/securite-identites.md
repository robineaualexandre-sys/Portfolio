---
title: Sécurisation des identités et des accès
---

{% include site-style.html %}

# Sécurisation des identités et des accès

## Contexte

Dans le cadre de la sécurisation du parc, plusieurs chantiers ont été menés pour renforcer la protection des comptes et des accès, sans complexifier l'expérience utilisateur au quotidien.

---

## Objectifs

- Réduire la surface d'attaque liée aux comptes à privilèges
- Renforcer l'authentification sans complexifier l'usage quotidien
- Garder une administration simple à maintenir dans la durée

---

## Technologies utilisées

- Microsoft Intune
- Microsoft Entra ID
- Windows Hello for Business
- Windows LAPS

---

## Réalisation

Déploiement de Windows Hello for Business selon une architecture "liste blanche" : une stratégie globale désactive Windows Hello Entreprise par défaut sur l'ensemble du parc, puis une stratégie ciblée réactive la fonctionnalité uniquement pour le groupe "Appareils Windows Hello" (postes Professeurs et Administratifs), avec une politique de PIN définie (6 caractères minimum). Ce même périmètre fait l'objet d'une désactivation contrôlée de Credential Guard, pour résoudre des erreurs d'authentification rencontrées en session RDP sur ces postes.

Mise en place de groupes d'administration locale distincts selon le type de poste (Administratif, Professeur, Pédagogique), pour éviter les conflits de stratégies lorsqu'un même groupe utilisateur reçoit deux règles différentes sur une même machine — compartimentation des règles par profil plutôt que règle unique globale.

Étude et mise en œuvre de Windows LAPS pour la gestion de l'administrateur local, en remplacement d'une gestion manuelle du compte, en s'appuyant sur l'intégration native Windows LAPS ↔ Microsoft Entra ID.

---

## Résultats

- Authentification renforcée sans mot de passe sur les postes éligibles
- Suppression des conflits de stratégies liés aux doublons de groupes d'admin locaux
- Gestion du compte administrateur local industrialisée et traçable

---

## Compétences démontrées

- Microsoft Intune
- Microsoft Entra ID
- Windows Hello for Business
- Windows LAPS
- Gestion des identités et des accès
