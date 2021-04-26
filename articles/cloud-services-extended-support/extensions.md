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
ms.openlocfilehash: 6db43acda679c6c1c1edd6336f693cc4757b6d45
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106220916"
---
# <a name="extensions-for-cloud-services-extended-support"></a>Erweiterungen für Cloud Services (erweiterter Support)

Erweiterungen sind kleine Anwendungen, die Konfigurations- und Automatisierungstasks für Rollen nach der Bereitstellung bereitstellen. Beispielsweise können Sie eine Remotedesktopverbindung in Ihrer Rolle während der Bereitstellung von Cloud Service (erweiterter Support) mithilfe der Remotedesktoperweiterung aktivieren.  

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

Weitere Informationen finden Sie unter [Hinzufügen von Microsoft Antimalware zum Azure-Cloud Service mit erweitertem Support (CS-ES)](https://docs.microsoft.com/azure/security/fundamentals/antimalware-code-samples#add-microsoft-antimalware-to-azure-cloud-service-using-extended-support)

Weitere Informationen zu Azure Antimalware finden Sie [hier](https://docs.microsoft.com/azure/security/fundamentals/antimalware)



## <a name="next-steps"></a>Nächste Schritte 
- Überprüfen Sie die [Bereitstellungsvoraussetzungen](deploy-prerequisite.md) für Cloud Services (erweiterter Support).
- Sehen Sie sich die [häufig gestellten Fragen](faq.md) zu Cloud Services (erweiterter Support) an.
- Stellen Sie eine Cloud Service-Instanz (erweiterter Support) über das [Azure-Portal](deploy-portal.md), mit [PowerShell](deploy-powershell.md), einer [Vorlage](deploy-template.md) oder [Visual Studio](deploy-visual-studio.md) bereit.
