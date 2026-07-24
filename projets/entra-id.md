---
title: Gestion dynamique des terminaux avec Microsoft Entra ID
---

# Gestion dynamique des terminaux avec Microsoft Entra ID

## Contexte

L'environnement comprend plus de 1 190 utilisateurs répartis sur deux établissements ainsi qu'un parc de plus de 780 terminaux administrés via Microsoft Intune.

La gestion manuelle des applications, configurations et stratégies devenait difficile à maintenir à grande échelle : chaque nouvel appareil ou changement de périmètre nécessitait une intervention manuelle sur les groupes d'affectation, avec un risque d'oubli ou d'erreur de ciblage.

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

Conception et déploiement de plus de 120 groupes dynamiques et filtres Intune.

Les règles d'appartenance reposent notamment sur :

- Les noms des appareils
- Les Group Tags Windows Autopilot
- Les attributs utilisateurs
- Les besoins spécifiques des établissements

**Convention de Group Tag Autopilot** : chaque tag encode l'établissement, le type de site/filière et l'usage de la machine (ex. `[Établissement] [Filière] [Salle/Usage]`, comme `"Site A Filière Labo"` ou `"Site B Filière Examen"`). Les groupes dynamiques Entra ID ciblent ensuite ces appareils via une règle du type :
(device.devicePhysicalIds -any (_ -contains "[ZTDId]:[GroupTag]"))
Cette approche permet de router automatiquement chaque appareil vers ses profils, applications et stratégies dès son enrôlement, sans créer de groupe manuel par salle ou par usage.

**Automatisation via Microsoft Graph** : le Group Tag d'un appareil doit parfois être corrigé ou attribué en masse après coup (nouvel arrivage, changement d'affectation d'une salle). Un script PowerShell s'appuyant sur le module Microsoft.Graph.DeviceManagement.Enrollment automatise ce traitement :

- Connexion à Microsoft Graph (`Connect-MgGraph`) avec le scope `DeviceManagementServiceConfig.ReadWrite.All`
- Import d'une liste de numéros de série depuis un fichier CSV
- Récupération des appareils Autopilot existants (`Get-MgDeviceManagementWindowsAutopilotDeviceIdentity`)
- Mise à jour en masse du Group Tag pour chaque appareil correspondant (`Update-MgDeviceManagementWindowsAutopilotDeviceIdentityDeviceProperty`)

Pour l'enrôlement initial des nouveaux postes, le Group Tag est directement assigné sur site au moment de la capture du hardware hash, via le script `Get-WindowsAutopilotInfo` (module communautaire WindowsAutopilotIntune), ce qui garantit que l'appareil rejoint son groupe dynamique dès sa première synchronisation Autopilot.

Ces groupes permettent l'affectation automatique :

- Des applications
- Des profils de configuration
- Des stratégies de sécurité
- Des scripts PowerShell
- Des scripts de remédiation

---

## Difficultés rencontrées

- Décalage entre l'enrôlement initial (Group Tag assigné sur site) et les corrections a posteriori (changement de salle, erreur de saisie), nécessitant un script de correction en masse plutôt qu'une modification manuelle appareil par appareil
- Temps de propagation des règles dynamiques Entra ID, à anticiper lors des tests de mise en conformité

Ce travail a par la suite servi de socle à une refonte plus large de l'architecture de gestion du parc, orientée vers les filtres Intune pour le ciblage des stratégies spécifiques (voir [Refonte de l'architecture de gestion Intune](/Portfolio/projets/architecture-intune.html)).

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
