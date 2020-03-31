---
title: Azure Service Fabric-Versionen
description: Versionshinweise für Azure Service Fabric Informationen zu den neuesten Features und Verbesserungen in Service Fabric
ms.date: 06/10/2019
ms.topic: conceptual
hide_comments: true
hideEdit: true
ms.openlocfilehash: cb4fdd56e9cf67c71ac690d423499929167f8977
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064240"
---
# <a name="service-fabric-releases"></a>Service Fabric-Versionen

| <a href="https://github.com/Azure/Service-Fabric-Troubleshooting-Guides" target="blank">Handbücher zur Problembehandlung</a> 
| <a href="https://github.com/Azure/service-fabric-issues" target="blank">Problemverfolgung</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-support" target="blank">Supportoptionen</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-versions" target="blank">Unterstützte Versionen</a> 
| <a href="https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0" target="blank">Codebeispiele</a>

Dieser Artikel enthält weitere Informationen zu den neuesten Versionen und Updates für die Service Fabric-Runtime und -SDKs.

## <a name="whats-new-in-service-fabric"></a>Neuigkeiten in Service Fabric

### <a name="service-fabric-70"></a>Service Fabric 7.0

Azure Service Fabric 7.0 ist jetzt verfügbar! Sie können über das Azure-Portal oder über eine Azure Resource Manager-Bereitstellung ein Update auf Version 7.0 durchführen. Aufgrund von Kundenfeedback zu Releases am Jahresende beginnen wir mit der automatischen Aktualisierung von Clustern, die für den Empfang automatischer Upgrades eingerichtet wurden, erst im Januar.
Im Januar wird die Standardprozedur für das Rollout fortgesetzt, und Cluster mit aktivierten automatischen Upgrades empfangen das Update auf Version 7.0 automatisch. Wir werden eine weitere Ankündigung bereitstellen, bevor das Rollout beginnt.
Wir aktualisieren auch unsere geplanten Veröffentlichungstermine, um anzugeben, dass diese Richtlinie berücksichtigt wird. Hier finden Sie Updates zu unseren zukünftigen [Releasezeitplänen](https://github.com/Microsoft/service-fabric/#service-fabric-release-schedule).
 
Dies ist die neueste Version von Service Fabric mit wichtigen Features und umfangreichen Verbesserungen.

### <a name="key-announcements"></a>Wichtige Ankündigungen
 - [**KeyVaultReference-Unterstützung für Anwendungsgeheimnisse (Vorschau)** ](https://docs.microsoft.com/azure/service-fabric/service-fabric-keyvault-references): Service Fabric-Anwendungen, für die [verwaltete Identitäten](https://docs.microsoft.com/azure/service-fabric/concepts-managed-identity) aktiviert sind, können nun direkt auf eine Key Vault-Geheimnis-URL als Umgebungsvariable, Anwendungsparameter oder Containerrepository-Anmeldeinformationen verweisen. Service Fabric löst das Geheimnis mithilfe der verwalteten Identität der Anwendung automatisch auf. 
     
- **Verbesserte Upgradesicherheit für zustandslose Dienste**: Um die Verfügbarkeit während eines Anwendungsupgrades zu gewährleisten, haben wir neue Konfigurationen eingeführt, um die [Mindestanzahl von Instanzen für zustandslose Dienste](https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription?view=azure-dotnet) zu definieren, die als verfügbar erachtet werden. Zuvor war dieser Wert für alle Dienste 1 und konnte nicht geändert werden. Mit dieser neuen Sicherheitsüberprüfung pro Dienst können Sie sicherstellen, dass Ihre Dienste bei Anwendungsupgrades, Clusterupgrades und anderen Wartungsarbeiten, die von den Integritäts- und Sicherheitsüberprüfungen von Service Fabric abhängig sind, eine Mindestanzahl von aktiven Instanzen beibehalten.
  
- [**Resourceneinschränkungen für Benutzerdienste**](https://docs.microsoft.com/azure/service-fabric/service-fabric-resource-governance#enforcing-the-resource-limits-for-user-services): Benutzer können Ressourceneinschränkungen für die Benutzerdienste auf einem Knoten einrichten, um Szenarien wie die Erschöpfung von Ressourcen der Systemdienste von Service Fabric zu verhindern. 
  
- [**Sehr hohe Dienstverschiebungskosten**](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-movement-cost) für einen Replikattyp. Replikate mit sehr hohen Verschiebungskosten werden nur verschoben, wenn eine Einschränkungsverletzung im Cluster vorliegt, die nicht auf andere Weise behoben werden kann. Weitere Informationen dazu, wann die Nutzung von „sehr hohen“ Verschiebungskosten angemessen ist, sowie weitere Überlegungen finden Sie in der Dokumentation.
  
-  **Zusätzliche Clustersicherheitsüberprüfungen**: In diesem Release haben wir eine konfigurierbare Quorumsicherheitsüberprüfung für den Seedknoten eingeführt. Auf diese Weise können Sie anpassen, wie viele Seedknoten während des Lebenszyklus von Clustern und in Verwaltungsszenarien verfügbar sein müssen. Vorgänge, die den Cluster unter den konfigurierten Wert bringen würden, werden blockiert. Zurzeit ist der Standardwert immer ein Quorum der Seedknoten. Wenn Sie z.B. 7 Seedknoten verwenden, würde ein Vorgang, der Sie unter 5 Seedknoten bringen würde, standardmäßig blockiert. Mit dieser Änderung können Sie den minimal sicheren Wert auf 6 festlegen, sodass nur ein Seedknoten gleichzeitig ausfallen darf.
   
- Unterstützung für [**Verwaltung des Sicherungs- und Wiederherstellungsdiensts in Service Fabric Explorer**](https://docs.microsoft.com/azure/service-fabric/service-fabric-backuprestoreservice-quickstart-azurecluster) hinzugefügt. Dies ermöglicht die folgenden Aktivitäten direkt aus SFX heraus: Ermitteln des Sicherungs- und Wiederherstellungsdiensts, Erstellen von Sicherungsrichtlinien, Aktivieren automatischer Sicherungen, Erstellen von Ad-hoc-Sicherungen, Auslösen von Wiederherstellungsvorgängen und Durchsuchen vorhandener Sicherungen.

- Ankündigung der Verfügbarkeit von [**ReliableCollectionsMissingTypesTool**](https://github.com/hiadusum/ReliableCollectionsMissingTypesTool): Mit diesem Tool können Sie überprüfen, ob die in zuverlässigen Sammlungen verwendeten Typen bei einem parallelen Anwendungsupgrade aufwärts- und abwärtskompatibel sind. Dadurch können Upgradefehler oder Datenverluste und Datenbeschädigungen aufgrund fehlender oder inkompatibler Typen verhindert werden.

- [**Aktivieren stabiler Lesevorgänge für sekundäre Replikate**](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-configuration#configuration-names-1): Durch stabile Lesevorgänge werden die Rückgaben sekundärer Replikate auf Werte beschränkt, die durch ein Quorum bestätigt wurden.

Dieses Release enthält außerdem viele neue Funktionen und Fehlerbehebungen sowie Unterstützungs-, Zuverlässigkeits- und Leistungsverbesserungen. Die vollständige Liste der Änderungen finden Sie in den [Versionshinweisen](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_70.md).

### <a name="service-fabric-70-releases"></a>Service Fabric 7.0-Releases

| Veröffentlichungsdatum | Release | Weitere Informationen |
|---|---|---|
| 18. November 2019 | [Azure Service Fabric 7.0](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Service-Fabric-7-0-Release/ba-p/1015482)  | [Versionshinweise](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_70.md)|
| 30. Januar 2020 | [Azure Service Fabric 7.0 Refresh Release](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-0-second-refresh-release/ba-p/1137690)  | [Versionshinweise](https://github.com/Azure/service-fabric/blob/master/release_notes/Service-Fabric-70CU2-releasenotes.md)|
| 6\. Februar 2020 | [Azure Service Fabric 7.0 Refresh Release](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-0-third-refresh-release/ba-p/1156508)  | [Versionshinweise](https://github.com/Azure/service-fabric/blob/master/release_notes/Service-Fabric-70CU3-releasenotes.md)|

### <a name="service-fabric-65"></a>Service Fabric 6.5

Dieses Release umfasst Verbesserungen hinsichtlich Unterstützungsmöglichkeiten, Zuverlässigkeit und Leistung sowie neue Features, Fehlerbehebungen und Verbesserungen, um die Cluster- und Anwendungslebenszyklusverwaltung zu vereinfachen.

> [!IMPORTANT]
> Service Fabric 6.5 ist die endgültige Version mit Unterstützung für Service Fabric-Tools in Visual Studio 2015. Kunden wird empfohlen, in Zukunft auf [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) umzusteigen.

Dies sind die Neuigkeiten in Service Fabric 6.5:

- Service Fabric Explorer enthält einen [Image Store-Viewer](service-fabric-visualizing-your-cluster.md#image-store-viewer) zum Überprüfen von Anwendungen, die Sie in den Imagespeicher hochgeladen haben.

- [Patch Orchestration Application (POA)](service-fabric-patch-orchestration-application.md) Version [1.4.0](https://github.com/microsoft/Service-Fabric-POA/releases/tag/v1.4.0) umfasst viele Verbesserungen für die Selbstdiagnose. Kunden von POA wird empfohlen, auf diese Version umzusteigen.

- [EventStore-Dienst ist standardmäßig für Service Fabric 6.5-Cluster aktiviert](service-fabric-visualizing-your-cluster.md#event-store), es sei denn, Sie haben sich abgemeldet.

- [Replikatlebenszyklusereignisse](service-fabric-diagnostics-event-generation-operational.md#replica-events) wurden für zustandsbehaftete Dienste hinzugefügt.

- [Bessere Sichtbarkeit der Status von Startknoten](service-fabric-understand-and-troubleshoot-with-system-health-reports.md#seed-node-status), einschließlich Clusterebenenwarnungen, wenn ein Startknoten (Seedknoten) fehlerhaft ist (*Ausgefallen*, *Entfernt* oder *Unbekannt*).

- Das [Service Fabric Application Disaster Recovery-Tool](https://github.com/Microsoft/Service-Fabric-AppDRTool) ermöglicht, dass zustandsbehaftete Service Fabric-Dienste nach einem Notfall im primären SF-Cluster schnell wiederhergestellt werden können. Daten aus dem primären Cluster werden ständig in der sekundären Standbyanwendung über regelmäßige Sicherung und Wiederherstellung synchronisiert.

- Visual Studio-Unterstützung für [Veröffentlichen von .NET Core-Apps in Linux-basierten Clustern](service-fabric-how-to-publish-linux-app-vs.md).

- [Azure Service Fabric CLI (SFCTL)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) wird für Service Fabric 6.5 (und höhere Versionen) automatisch installiert, wenn Sie ein Upgrade ausführen oder einen neuen Linux-Cluster in Azure erstellen.

- [SFCTL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) wird in MacOS/Linux OneBox-Clustern standardmäßig erstellt.

Weitere Informationen finden Sie in den [Versionshinweisen zu Service Fabric 6.5](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf).

### <a name="service-fabric-65-releases"></a>Service Fabric 6.5-Releases

| Veröffentlichungsdatum | Release | Weitere Informationen |
|---|---|---|
| 11. Juni 2019 | [Azure Service Fabric 6.5](https://blogs.msdn.microsoft.com/azureservicefabric/2019/06/11/azure-service-fabric-6-5-release/)  | [Versionshinweise](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf)|
| 2\. Juli 2019 | [Azure Service Fabric 6.5 Refresh-Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/07/04/azure-service-fabric-6-5-refresh-release/)  | [Versionshinweise](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU1.pdf)  |
| 29. Juli 2019 | [Azure Service Fabric 6.5 Refresh-Release](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Second-Refresh-Release/ba-p/800523)  | [Versionshinweise](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU2.pdf)  |
| 23. August 2019 | [Azure Service Fabric 6.5 Refresh-Release](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Third-Refresh-Release/ba-p/818599)  | [Versionshinweise](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU3.pdf)  |
| 14. Oktober 2019 | [Azure Service Fabric 6.5 Refresh-Release](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Fifth-Refresh-Release/ba-p/913296)  | [Versionshinweise](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU5.md)  |


## <a name="previous-versions"></a>Vorgängerversionen

### <a name="service-fabric-64-releases"></a>Service Fabric 6.4-Releases

| Veröffentlichungsdatum | Release | Weitere Informationen |
|---|---|---|
| 30. November 2018 | [Azure Service Fabric 6.4](https://blogs.msdn.microsoft.com/azureservicefabric/2018/11/30/azure-service-fabric-6-4-release/)  | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2018/12/Service-Fabric-6.4-Release.pdf)|
| 12. Dezember 2018 | [Azure Service Fabric 6.4 Refresh Release jfür Windows-Cluster](https://blogs.msdn.microsoft.com/azureservicefabric/2018/12/12/azure-service-fabric-6-4-refresh-for-windows-clusters/)  | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2018/12/Links.pdf)  |
| 4\. Februar 2019 | [Azure Service Fabric 6.4 Refresh Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/02/04/azure-service-fabric-6-4-refresh-release/) | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2019/02/Service-Fabric-6.4CU3-Release-Notes.pdf) |
| 4\. März 2019 | [Azure Service Fabric 6.4 Refresh Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/03/12/azure-service-fabric-6-4-refresh-release-2/) | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2019/03/Service-Fabric-6.4CU4-Release-Notes.pdf)
| 8\. April 2019 | [Azure Service Fabric 6.4 Refresh Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/04/08/azure-service-fabric-6-4-refresh-release-5/) | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2019/04/Service-Fabric-6.4CU5-ReleaseNotes3.pdf)
| 2\. Mai 2019 | [Azure Service Fabric 6.4 Refresh Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/02/azure-service-fabric-6-4-refresh-release-3/) | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2019/05/Service-Fabric-64CU6-Release-Notes-V2.pdf)
| 28. Mai 2019 | [Azure Service Fabric 6.4 Refresh Release](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/28/azure-service-fabric-6-4-refresh-release-4/) | [Versionshinweise](https://msdnshared.blob.core.windows.net/media/2019/05/Service_Fabric_64CU7_Release_Notes1.pdf)
