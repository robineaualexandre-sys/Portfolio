---
title: Automatisation et scripts de correction PowerShell | Portfolio
---

{% include site-style.html %}


# Automatisation et scripts de correction PowerShell

## Contexte

La gestion quotidienne d'un parc de plus de 780 terminaux fait remonter des dérives de configuration récurrentes (services arrêtés, clés de registre non conformes, paramètres d'alimentation incohérents) qui doivent être détectées et corrigées automatiquement, sans intervention manuelle poste par poste.

---

## Objectifs

- Détecter et corriger automatiquement les dérives de configuration
- Standardiser la logique de sortie des scripts pour un suivi centralisé
- Réduire la dette technique plutôt que l'accumuler

---

## Technologies utilisées

- PowerShell
- Microsoft Intune (Scripts de correction)
- Registre Windows

---

## Réalisation

Quatre scripts de correction sont actuellement en production, avec une logique standardisée (`Exit 0` = conforme, `Exit 1` = non conforme) :

Un script fiabilise le service de synchronisation horaire W32Time, en vérifiant sa présence, son état de démarrage et son mode automatique, pour éviter les échecs d'authentification Kerberos silencieux liés à une désynchronisation.

Un script corrige une clé de registre RDP (`RedirectionWarningDialogVersion` sous Terminal Services\Client), pour fiabiliser le comportement de la fenêtre d'avertissement de redirection de ressources en session RDP.

Un script désactive l'hibernation sur les postes concernés via `powercfg`, avec vérification de l'état réel après exécution plutôt que de supposer la réussite de la commande.

Un script désactive de façon contrôlée l'intégrité de la mémoire (HVCI) sur les postes où certains pilotes et logiciels pédagogiques anciens présentent des incompatibilités avec cette protection, en corrigeant la clé 
`HypervisorEnforcedCodeIntegrity`. Le script cible uniquement les postes concernés par ces incompatibilités, plutôt que de désactiver la fonctionnalité de façon générale sur le parc.

Deux automatisations envisagées initialement via script ont depuis été remplacées par des solutions plus pérennes : le nettoyage des profils utilisateurs inactifs est désormais géré par une stratégie Intune native (Mode PC partagé), et un script de contournement pour la synchronisation OneDrive sur postes partagés — en place avant ma prise de poste — a été retiré après correction directe de la stratégie native concernée (Mode PC partagé avec prise en charge OneDrive).

---

## Résultats

- Quatre scripts de correction actifs en production, couvrant services, registre et paramètres d'alimentation
- Logique de détection/correction standardisée sur l'ensemble du parc
- Deux automatisations de contournement supprimées au profit de correctifs natifs plus robustes

---

## Compétences démontrées

- PowerShell
- Microsoft Intune
- Scripts de correction (detect/remediate)
- Registre Windows
- Diagnostic et résolution de dette technique
