# Automatisation et scripts de correction PowerShell

## Contexte

La gestion quotidienne d'un parc de plus de 780 terminaux fait remonter des 
dérives de configuration récurrentes (services arrêtés, clés de registre non 
conformes, paramètres d'alimentation incohérents) qui doivent être détectées 
et corrigées automatiquement, sans intervention manuelle poste par poste.

## Objectif

Construire une bibliothèque de scripts de détection/correction exploitables 
via la fonctionnalité Scripts de correction d'Intune, avec une logique de 
sortie standardisée (`Exit 0` = conforme, `Exit 1` = non conforme) permettant 
un suivi centralisé de la conformité du parc.

## Exemples de scripts en production

### Fiabilisation du service de synchronisation horaire (W32Time)
Vérifie que le service Windows Time est présent, démarré et configuré en 
démarrage automatique ; le corrige sinon.

```powershell
# Détection
$service = Get-Service -Name "W32Time" -ErrorAction SilentlyContinue
if (-not $service) { exit 1 }

$startupType = (Get-WmiObject -Class Win32_Service -Filter "Name='W32Time'").StartMode
if ($startupType -ne "Auto" -or $service.Status -ne "Running") { exit 1 }
exit 0
```

Un service de temps mal synchronisé peut provoquer des échecs d'authentification 
Kerberos silencieux — ce script évite d'avoir à diagnostiquer ce genre 
d'incident a posteriori.

### Correction d'une clé de registre RDP
Déploiement de la clé `RedirectionWarningDialogVersion` sous 
`Terminal Services\Client`, pour fiabiliser le comportement de la fenêtre 
d'avertissement de redirection de ressources en session RDP.

```powershell
$path = "HKLM:\Software\Policies\Microsoft\Windows NT\Terminal Services\Client"
$name = "RedirectionWarningDialogVersion"

if (-not (Test-Path $path)) { New-Item -Path $path -Force | Out-Null }
Set-ItemProperty -Path $path -Name $name -Value 1 -Type DWord
```

### Désactivation de l'hibernation
Certains postes conservaient l'hibernation activée par défaut, consommant de 
l'espace disque inutilement (fichier `hiberfil.sys`). Script de correction 
via `powercfg`, avec vérification de l'état réel après exécution plutôt que 
de supposer la réussite de la commande.

```powershell
powercfg -h off
$hibernationState = powercfg -a | Select-String "Hibernation"
if ($hibernationState -match "not available") { exit 0 } else { exit 1 }
```

### Désactivation ciblée de l'intégrité de la mémoire (HVCI)
Sur un sous-ensemble de postes présentant des incompatibilités matérielles ou 
logicielles avec l'intégrité de la mémoire basée sur la virtualisation, 
correction de la clé `HypervisorEnforcedCodeIntegrity` pour désactiver la 
fonctionnalité de façon contrôlée et détectable, plutôt que de laisser une 
configuration divergente non tracée.

```powershell
$registryPath = "HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard\Scenarios\HypervisorEnforcedCodeIntegrity"
$currentValue = Get-ItemProperty -Path $registryPath | Select-Object -ExpandProperty "Enabled"
if ($currentValue -ne 0) {
    Set-ItemProperty -Path $registryPath -Name "Enabled" -Value 0
}
```

## Évolutions et arbitrages

Deux automatisations initialement envisagées via script ont finalement été 
remplacées par des solutions plus pérennes :

- **Nettoyage des profils utilisateurs inactifs** : géré au départ par script 
  ponctuel, cette tâche a été reprise par une stratégie Intune native, plus fiable et ne 
  nécessitant plus de suivi manuel du script.
- **Synchronisation OneDrive sur postes partagés** : un script de 
  contournement était en place avant ma prise en poste, pour compenser une 
  configuration incomplète de la stratégie Mode PC partagé. Plutôt que de 
  maintenir ce correctif, la stratégie native a été corrigée directement 
  (activation du Mode PC partagé avec prise en charge de la synchronisation 
  OneDrive), permettant de retirer le script devenu inutile.

## Résultats

- Quatre scripts de correction actifs en production, couvrant services, 
  registre et paramètres d'alimentation
- Logique de détection/correction standardisée et testée sur l'ensemble du parc
- Deux automatisations "de contournement" supprimées au profit de correctifs 
  natifs plus robustes, réduisant la dette technique plutôt que de l'accumuler

## Compétences mobilisées

`PowerShell` `Microsoft Intune` `Scripts de correction (detect/remediate)` 
`Registre Windows` `Gestion des services Windows` `Diagnostic et résolution 
de dette technique`
