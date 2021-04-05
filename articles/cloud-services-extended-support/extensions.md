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
ms.openlocfilehash: f9029a36dc3b778e139b4553524e8e2ca6b4bbad
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98757168"
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


## <a name="next-steps"></a>Nächste Schritte 
- Überprüfen Sie die [Bereitstellungsvoraussetzungen](deploy-prerequisite.md) für Cloud Services (erweiterter Support).
- Sehen Sie sich die [häufig gestellten Fragen](faq.md) zu Cloud Services (erweiterter Support) an.
- Stellen Sie eine Cloud Service-Instanz (erweiterter Support) über das [Azure-Portal](deploy-portal.md), mit [PowerShell](deploy-powershell.md), einer [Vorlage](deploy-template.md) oder [Visual Studio](deploy-visual-studio.md) bereit.
