---
title: Erweiterungen für Cloud Services (erweiterter Support)
description: Erweiterungen für Cloud Services (erweiterter Support)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 84b27bc4ae30e28a1d97fb8e9931602a1eb14504
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2021
ms.locfileid: "114444784"
---
# <a name="extensions-for-cloud-services-extended-support"></a>Erweiterungen für Cloud Services (erweiterter Support)

Erweiterungen sind kleine Anwendungen, die Konfigurations- und Automatisierungstasks für Rollen nach der Bereitstellung bereitstellen. Beispielsweise können Sie eine Remotedesktopverbindung in Ihrer Rolle während der Bereitstellung von Cloud Service (erweiterter Support) mithilfe der Remotedesktoperweiterung aktivieren. 

## <a name="key-vault-extension"></a>Key Vault-Erweiterung

Die Key Vault-VM-Erweiterung ermöglicht die automatische Aktualisierung von Zertifikaten, die in einem Azure Key Vault gespeichert sind. Hierbei überwacht die Erweiterung eine Liste mit beobachteten Zertifikaten, die in Schlüsseltresoren gespeichert sind. Erkennt sie eine Änderung, werden die entsprechenden Zertifikate abgerufen und installiert. Sie ermöglicht auch das regions- bzw. abonnementübergreifende Verweisen auf Zertifikate für den Clouddienst (erweiterter Support).

Weitere Informationen finden Sie unter [Konfigurieren der Schlüsseltresorerweiterung für den Clouddienst (erweiterter Support)](./enable-key-vault-virtual-machine.md).

## <a name="remote-desktop-extension"></a>Remotedesktoperweiterung

Mit Remotedesktop können Sie auf den Desktop einer Rolle zugreifen, die in Azure ausgeführt wird. Mithilfe einer Remotedesktopverbindung können Sie Probleme mit Ihrer Anwendung diagnostizieren und beheben, während diese ausgeführt wird.

Sie können während der Entwicklung eine Remotedesktopverbindung in Ihrer Rolle aktivieren, indem Sie die Remotedesktopmodule in Ihre Dienstdefinition einschließen, oder indem Sie die Remotedesktoperweiterung verwenden. 

Weitere Informationen finden Sie unter [Konfigurieren von Remotedesktop über das Azure-Portal](enable-rdp.md).

## <a name="windows-azure-diagnostics-extension"></a>Microsoft Azure-Diagnoseerweiterung

Sie können wichtige Leistungsmetriken für beliebige Clouddienste überwachen. Für jede Clouddienstrolle werden minimale Daten zur CPU-Nutzung, Netzwerkverwendung und Datenträgerverwendung erfasst. Wenn auf eine Rolle des Clouddiensts die Erweiterung „Microsoft.Azure.Diagnostics“ angewandt wurde, können für diese Rolle zusätzliche Datenpunkte erfasst werden. 

Bei der grundlegenden Überwachung werden alle drei Minuten Leistungsindikatordaten aus den Rolleninstanzen abgefragt und erfasst. Daten aus der grundlegenden Überwachung werden nicht unter Ihrem Speicherkonto gespeichert und verursachen keine zusätzlichen Kosten. 

Bei der erweiterten Überwachung werden in Intervallen von fünf Minuten, einer Stunde und zwölf Stunden zusätzliche Metriken abgefragt und erfasst. Die aggregierten Daten werden unter einem Speicherkonto in Tabellen gespeichert und nach zehn Tagen gelöscht. Das verwendete Speicherkonto wird nach der Rolle konfiguriert. Sie können verschiedene Speicherkonten für verschiedene Rollen verwenden. 

Weitere Informationen finden Sie unter [Anwenden der Microsoft Azure-Diagnoseerweiterung in Cloud Services (erweiterter Support)](enable-wad.md).

## <a name="anti-malware-extension"></a>Anti-Schadsoftware-Erweiterung
Eine Azure-Anwendung oder ein Azure-Dienst kann Microsoft Antimalware für Azure Cloud Services mit PowerShell-Cmdlets aktivieren und konfigurieren. Beachten Sie, dass Microsoft Antimalware im deaktivierten Zustand auf der Cloud Services-Plattform, die auf einem Windows Server 2012 R2 und älter läuft, installiert wird und durch eine Azure-Anwendung aktiviert werden muss. Für Windows Server 2016 und höher ist Windows Defender standardmäßig aktiviert. Daher können diese „cmdlets“ zum Konfigurieren von Antischadsoftware verwendet werden.

Weitere Informationen finden Sie unter [Hinzufügen von Microsoft Antimalware zum Azure-Cloud Service mit erweitertem Support (CS-ES)](../security/fundamentals/antimalware-code-samples.md#add-microsoft-antimalware-to-azure-cloud-service-using-extended-support)

Weitere Informationen zu Azure Antimalware finden Sie [hier](../security/fundamentals/antimalware.md)



## <a name="next-steps"></a>Nächste Schritte 
- Überprüfen Sie die [Bereitstellungsvoraussetzungen](deploy-prerequisite.md) für Cloud Services (erweiterter Support).
- Sehen Sie sich die [häufig gestellten Fragen](faq.yml) zu Cloud Services (erweiterter Support) an.
- Stellen Sie eine Cloud Service-Instanz (erweiterter Support) über das [Azure-Portal](deploy-portal.md), mit [PowerShell](deploy-powershell.md), einer [Vorlage](deploy-template.md) oder [Visual Studio](deploy-visual-studio.md) bereit.
