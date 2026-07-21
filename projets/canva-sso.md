---
title: Canva SSO
---

{% include site-style.html %}

# Intégration Canva en SSO via Microsoft Entra ID

## Contexte

Éligibilité de l'établissement à l'offre Canva pour l'Éducation, avec la volonté de proposer un accès simplifié et sécurisé à l'ensemble des utilisateurs du domaine, sans gestion de mots de passe supplémentaires.

---

## Objectifs

- Permettre à tous les membres du domaine de se connecter à Canva avec leur compte Microsoft existant
- Configurer l'authentification unique via SAML
- Éviter la gestion d'identifiants séparés pour cette application

---

## Technologies utilisées

- Microsoft Entra ID
- SAML
- Applications d'entreprise

---

## Réalisation

Vérification de la compatibilité SSO entre Canva et Microsoft Entra ID à partir de la documentation croisée des deux éditeurs, puis création d'un compte administrateur Canva dédié via un compte de service.

Configuration de l'application d'entreprise dans Entra ID, avec l'URL de connexion SAML2 et l'identificateur d'entité renseignés.

Attribution manuelle du profil "Enseignants" aux comptes concernés, Entra ID ne permettant pas cette distinction automatiquement pour ce cas d'usage.

---

## Résultats

- Authentification unique fonctionnelle pour tous les membres du domaine
- Suppression du besoin de gérer des identifiants Canva séparés
- Accès facilité pour les enseignants dans leurs usages pédagogiques quotidiens

---

## Compétences démontrées

- Microsoft Entra ID
- SAML / SSO
- Applications d'entreprise
- Gestion des identités
