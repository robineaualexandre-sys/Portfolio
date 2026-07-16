# Windows Autopilot

## Contexte

À mon arrivée, la préparation des postes reposait sur un déploiement 
d'images Acronis propagées via des disques durs externes, branchés 
manuellement sur chaque appareil. La mise en service d'un poste nécessitait 
ensuite : connexion sur un compte administrateur local (non supprimé après 
coup), renommage manuel de l'appareil, redémarrage, reconnexion, puis 
rattachement au tenant via un compte Microsoft professionnel.

Cette méthode posait deux problèmes : un temps de préparation important par 
poste, et une dette de sécurité silencieuse — les comptes administrateurs 
locaux utilisés pour l'imagerie restaient actifs sur les machines en 
production, sans qu'aucune procédure ne prévoie leur suppression.

## Objectif

Remplacer ce processus manuel par un déploiement Autopilot standardisé, 
permettant un rattachement direct au tenant Entra ID dès le déballage du 
poste, sans intervention sur un compte administrateur local, et avec une 
configuration adaptée automatiquement au type de poste.

## Actions menées

### Procédure d'enrôlement standardisée
Mise en place d'une procédure reproductible d'intégration à Autopilot via 
l'OOBE (clé USB de préparation, script d'enrôlement PowerShell, vérification 
de l'affectation au profil de déploiement, suivi de la synchronisation et de 
l'état "En attente" avant mise à disposition du poste) — en remplacement 
complet du clonage d'image et de la préparation manuelle.

### Groupes Autopilot vs groupes de production
Distinction entre deux logiques de nommage et de règles dynamiques :
- **Groupes Autopilot** : alimentés dès l'enrôlement via 
  `devicePhysicalIds` (étiquette de commande), utilisés uniquement pour la 
  phase de déploiement initial
- **Groupes de production** : alimentés ensuite via `deviceTrustType` et 
  `displayName`, pour la gestion courante du poste une fois déployé

### Trois profils de déploiement différenciés
Conception de profils Autopilot distincts selon la typologie de poste, avec 
des paramètres volontairement différents :

| Paramètre | Pédagogique | Professeurs | Administratif |
|---|---|---|---|
| Mode de déploiement | Automatique | Géré par utilisateur | Géré par utilisateur |
| Type de compte | Standard | Administrateur | Administrateur |
| Déploiement préconfiguré | Non | Oui | Oui |
| Affecter un utilisateur | Non | Non | Oui |
| Modèle de nom d'appareil | Oui | Oui | Non |

Cette différenciation reflète des besoins réels : un poste Pédagogique 
partagé n'a pas besoin d'affectation utilisateur ni de droits admin, alors 
qu'un poste Administratif ou Professeur, utilisé nominativement, en a besoin.

## Résultats

- Suppression du recours aux images Acronis et aux disques durs externes 
  pour la préparation des postes
- Élimination du compte administrateur local resté actif après imagerie — 
  fermeture d'une dette de sécurité qui n'était plus tracée
- Rattachement au tenant Entra ID automatisé dès le déballage, sans étape 
  manuelle de renommage ou de reconnexion
- Autopilot propagé à l'ensemble du parc, profils en production
- Configuration adaptée automatiquement au type d'usage, sans intervention 
  manuelle supplémentaire après enrôlement

## Compétences mobilisées

`Windows Autopilot` `Microsoft Intune` `Microsoft Entra ID` `Groupes dynamiques` 
`Profils de déploiement` `Industrialisation du déploiement de parc` 
`Réduction de la dette de sécurité`
