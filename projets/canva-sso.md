# Intégration Canva en SSO via Microsoft Entra ID

## Contexte

Éligibilité de l'établissement à l'offre Canva pour l'Éducation, avec la volonté 
de proposer un accès simplifié et sécurisé à l'ensemble des utilisateurs du 
domaine, sans gestion de mots de passe supplémentaires.

## Objectif

Permettre à tous les membres du domaine de se connecter à Canva avec leur compte 
Microsoft existant, en configurant l'authentification unique (SSO) via SAML.

## Actions menées

- Vérification de la compatibilité SSO entre Canva et Microsoft Entra ID 
  (documentation croisée Canva + Microsoft)
- Création d'un compte administrateur Canva dédié via un compte de service
- Configuration de l'application d'entreprise dans Entra ID :
  - URL de connexion SAML2 configurée
  - Identificateur d'entité renseigné
- Attribution manuelle du profil "Enseignants" aux comptes concernés, Entra ID 
  ne permettant pas cette distinction automatiquement pour ce cas d'usage

## Résultats

- Authentification unique fonctionnelle pour tous les membres du domaine
- Suppression du besoin de gérer des identifiants Canva séparés
- Accès facilité pour les enseignants dans leurs usages pédagogiques quotidiens

## Compétences mobilisées

`Microsoft Entra ID` `SAML` `SSO` `Applications d'entreprise` `Gestion des identités`
