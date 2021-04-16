---
title: Azure Automanage für Windows Server
description: Erfahren Sie mehr über die bewährten Methoden für Azure Automanage für virtuelle Computer für Dienste, für die automatisch das Onboarding und die Konfiguration für Windows Server-Computer durchgeführt wird.
author: deanwe
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 02/22/2021
ms.author: deanwe
ms.openlocfilehash: c1093491c5d4f0c475254e31c2be079d7dd4f5e1
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106278846"
---
# <a name="azure-automanage-for-virtual-machines-best-practices---windows-server"></a>Bewährte Methoden für Azure Automanage für virtuelle Computer: Windows Server

Für diese Azure-Dienste wird automatisch für Sie das Onboarding durchgeführt, wenn Sie Azure Automanage für virtuelle Computer (VMs) auf einem virtuellen Windows Server-Computer verwenden. Sie sind für unser Whitepaper über bewährte Methoden unerlässlich, das Sie in unserem [Cloud Adoption Framework](/azure/cloud-adoption-framework/manage/azure-server-management) finden.

Bei all diesen Diensten werden wir das Onboarding automatisch durchführen, sie automatisch konfigurieren, auf Datendrift überwachen und korrigieren, wenn ein Datendrift festgestellt wird. Weitere Informationen zu diesem Prozess finden Sie unter [Automatische Azure-Verwaltung für virtuelle Computer](automanage-virtual-machines.md).

## <a name="supported-windows-server-versions"></a>Unterstützte Windows Server-Versionen

Automanage unterstützt die folgenden Windows Server-Versionen:

- Windows Server 2012/R2
- Windows Server 2016
- Windows Server 2019
- Windows Server 2019 Azure Edition

## <a name="participating-services"></a>Beteiligte Dienste

|Dienst    |BESCHREIBUNG    |Unterstützte Umgebungen<sup>1</sup>    |Unterstützte Voreinstellungen<sup>1</sup>    |
|-----------|---------------|----------------------|-------------------------|
|[VM Insights-Überwachung](https://docs.microsoft.com/azure/azure-monitor/vm/vminsights-overview)    |Azure Monitor für VMs überwacht die Leistung und Integrität Ihrer virtuellen Computer, einschließlich ihrer ausgeführten Prozesse und Abhängigkeiten von anderen Ressourcen. [Weitere Informationen](../azure-monitor/vm/vminsights-overview.md).    |Bereitstellung    |Nein    |
|[Backup](https://docs.microsoft.com/azure/backup/backup-overview)    |Azure Backup bietet unabhängige und isolierte Sicherungen zum Schutz vor dem versehentlichen Löschen der Daten auf Ihren VMs. [Weitere Informationen](../backup/backup-azure-vms-introduction.md). Die Gebühren basieren auf der Anzahl und Größe der VMs, die geschützt werden. [Weitere Informationen](https://azure.microsoft.com/pricing/details/backup/).    |Bereitstellung    |Ja    |
|[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-introduction)    |Azure Security Center ist ein vereinheitlichtes Sicherheitsverwaltungssystem für Infrastrukturen, mit dem der Sicherheitsstatus Ihrer Rechenzentren gestärkt wird und ein erweiterter Schutz vor Bedrohungen für Ihre Hybridworkloads in der Cloud bereitgestellt wird. [Weitere Informationen](../security-center/security-center-introduction.md).  Die automatische Verwaltung konfiguriert das Abonnement, in dem sich Ihre VM befindet, für das Free-Tarifangebot von Azure Security Center. Wenn Ihr Abonnement bereits mit Azure Security Center verbunden ist, wird es von Automanage nicht neu konfiguriert.    |Produktion, Dev/Test    |Nein    |
|[Microsoft Antimalware](https://docs.microsoft.com/azure/security/fundamentals/antimalware)    |Microsoft Antimalware für Azure ist eine kostenlose Echtzeit-Schutzfunktion zum Erkennen und Entfernen von Viren, Spyware und anderer Schadsoftware. Das Tool generiert Warnungen, wenn bekannte schädliche oder unerwünschte Software versucht, sich selbst auf Ihren Azure-Systemen zu installieren oder dort auszuführen. [Weitere Informationen](../security/fundamentals/antimalware.md). |Produktion, Dev/Test    |Ja    |
|[Updateverwaltung](https://docs.microsoft.com/azure/automation/update-management/overview)    |Sie können die Updateverwaltung in Azure Automation verwenden, um Betriebssystemupdates für Ihre virtuellen Computer zu verwalten. Sie können den Status der verfügbaren Updates auf allen Agent-Computern schnell auswerten und die Installation der für den Server erforderlichen Updates initiieren. [Weitere Informationen](../automation/update-management/overview.md).    |Produktion, Dev/Test    |Nein    |
|[Änderungsnachverfolgung und Bestand](https://docs.microsoft.com/azure/automation/change-tracking/overview) |„Änderungsnachverfolgung und Bestand“ kombiniert Änderungsnachverfolgungs- und Bestandsfunktionen, mit denen Sie Änderungen an virtuellen Computern und Serverinfrastrukturen nachverfolgen können. Da der Dienst auch eine Änderungsnachverfolgung für Dienste, Daemons, Software, Registrierung und Dateien in Ihrer Umgebung unterstützt, können Sie unerwünschte Änderungen ermitteln und entsprechende Warnungen auslösen. Durch die Unterstützung einer Bestandserfassung ist es zudem möglich, Ressourcen auf Gastsystemen abzurufen, um sich über die installierten Anwendungen und andere Konfigurationselemente zu informieren.  [Weitere Informationen](../automation/change-tracking/overview.md).    |Produktion, Dev/Test    |Nein    |
|[Azure-Gastkonfiguration](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration) | Die Gastkonfigurationsrichtlinie dient zur Überwachung der Konfiguration sowie zur Generierung von Berichten zur Konformität des Computers. Der Automanage-Dienst installiert die [Windows-Sicherheitsbaselines](/windows/security/threat-protection/windows-security-baselines) mithilfe der Gastkonfigurationserweiterung. Bei Windows-Computern wendet der Gastkonfigurationsdienst die Baselineeinstellungen automatisch erneut an, wenn sie nicht mehr konform sind. [Weitere Informationen](../governance/policy/concepts/guest-configuration.md).    |Produktion, Dev/Test    |Nein    |
|[Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/boot-diagnostics)    | Die Startdiagnose ist ein Debuggingfeature für Azure-VMs, die eine Diagnose von Fehlern beim Starten von VMs ermöglicht. Mit der Startdiagnose können Benutzer anhand von Informationen des seriellen Protokolls und Screenshots den Zustand ihrer VMs beim Systemstart beobachten. |Produktion, Dev/Test    |Nein    |
|[Azure Automation-Konto](https://docs.microsoft.com/azure/automation/automation-create-standalone-account)    |Azure Automation unterstützt die Verwaltung des gesamten Lebenszyklus Ihrer Infrastruktur und Anwendungen. [Weitere Informationen](../automation/automation-intro.md).    |Produktion, Dev/Test    |Nein    |
|[Log Analytics-Arbeitsbereich](https://docs.microsoft.com/azure/azure-monitor/logs/log-analytics-overview) |Azure Monitor speichert Protokolldaten in einem Log Analytics-Arbeitsbereich, bei dem es sich um eine Azure-Ressource und einen Container handelt, in dem Daten gesammelt und aggregiert werden und der als eine administrative Grenze dient. [Weitere Informationen](../azure-monitor/logs/design-logs-deployment.md).    |Produktion, Dev/Test    |Nein    |


<sup>1</sup> Die Auswahl der Umgebung ist verfügbar, wenn Sie Automanage aktivieren. [Weitere Informationen](automanage-virtual-machines.md#environment-configuration). Sie können auch die Standardeinstellungen der Umgebung anpassen und Ihre eigenen Voreinstellungen im Rahmen der bewährten Methoden festlegen.


## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie, die automatische Verwaltung für virtuelle Computer im Azure-Portal zu aktivieren.

> [!div class="nextstepaction"]
> [Aktivieren der automatischen Verwaltung für virtuelle Computer im Azure-Portal](quick-create-virtual-machines-portal.md)