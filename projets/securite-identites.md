# Sécurisation des identités et des accès

## Contexte

Dans le cadre de la sécurisation du parc (postes Administratifs, Professeurs et 
Pédagogiques), plusieurs chantiers ont été menés pour renforcer la protection 
des comptes et des accès, sans complexifier l'expérience utilisateur au quotidien.

## Objectif

Réduire la surface d'attaque liée aux comptes à privilèges et à l'authentification, 
tout en gardant une administration simple à maintenir dans la durée.

## Actions menées

### Windows Hello for Business
Déploiement d'une stratégie de Protection de compte (Sécurité du point de 
terminaison > Protection de compte) pour remplacer l'authentification par mot 
de passe sur les postes éligibles, via un groupe dédié "Appareils Windows Hello".

### Credential Guard
Activation de la sécurité basée sur la virtualisation sur les machines 
Administratives, via le Catalogue de paramètres Intune. Tests de non-régression 
réalisés sur connexions RDP/TSE avant généralisation à l'ensemble du parc concerné.

### Gestion différenciée des droits d'administration locale
Mise en place de groupes d'administration locale distincts selon le type de poste 
(Administratif, Professeur, Pédagogique), pour éviter les conflits de stratégies 
lorsqu'un même groupe utilisateur reçoit deux règles différentes sur une même 
machine. Compartimentation des règles par profil plutôt que règle unique globale.

### Windows LAPS
Étude et mise en œuvre d'une solution de gestion de l'administrateur local, en 
remplacement d'une gestion manuelle du compte, en s'appuyant sur l'intégration 
native Windows LAPS ↔ Microsoft Entra ID.

## Résultats

- Réduction des invites d'élévation de privilèges non maîtrisées
- Suppression des conflits de stratégies liés aux doublons de groupes d'admin locaux
- Authentification renforcée sans mot de passe sur les postes éligibles
- Gestion du compte administrateur local industrialisée et traçable

## Compétences mobilisées

`Microsoft Intune` `Microsoft Entra ID` `Windows Hello for Business` 
`Credential Guard` `Windows LAPS` `Sécurité des identités` `Gestion des accès`
