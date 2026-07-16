---
title: Sécurisation des identités et des accès | Portfolio
---

# Sécurisation des identités et des accès

## Contexte

Dans le cadre de la sécurisation du parc (postes Administratifs, Professeurs et Pédagogiques), plusieurs chantiers ont été menés pour renforcer la protection des comptes et des accès, sans complexifier l'expérience utilisateur au quotidien.

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
- Credential Guard
- Windows LAPS

---

## Réalisation

Déploiement d'une stratégie de Protection de compte (Sécurité du point de terminaison > Protection de compte) pour activer Windows Hello for Business sur les postes éligibles, via un groupe dédié "Appareils Windows Hello".

Activation de la sécurité basée sur la virtualisation (Credential Guard) sur les machines Administratives, via le Catalogue de paramètres Intune, avec tests de non-régression sur connexions RDP/TSE avant généralisation.

Mise en place de groupes d'administration locale distincts selon le type de poste (Administratif, Professeur, Pédagogique), pour éviter les conflits de stratégies lorsqu'un même groupe utilisateur reçoit deux règles différentes sur une même machine — compartimentation des règles par profil plutôt que règle unique globale.

Étude et mise en œuvre de Windows LAPS pour la gestion de l'administrateur local, en remplacement d'une gestion manuelle du compte, en s'appuyant sur l'intégration native Windows LAPS ↔ Microsoft Entra ID.

---

## Résultats

- Réduction des invites d'élévation de privilèges non maîtrisées
- Suppression des conflits de stratégies liés aux doublons de groupes d'admin locaux
- Authentification renforcée sans mot de passe sur les postes éligibles
- Gestion du compte administrateur local industrialisée et traçable

---

## Compétences démontrées

- Microsoft Intune
- Microsoft Entra ID
- Windows Hello for Business
- Credential Guard
- Windows LAPS
- Gestion des identités et des accès
