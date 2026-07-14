# Gestion dynamique des terminaux avec Microsoft Entra ID

## Contexte

L'environnement comprend plus de 1 190 utilisateurs répartis sur deux
établissements ainsi qu'un parc de près de 800 terminaux administrés via
Microsoft Intune.

Afin d'éviter l'affectation manuelle des applications, profils de
configuration et stratégies de sécurité, une automatisation basée sur
Microsoft Entra ID a été mise en œuvre.

---

## Objectifs

- Réduire les opérations manuelles d'administration
- Standardiser les configurations des équipements
- Simplifier le déploiement des applications
- Industrialiser la gestion du parc informatique
- Faciliter l'administration Microsoft Intune

---

## Architecture

### Technologies utilisées

- Microsoft Entra ID
- Microsoft Intune
- Microsoft 365
- Windows Autopilot
- PowerShell
- Microsoft Graph

---

## Réalisation

Conception et déploiement de 123 groupes dynamiques Microsoft Entra ID.

Les règles d'appartenance reposent notamment sur :

- Les noms des appareils
- Les Group Tags Windows Autopilot
- Les attributs utilisateurs
- Les besoins spécifiques des établissements

Ces groupes sont utilisés pour :

- Les applications Win32
- Les stratégies de sécurité
- Les profils de configuration
- Les politiques de conformité
- Les scripts PowerShell
- Les scripts de remédiation

---

## Exemples d'automatisation

### Pré-déploiement

Affectation automatique des applications et paramètres aux appareils avant leur mise à disposition.

### Post-déploiement

Application automatique des configurations adaptées à l'utilisateur principal après connexion.

### Gestion du cycle de vie

Modification automatisée des Group Tags Windows Autopilot via PowerShell et Microsoft Graph.

---

## Résultats

- 123 groupes dynamiques déployés
- Plus de 780 terminaux administrés
- Réduction significative des affectations manuelles
- Standardisation des configurations
- Simplification des déploiements Intune
- Réduction des risques d'erreur humaine

---

## Compétences démontrées

- Microsoft Entra ID
- Microsoft Intune
- Windows Autopilot
- PowerShell
- Microsoft Graph
- Gestion des identités
- Automatisation
- Endpoint Management
