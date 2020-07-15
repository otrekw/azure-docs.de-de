---
title: PowerShell-Beispiele
description: Hier finden Sie Azure PowerShell-Beispiele für einige gängige App Service-Szenarien. Hier erfahren Sie, wie Sie Ihre Bereitstellungs- oder Verwaltungsaufgaben für App Service automatisieren.
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.topic: sample
ms.date: 07/07/2020
ms.custom: mvc
ms.openlocfilehash: a1577d42de9a4452467a448a0de5cd5f9575a55f
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86169424"
---
# <a name="powershell-samples-for-azure-app-service"></a>PowerShell-Beispiele für Azure App Service

Die folgende Tabelle enthält Links zu PowerShell-Skripts, die mithilfe von Azure PowerShell erstellt wurden.

| Skript | BESCHREIBUNG |
|-|-|
|**Erstellen einer App**||
| [Erstellen einer App mit Bereitstellung über GitHub](./scripts/powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine App Service-App, die Code aus GitHub bezieht. |
| [Erstellen einer App mit Continuous Deployment über GitHub](./scripts/powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine App Service-App, die dauerhaft Code aus GitHub bereitstellt. |
| [Erstellen einer App und Bereitstellen von Code mit FTP](./scripts/powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App und lädt Dateien aus einem lokalen Verzeichnis per FTP hoch. |
| [Erstellen einer App und Bereitstellen von Code über ein lokales GitHub-Repository](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App und konfiguriert einen Codepush aus einem lokalen Git-Repository. |
| [Erstellen einer App und Bereitstellen von Code in einer Stagingumgebung](./scripts/powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App mit einem Bereitstellungsslot für Änderungen am Stagingcode. |
|  [Erstellen einer App und Verfügbarmachen Ihrer App mit einem privaten Endpunkt](./scripts/powershell-deploy-private-endpoint.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hier wird eine App Service-App mit einem privaten Endpunkt erstellt. |
|**Konfigurieren der App**||
| [Zuordnen einer benutzerdefinierten Domäne zu einer App](./scripts/powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine App Service-App und ordnet ihr einen benutzerdefinierten Domänennamen zu. |
| [Binden eines benutzerdefinierten TLS-/SSL-Zertifikats an eine App](./scripts/powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine App Service-App und bindet das TLS-/SSL-Zertifikat eines benutzerdefinierten Domänennamens an die App. |
|**Skalieren der App**||
| [Manuelles Skalieren einer App](./scripts/powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App und skaliert sie über zwei Instanzen. |
| [Weltweites Skalieren einer App mit einer Hochverfügbarkeitsarchitektur](./scripts/powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt zwei App Service-Apps in zwei verschiedenen geografischen Regionen und macht sie mithilfe von Azure Traffic Manager über einen zentralen Endpunkt verfügbar. |
|**Verbinden der App mit Ressourcen**||
| [Verbinden einer App mit einer SQL-Datenbank](./scripts/powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine App Service-App und eine Datenbank in Azure SQL-Datenbank und fügt dann die Datenbank-Verbindungszeichenfolge den App-Einstellungen hinzu. |
| [Verbinden einer App mit einem Speicherkonto](./scripts/powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine App Service-App und ein Speicherkonto und fügt dann die Speicherverbindungszeichenfolge den App-Einstellungen hinzu. |
|**Sichern und Wiederherstellen einer App**||
| [Sichern einer App](./scripts/powershell-backup-onetime.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App und eine einmalige Sicherung dafür. |
| [Erstellen einer geplanten Sicherung für eine App](./scripts/powershell-backup-scheduled.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App und eine geplante Sicherung dafür. |
| [Löschen einer Sicherung für eine App](./scripts/powershell-backup-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Löscht eine vorhandene Sicherung für eine App. |
| [Restore a web app from a backup](./scripts/powershell-backup-restore.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Wiederherstellen einer Web-App aus einem Backup) | Stellt eine App aus einer zuvor ausgeführten Sicherung wieder her. |
| [Restore a web app from a backup in another subscription](./scripts/powershell-backup-restore-diff-sub.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Wiederherstellen einer Web-App aus einer Sicherung in einem anderen Abonnement) | Stellt eine Web-App aus einer Sicherung in einem anderen Abonnement wieder her. |
|**Überwachen der App**||
| [Überwachen einer Web-App mit Webserverprotokollen](./scripts/powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App, aktiviert die Protokollierung für die App und lädt die Protokolle auf Ihren lokalen Computer herunter. |
| | |
