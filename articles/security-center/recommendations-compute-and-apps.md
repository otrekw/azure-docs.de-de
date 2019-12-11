---
title: Azure Security Center-Empfehlungen für Computer und Apps
description: Sicherheitsempfehlungen von Azure Security Center, die zum Schutz Ihrer virtuellen und physischen Computer sowie Ihrer Web-Apps und App Service-Umgebungen beitragen.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2019
ms.author: memildin
ms.openlocfilehash: c0cf5951daf004a0c805b688f08d1ccfa4679615
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74781902"
---
# <a name="compute-and-app-recommendations---reference-guide"></a>Compute- und App-Empfehlungen – Referenzanleitung

Dieser Artikel enthält eine vollständige Liste der Empfehlungen, die Ihnen möglicherweise in Azure Security Center bezüglich folgenden Ressourcentypen angezeigt werden:

* VMs und Computer
* VM Scale Sets
* Cloud Services
* App Services
* Container
* Computeressourcen

[Hier](security-center-virtual-machine-protection.md) finden Sie eine Erläuterung dazu, wie Sie diese Empfehlungen finden und Probleme beheben.

## Compute- und App-Empfehlungen<a name="compute-and-app-recs"></a>
|Ressourcentyp|Sicherheitsbewertung|Empfehlung|BESCHREIBUNG|
|----|----|----|----|
|App Service|20|Zugriff auf Webanwendung nur über HTTPS gestatten|Beschränken Sie den Zugriff von Webanwendungen nur auf HTTPS.|
|App Service|20|Zugriff auf Funktions-App nur über HTTPS gestatten|Beschränken Sie den Zugriff von Funktionen-Apps nur auf HTTPS.|
|App Service|5|Diagnoseprotokolle in App Services sollten aktiviert sein.|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|App Service|10|Remotedebuggen muss für Webanwendung deaktiviert werden|Deaktivieren Sie das Debuggen für eine Webanwendung, wenn Sie sie nicht mehr benötigen. Für das Remotedebuggen müssen die eingehenden Ports für eine Funktions-App geöffnet sein.|
|App Service|10|Remotedebuggen muss für Funktionsanwendung deaktiviert werden|Deaktivieren Sie das Debuggen für eine Funktionen-App, wenn Sie sie nicht mehr benötigen. Für das Remotedebuggen müssen die eingehenden Ports für eine Funktions-App geöffnet sein.|
|App Service|10|Nicht allen („*“) Ressourcen Zugriff auf Ihre Anwendung erteilen| Lassen Sie nicht zu, dass der Parameter „WEBSITE_LOAD_CERTIFICATES“ auf "" gesetzt wird. Durch die Einstellung des Parameters auf ‘’ werden alle Zertifikate in den persönlichen Zertifikatspeicher Ihrer Webanwendungen geladen. Dies kann zum Missbrauch des Prinzips der „geringsten Rechte“ führen, da es unwahrscheinlich ist, dass die Website Zugriff auf sämtliche Zertifikate zur Laufzeit benötigt.|
|App Service|20|Nicht jeder Ressource den Zugriff auf Ihre Webanwendungen über CORS gestatten|Ermöglichen Sie nur erforderlichen Domänen die Interaktion mit Ihrer Webanwendung. Bei der Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) sollte nicht allen Domänen gestattet werden, auf Ihre Webanwendung zuzugreifen.|
|App Service|20|Nicht jeder Ressource den Zugriff auf Ihre Funktions-App über CORS gestatten| Ermöglichen Sie nur erforderlichen Domänen die Interaktion mit Ihrer Funktionsanwendung. Bei der Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) sollte nicht allen Domänen gestattet werden, auf Ihre Funktionsanwendung zuzugreifen.|
|Computeressourcen (Batch)|1|Metrikwarnungsregeln sollten in Batch-Konten konfiguriert werden.|Konfigurieren Sie Metrikwarnungsregeln für das Batch-Konto, und aktivieren Sie die Metriken „Abgeschlossene Ereignisse zum Löschen von Pools“ und „Startereignisse zum Löschen von Pools“.|
|Computeressourcen (Service Fabric)|10|Service Fabric-Cluster sollten nur Azure Active Directory für die Clientauthentifizierung verwenden.|Führen Sie die Clientauthentifizierung in Service Fabric ausschließlich über Azure Active Directory durch.|
|Computeressourcen (Automation-Konto)|5|Automation-Kontovariablen sollten verschlüsselt werden.|Aktivieren Sie die Verschlüsselung für Variablenobjekte von Automation-Konten, wenn vertrauliche Daten gespeichert werden.|
|Computeressourcen (Suche)|5|Aktivierung von Diagnoseprotokollen für Search-Dienste überwachen|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Service Bus)|5|Diagnoseprotokolle in Service Bus sollten aktiviert sein.|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Stream Analytics)|5|Diagnoseprotokolle in Azure Stream Analytics sollten aktiviert werden.|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Batch)|5|Diagnoseprotokolle in Batch-Konten aktivieren|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Event Hub)|5|Diagnoseprotokolle in Event Hub sollten aktiviert sein.|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Logik-Apps)|5|Diagnoseprotokolle in Logic Apps aktivieren|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn Sie Sicherheitsincidents untersuchen oder Ihr Netzwerk gefährdet ist. |
|Computeressourcen (Service Fabric)|15|Eigenschaft „ClusterProtectionLevel“ in Service Fabric auf „EncryptAndSign“ festlegen (Vorschauversion)|Service Fabric bietet drei Schutzebenen („None“, „Sign“ und „EncryptAndSign“) für die Kommunikation zwischen zwei Knoten unter Verwendung eines primären Clusterzertifikats. Legen Sie die Schutzebene fest, um sicherzustellen, dass alle zwischen zwei Knoten übertragenen Nachrichten verschlüsselt und digital signiert werden. |
|Computeressourcen (Service Bus)|1|Alle Autorisierungsregeln außer RootManageSharedAccessKey aus Service Bus-Namespace entfernen |Service Bus-Clients dürfen keine Zugriffsrichtlinie auf Namespace-Ebene verwenden, die Zugriff auf alle Warteschlangen und Themen in einem Namespace bereitstellt. Um dem Sicherheitsmodell der geringsten Rechte zu entsprechen, müssen Sie Zugriffsrichtlinien auf Entitätsebene erstellen, damit nur der jeweiligen Entität Zugriff auf Warteschlangen und Themen gewährt wird.|
|Computeressourcen (Event Hub)|1|Alle Autorisierungsregeln außer RootManageSharedAccessKey sollten aus dem Event Hub-Namespace entfernt werden.|Event Hub-Clients dürfen keine Zugriffsrichtlinie auf Namespace-Ebene verwenden, die Zugriff auf alle Warteschlangen und Themen in einem Namespace bereitstellt. Um dem Sicherheitsmodell der geringsten Rechte zu entsprechen, müssen Sie Zugriffsrichtlinien auf Entitätsebene erstellen, damit nur der jeweiligen Entität Zugriff auf Warteschlangen und Themen gewährt wird.|
|Computeressourcen (Event Hub)|5|Autorisierungsregeln für die Event Hub-Entität sollten definiert werden.|Überwachen Sie Autorisierungsregeln in der Event Hub-Entität, um die geringstmöglichen Zugriffsberechtigungen zu erteilen.|
|Machine|50|Monitoring Agent auf Ihren Computern installieren|Installieren Sie den Überwachungs-Agent, um die Datensammlung, das Prüfen auf Updates, die Baselineüberprüfung und Endpoint Protection auf jedem Computer zu aktivieren.|
|Machine|50|Automatische Bereitstellung und Datensammlung für Ihre Abonnements aktivieren |Aktivieren Sie die automatische Bereitstellung und Datensammlung für Computer in Ihren Abonnements, um die Datensammlung, das Prüfen auf Updates, die Baselineüberprüfung und Endpoint Protection auf jedem Computer zu aktivieren, der Ihren Abonnements hinzugefügt wird.|
|Machine|40|Monitoring Agent-Integritätsprobleme auf Ihren Computern beheben|Beheben Sie Monitoring Agent-Probleme auf Ihren Computern mithilfe der Anweisungen im Handbuch zur Problembehandlung, um den vollständigen Security Center-Schutz zu erhalten.| 
|Machine|40|Endpoint Protection-Integritätsprobleme auf Ihren Computern beheben|Beheben Sie Monitoring Agent-Probleme auf Ihren Computern mithilfe der Anweisungen im Handbuch zur Problembehandlung, um den vollständigen Security Center-Schutz zu erhalten.|
|Machine|40|Problembehandlung für fehlende Überprüfungsdaten auf Ihren Computern durchführen|Führen Sie eine Problembehandlung für fehlende Überprüfungsdaten auf virtuellen und physischen Computern durch. Fehlende Überprüfungsdaten auf Ihren Computern führen dazu, dass Sicherheitsbewertungen wie das Prüfen auf Updates, die Baselineüberprüfung und die Überprüfung der Endpoint Protection-Lösung nicht durchgeführt werden.|
|Machine|40|Systemupdates sollten auf Ihren Computern installiert sein.|Installieren Sie fehlende Systemsicherheitsupdates und kritische Updates zum Schutz Ihrer virtuellen und physischen Windows- und Linux-Computer.
|Machine|15|Web Application Firewall hinzufügen| Stellen Sie eine Web Application Firewall (WAF) bereit, um Ihre Webanwendungen zu schützen. |
|Machine|40|Betriebssystemversion für Ihre Clouddienstrollen aktualisieren|Aktualisieren Sie die Betriebssystemversion für Ihre Clouddienstrollen auf die aktuelle Version für Ihre Betriebssystemfamilie.|
|Machine|35|Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer sollten beseitigt werden.|Beseitigen Sie Sicherheitsrisiken in der Sicherheitskonfiguration Ihrer Computer, um sie vor Angriffen zu schützen.|
|Machine|35|Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Container beseitigen|Beseitigen Sie Sicherheitsrisiken in der Sicherheitskonfiguration von Computern, auf denen Docker installiert ist, um sie vor Angriffen zu schützen.|
|Machine|25|Adaptive Anwendungssteuerung aktivieren|Aktivieren Sie Anwendungssteuerung, um zu steuern, welche Anwendungen auf Ihren virtuellen Computern in Azure ausgeführt werden können. Dadurch sind Ihre virtuellen Computer besser gegen Schadsoftware abgesichert. Security Center nutzt maschinelles Lernen, um die auf den einzelnen virtuellen Computern ausgeführten Anwendungen zu analysieren, und unterstützt Sie beim Anwenden von Zulassungsregeln, die auf diesen Daten basieren. Dadurch können Sie Zulassungsregeln für Anwendungen einfacher konfigurieren und verwalten.|
|Machine|20|Endpoint Protection-Lösung auf Ihren Computern installieren|Installieren Sie eine Endpoint Protection-Lösung auf Ihren virtuellen Computern, um sie vor Bedrohungen und Sicherheitsrisiken zu schützen.|
|Machine|20|Computer zum Anwenden von Systemupdates neu starten|Starten Sie Ihre Computer neu, um die Systemupdates anzuwenden und Ihre Computer vor Sicherheitsrisiken zu schützen.|
|Machine|15|Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.|Verschlüsseln Sie die Datenträger Ihrer virtuellen Computer mit Azure Disk Encryption für virtuelle Windows- und Linux-Computer. Azure Disk Encryption (ADE) verwendet das BitLocker-Feature (Branchenstandard) von Windows und das DM-Crypt-Feature von Linux, um das Betriebssystem und den Datenträger zu verschlüsseln. Dies trägt zum Schutz Ihrer Daten und zur Erfüllung der Sicherheits- und Complianceanforderungen im Azure-Schlüsseltresor des Kunden bei. Wenn Ihre Compliance- und Sicherheitsauflagen eine End-to-End-Datenverschlüsselung mithilfe von Verschlüsselungsschlüsseln – einschließlich Verschlüsselung der kurzlebigen (lokal angeschlossenen temporären) Festplatte – vorsehen, nutzen Sie Azure Disk Encryption. Alternativ werden verwaltete Datenträger im Ruhezustand standardmäßig mit der Azure Speicherdienstverschlüsselung verschlüsselt. Hierbei werden von Microsoft verwaltete Schlüssel in Azure als Verschlüsselungsschlüssel verwendet. Wenn dieses Szenario Ihren Compliance- und Sicherheitsanforderungen entspricht, können Sie die standardmäßige Verschlüsselung für verwaltete Datenträger nutzen, um Ihre Anforderungen zu erfüllen.|
|Machine|30|Lösung zur Sicherheitsrisikobewertung auf Ihren virtuellen Computern installieren|Lösung zur Sicherheitsrisikobewertung auf Ihren virtuellen Computern installieren|
|Machine|15|Web Application Firewall hinzufügen| Stellen Sie eine Web Application Firewall (WAF) bereit, um Ihre Webanwendungen zu schützen. |
|Machine|30|Sicherheitsrisiken sollten durch eine Lösung zur Sicherheitsrisikobewertung beseitigt werden.|Virtuelle Computer, für die eine Drittanbieterlösung zur Sicherheitsrisikobewertung bereitgestellt ist, werden kontinuierlich auf Schwachstellen in Anwendungen und im Betriebssystem überprüft. Wenn solche Sicherheitsrisiken gefunden werden, stehen diese Informationen im Rahmen der Empfehlung zur Verfügung.|
|Machine|30|Lösung zur Sicherheitsrisikobewertung auf Ihren virtuellen Computern installieren|Lösung zur Sicherheitsrisikobewertung auf Ihren virtuellen Computern installieren|
|Machine|1|Virtuelle Computer sollten zu neuen AzureRM-Ressourcen migriert werden.|Verwenden Sie Azure Resource Manager für Ihre virtuellen Computer, um von den folgenden Sicherheitsverbesserungen zu profitieren: strengere Zugriffssteuerung (RBAC), bessere Überwachung, Resource Manager-basierte Bereitstellung und Governance, Zugriff auf verwaltete Identitäten, Zugriff auf Schlüsseltresor für Geheimnisse, Azure AD-basierte Authentifizierung und Unterstützung für Markierungen und Ressourcengruppen für eine einfachere Sicherheitsverwaltung. |
|Machine|30|Sicherheitsrisiken sollten durch eine Lösung zur Sicherheitsrisikobewertung beseitigt werden.|Virtuelle Computer, für die eine Drittanbieterlösung zur Sicherheitsrisikobewertung bereitgestellt ist, werden kontinuierlich auf Schwachstellen in Anwendungen und im Betriebssystem überprüft. Wenn solche Sicherheitsrisiken gefunden werden, stehen diese Informationen im Rahmen der Empfehlung zur Verfügung.|
|VM-Skalierungsgruppe |4|Diagnoseprotokolle in Virtual Machine Scale Sets sollten aktiviert werden.|Aktivieren Sie Protokolle, und bewahren Sie sie bis zu ein Jahr lang auf. So können Sie Aktivitätsspuren zu Untersuchungszwecken neu erstellen. Dies ist nützlich, wenn ein Sicherheitsvorfall eintritt, oder Ihr Netzwerk kompromittiert ist.|
|VM-Skalierungsgruppe|35|Sicherheitsrisiken in der Sicherheitskonfiguration von VM-Skalierungsgruppen sollten beseitigt werden.|Beseitigen Sie Sicherheitsrisiken in der Sicherheitskonfiguration Ihrer VM-Skalierungsgruppen, um sie vor Angriffen zu schützen. |
|VM-Skalierungsgruppe|5|Endpoint Protection-Integritätsfehler in VM-Skalierungsgruppen beheben|Beheben Sie Endpoint Protection-Integritätsfehler in Ihren VM-Skalierungsgruppen, um sie vor Bedrohungen und Sicherheitsrisiken zu schützen. |
|VM-Skalierungsgruppe|10|Endpoint Protection sollte auf virtuellen Computern installiert sein.|Installieren Sie eine Endpoint Protection-Lösung in Ihren VM-Skalierungsgruppen, um sie vor Bedrohungen und Sicherheitsrisiken zu schützen. |
|VM-Skalierungsgruppe|40|Systemupdates für VM-Skalierungsgruppen sollten installiert werden.|Installieren Sie fehlende Systemsicherheitsupdates und kritische Updates zum Schutz Ihrer Windows- und Linux-VM-Skalierungsgruppen. |
|


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Empfehlungen für andere Arten von Azure-Ressourcen finden Sie in den folgenden Themen:

* [Überwachen von Identität und Zugriff in Azure Security Center](security-center-identity-access.md)
* [Schützen Ihres Netzwerks in Azure Security Center](security-center-network-recommendations.md)
* [Schützen Ihres Azure SQL-Diensts in Azure Security Center](security-center-sql-service-recommendations.md)
