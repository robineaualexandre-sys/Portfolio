---
title: Gestion dynamique des terminaux avec Microsoft Entra ID
---

# Gestion dynamique des terminaux avec Microsoft Entra ID

## Contexte

L'environnement comprend plus de 1 190 utilisateurs répartis sur deux établissements ainsi qu'un parc de plus de 780 terminaux administrés via Microsoft Intune.

Une fois la convention de nommage des appareils refondue et les premiers filtres dynamiques mis en place (voir [Refonte de l'architecture de gestion Intune](/Portfolio/projets/architecture-intune.html)), l'étape suivante consistait à généraliser les groupes dynamiques Entra ID à l'ensemble du parc, pour que chaque appareil rejoigne automatiquement ses groupes d'affectation dès son intégration, sans intervention manuelle.

---

## Objectifs

- Réduire les opérations manuelles
- Standardiser les configurations
- Simplifier l'administration Intune
- Automatiser l'affectation des ressources

---

## Technologies utilisées

- Microsoft Entra ID
- Microsoft Intune
- Windows Autopilot
- PowerShell
- Microsoft Graph

---

## Réalisation

Conception et déploiement de plus de 120 groupes dynamiques et filtres Intune, en s'appuyant sur la convention de nommage des appareils établie en amont.

Les règles d'appartenance reposent notamment sur :

- Les noms des appareils
- Les attributs utilisateurs
- Le type de jointure Entra ID (`deviceTrustType`)
- Les besoins spécifiques des établissements

**Exemple de règle dynamique de production** :
(device.deviceTrustType -eq "AzureAD") and (device.displayName -startsWith "LP-MR-EXM-")
<figure>
  <img src="{{ site.baseurl }}/assets/images/entra-id/groupes-dynamiques-liste.png" alt="Liste des groupes dynamiques Entra ID">
  <figcaption>Une partie des groupes dynamiques déployés pour la gestion du parc</figcaption>
</figure>

Ces groupes permettent l'affectation automatique :

- Des applications
- Des profils de configuration
- Des stratégies de sécurité
- Des scripts PowerShell
- Des scripts de remédiation

**Automatisation via Microsoft Graph** : certaines corrections de masse (ex. réaffectation du Group Tag Autopilot d'un lot d'appareils après un changement de salle ou d'usage) sont automatisées via le module Microsoft.Graph.DeviceManagement.Enrollment :

- Connexion à Microsoft Graph (`Connect-MgGraph`) avec le scope `DeviceManagementServiceConfig.ReadWrite.All`
- Import d'une liste de numéros de série depuis un fichier CSV
- Récupération des appareils Autopilot existants (`Get-MgDeviceManagementWindowsAutopilotDeviceIdentity`)
- Mise à jour en masse du Group Tag pour chaque appareil correspondant (`Update-MgDeviceManagementWindowsAutopilotDeviceIdentityDeviceProperty`)

Cette base de groupes dynamiques a ensuite servi de fondation à la mise en place d'Autopilot (voir [Windows Autopilot](/Portfolio/projets/autopilot.html)), en particulier pour distinguer les groupes dédiés au provisioning des groupes de production.

---

## Résultats

- Plus de 120 groupes déployés
- Industrialisation de l'administration Intune
- Réduction significative des tâches manuelles
- Standardisation du parc informatique
- Temps moyen de mise en conformité passé d'une demi-journée à 2h

---

## Compétences démontrées

- Microsoft Entra ID
- Intune
- PowerShell
- Microsoft Graph
- Gestion des identités
- Automatisation
