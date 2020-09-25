---
title: Bedrohungsschutz in Azure Security Center
description: In diesem Thema werden die Ressourcen beschrieben, die von den Bedrohungsschutzfeatures von Azure Security Center geschützt werden.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: memildin
ms.openlocfilehash: e2f72cc0ea6851caaf5d0db2f17f8e16473d420e
ms.sourcegitcommit: 0194a29a960e3615f96a2d9d8a7e681cf3e8f9ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89667546"
---
# <a name="threat-protection-in-azure-security-center"></a>Bedrohungsschutz in Azure Security Center

Wenn Security Center in einem der Bereiche Ihrer Umgebung eine Bedrohung erkennt, wird eine Warnung generiert. Diese Warnungen beschreiben Details zu den betroffenen Ressourcen, empfohlene Problembehandlungsschritte sowie in einigen Fällen eine Option, mit der eine Logik-App als Reaktion ausgelöst werden kann.

Der Bedrohungsschutz von Azure Security Center bietet umfassende Schutzmaßnahmen für Ihre Umgebung:

* **Bedrohungsschutz für Azure-Computeressourcen**: Windows-Computer, Linux-Computer, Azure App Service und Azure-Container

* **Bedrohungsschutz für Azure-Datenressourcen**: SQL-Datenbank und Azure Synapse Analytics (früher SQL Data Warehouse), Azure Storage und Azure Cosmos DB

* **Bedrohungsschutz für Azure-Dienstebenen**: Azure-Netzwerkebene, Azure-Verwaltungsebene (Azure Resource Manager (Vorschau)) und Azure Key Vault (Vorschau)

Sie können Warnungen exportieren, und zwar unabhängig davon, ob sie von Security Center generiert oder von Security Center über ein anderes Sicherheitsprodukt empfangen wurden. Befolgen Sie die Anweisungen unter [Exportieren von Sicherheitswarnungen und -empfehlungen (Vorschau)](continuous-export.md), um die Warnungen zu Azure Sentinel (oder ein Drittanbieter-SIEM) bzw. ein beliebiges anderes externes Tool zu exportieren. 

> [!NOTE]
> Warnungen aus unterschiedlichen Quellen erfordern möglicherweise unterschiedlich lange Zeit, bis sie angezeigt werden. Beispielsweise kann es länger dauern, bis Warnungen, die eine Analyse des Netzwerkdatenverkehrs erfordern, angezeigt werden, als das Anzeigen von Warnungen im Zusammenhang mit verdächtigen Prozessen auf virtuellen Computern.

> [!TIP]
> Wenn Sie die Security Center-Features zum Schutz vor Bedrohungen aktivieren möchten, müssen Sie den Tarif „Standard“ dem Abonnement mit den entsprechenden Workloads zuweisen.
>
> Sie können den Schutz vor Bedrohungen für **Azure Storage-Konten** entweder auf Abonnement- oder Ressourcenebene aktivieren.
> Sie können den Schutz vor Bedrohungen für **SQL-Server von Azure SQL-Datenbank** entweder auf Abonnement- oder Ressourcenebene aktivieren.
> Sie können den Schutz vor Bedrohungen für **Azure Database for MariaDB/MySQL/PostgreSQL** nur auf der Ressourcenebene aktivieren.



## <a name="threat-protection-for-windows-machines"></a>Bedrohungsschutz für Windows-Computer <a name="windows-machines"></a>

Azure Security Center wird zur Überwachung und zum Schutz Ihrer Windows-basierten Computer in Azure-Dienste integriert. In Security Center werden die Warnungen und Behandlungsvorschläge aus allen diesen Diensten in einem benutzerfreundlichen Format präsentiert.

* **Microsoft Defender Advanced Threat Protection (ATP)** <a name="windows-atp"></a> – Security Center erweitert seine Plattformen für den Cloudworkloadschutz durch die Integration in Microsoft Defender Advanced Threat Protection (ATP). Dadurch stehen umfassende EDR-Funktionen (Endpoint Detection and Response; Endpunkterkennung und -reaktion) zur Verfügung.

    > [!IMPORTANT]
    > Der Microsoft Defender ATP-Sensor wird automatisch auf Windows-Servern aktiviert, die Security Center verwenden.

    Wenn von Microsoft Defender ATP eine Bedrohung erkannt wird, wird eine Warnung ausgelöst. Die Warnung wird auf dem Security Center-Dashboard angezeigt. Über das Dashboard können Sie zur Microsoft Defender ATP-Konsole wechseln und eine detaillierte Untersuchung durchführen, um das Ausmaß des Angriffs zu ermitteln. Weitere Informationen zu Microsoft Defender ATP finden Sie unter [Integrieren von Servern in den Microsoft Defender ATP-Dienst](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints).

* **Erkennung dateiloser Angriffe** <a name="windows-fileless"></a>: Bei dateilosen Angriffen werden schädliche Nutzlasten in den Arbeitsspeicher injiziert, um die Erkennung durch Verfahren zur datenträgerbasierten Überprüfung zu verhindern. Die Nutzlast des Angreifers nistet sich im Arbeitsspeicher von kompromittierten Prozessen ein und führt ein breites Spektrum an schädlichen Aktivitäten aus.

    Die Erkennung dateiloser Angriffe erkennt dank automatisierter forensischer Techniken für den Arbeitsspeicher Toolkits, Techniken und Verhaltensweisen im Zusammenhang mit dateilosen Angriffen. Diese Lösung überprüft zur Laufzeit in regelmäßigen Abständen Ihren Computer und extrahiert Erkenntnisse direkt aus dem Arbeitsspeicher von Prozessen. Zu den spezifischen Erkenntnissen für Linux zählt die Ermittlung der folgenden Elemente: 

    - Bekannte Toolkits und Kryptografieminingsoftware 
    - Shellcode, ein kurzer Codeabschnitt, der normalerweise als Nutzlast bei der Ausnutzung eines Softwaresicherheitsrisikos verwendet wird
    - Injektion bösartiger ausführbarer Dateien in den Prozessspeicher

    Bei der Erkennung dateiloser Angriffe werden ausführliche Sicherheitswarnungen generiert, die Beschreibungen mit zusätzlichen Prozessmetadaten (etwa zur Netzwerkaktivität) enthalten. Dadurch werden die Warnungsselektion und die Korrelation beschleunigt und die Downstream-Reaktionszeit verkürzt. Dieser Ansatz dient zur Ergänzung ereignisbasierter EDR-Lösungen und ermöglicht ein größeres Erkennungsspektrum.

    Details der Warnungen bei Erkennung dateiloser Angriffe finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-windows).

> [!TIP]
> Sie können Windows-Warnungen simulieren, indem Sie [dieses Sicherheitswarnungen](https://gallery.technet.microsoft.com/Azure-Security-Center-f621a046).






## <a name="threat-protection-for-linux-machines"></a>Bedrohungsschutz für Linux-Computer <a name="linux-machines"></a>

Security Center sammelt Überwachungsdatensätze von Linux-Computern mithilfe von **auditd** – einem der am häufigsten verwendeten Linux-Überwachungsframeworks. auditd befindet sich im Mainline-Kernel. 

* **Integration von Linux-auditd-Warnungen mit dem Log Analytics-Agent:** <a name="linux-auditd"></a> Beim auditd-System handelt es sich um ein Subsystem auf Kernelebene, das zur Überwachung von Systemaufrufen dient. Es filtert sie nach einem angegebenen Regelsatz und schreibt entsprechende Meldungen in einen Socket. Security Center integriert Funktionen aus dem auditd-Paket im Log Analytics-Agent. Diese Integration ermöglicht das Sammeln von auditd-Ereignissen in allen unterstützten Linux-Distributionen ohne weitere Bedingungen.

    auditd-Datensätze werden gesammelt, angereichert und mithilfe des Log Analytics-Agents für Linux zu Ereignissen aggregiert. In Security Center werden ständig neue Analysen hinzugefügt, die Linux-Signale verwenden, um schädliches Verhalten auf cloudbasierten und lokalen Linux-Computern zu erkennen. Diese Analysen umfassen ähnlich wie bei Windows-Funktionen verdächtige Prozesse und Anmeldeversuche, das Laden von Kernmodulen sowie andere Aktivitäten. Diese Aktivitäten können darauf hindeuten, dass ein Computer angegriffen wird oder kompromittiert wurde.  

    Eine Liste der Linux-Warnungen finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-linux).

> [!TIP]
> Sie können Linux-Warnungen simulieren, indem Sie das [ Azure Security Center-Playbook: Linux Detections (Linux-Erkennungsfunktionen)](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef) herunterladen.





## <a name="threat-protection-for-azure-app-service"></a>Bedrohungsschutz für Azure App Service <a name="app-services"></a>

> [!NOTE]
> Dieser Dienst ist derzeit nicht in Azure Government- und Sovereign Cloud-Regionen verfügbar.

Security Center nutzt die Kapazitäten der Cloud, um Angriffe auf Anwendungen unter App Service zu identifizieren. Angreifer testen Webanwendungen, um Schwachstellen zu suchen und auszunutzen. Bevor Anforderungen für in Azure ausgeführte Anwendungen an bestimmte Umgebungen weitergeleitet werden, durchlaufen sie mehrere Gateways, wo sie jeweils überprüft und protokolliert werden. Diese Daten werden dann verwendet, um Exploits und Angreifer zu identifizieren sowie um neue Muster zu erkennen, die später zur Anwendung kommen.

Dank der Möglichkeiten, über die Azure als Cloudanbieter verfügt, kann Security Center interne App Service-Protokolle analysieren und Angriffsmethoden für mehrere Ziele identifizieren. Die Methodik umfasst beispielsweise großflächige Scans und verteilte Angriffe. Bei dieser Art von Angriff, der in der Regel von einer kleinen Untergruppe von IP-Adressen ausgeht, werden ähnliche Endpunkte auf mehreren Hosts durchforstet. Die Angriffe dienen dazu, anfällige Seiten oder Plug-Ins ausfindig zu machen, und sind aus der Perspektive eines einzelnen Hosts nicht erkennbar.

Wenn Sie einen Windows-basierten App Service-Plan ausführen, kann Security Center auch auf die zugrunde liegenden Sandboxes und VMs zugreifen. In Kombination mit den oben aufgeführten Protokolldaten kann die Infrastruktur das gesamte Spektrum erfassen – von neu in Umlauf gebrachten Angriffen bis hin zu kompromittierten Kundencomputern. Auch wenn Security Center bereitgestellt wird, nachdem eine Web-App ausgenutzt wurde, können fortlaufende Angriffe möglicherweise erkannt werden.

Eine Liste der Azure App Service-Warnungen finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-azureappserv).

Weitere Informationen zu App Service-Plänen finden Sie unter [App Service-Pläne](https://azure.microsoft.com/pricing/details/app-service/plans/).





## <a name="threat-protection-for-containers"></a>Bedrohungsschutz für Container <a name="azure-containers"></a>

### <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemein verfügbar|
|Preise:|Standard-Tarif|
|Erforderliche Rollen und Berechtigungen:|**Sicherheitsadministrator** kann Warnungen verwerfen.<br>Der **Sicherheitsleseberechtigte** kann Ergebnisse anzeigen.|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Nein](./media/icons/no-icon.png) National/Sovereign (US Gov, China Gov, andere Gov)|
|||

[!INCLUDE [AKS in ASC threat protection](../../includes/security-center-azure-kubernetes-threat-protection.md)]








## <a name="threat-protection-for-sql-database-and-azure-synapse-analytics-formerly-sql-data-warehouse"></a>Threat Protection für SQL-Datenbank und Azure Synapse Analytics (früher SQL Data Warehouse) <a name="data-sql"></a>

Advanced Threat Protection für Azure SQL-Datenbank erkennt Anomalien bei Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Ihre Datenbanken zuzugreifen oder diese zu nutzen.

Es werden Warnungen angezeigt, wenn verdächtige Datenbankaktivitäten, potenzielle Sicherheitsrisiken oder Angriffe durch Einschleusung von SQL-Befehlen sowie ungewöhnliche Datenbankzugriffs- und -abfragemuster vorliegen.

Advanced Threat Protection für Azure SQL-Datenbank und SQL ist Teil des einheitlichen Pakets [Advanced Data Security (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security) für erweiterte SQL-Sicherheitsfunktionen, das Azure SQL-Datenbank, Azure SQL Managed Instance, Azure Synapse Analytics-Datenbanken (früher SQL Data Warehouse) und SQL Server-Instanzen in Azure Virtual Machines abdeckt.

Weitere Informationen finden Sie unter

* [Aktivieren von Advanced Threat Protection für Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)
* [Aktivieren von Advanced Threat Protection für SQL Server-Instanzen auf Azure Virtual Machines](security-center-iaas-advanced-data.md)
* [Liste der Threat Protection-Warnungen für SQL-Datenbank und Azure Synapse Analytics (früher SQL Data Warehouse)](alerts-reference.md#alerts-sql-db-and-warehouse)



## <a name="threat-protection-for-azure-storage"></a>Bedrohungsschutz für Azure Storage <a name="azure-storage"></a>

### <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|[Blob Storage](https://azure.microsoft.com/services/storage/blobs/) (allgemeine Verfügbarkeit)<br>[Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) (Vorschau)<br>[Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction) (Vorschau)|
|Preise:|Standard-Tarif|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Ja](./media/icons/yes-icon.png) US Gov<br>![Nein](./media/icons/no-icon.png) China Gov/andere Gov-Clouds|
|||


### <a name="whats-protected"></a>Was wird geschützt?

Der Bedrohungsschutz für Azure Storage erkennt potenziell schädliche Aktivitäten in Ihren Azure Storage-Konten. Ihre Daten können geschützt werden, und zwar unabhängig davon, ob Sie als Blobcontainer, Dateifreigaben oder Data Lakes gespeichert werden.

Dank dieser Schutzebene können Sie auch *ohne* fundierte Sicherheitskenntnisse etwas gegen Bedrohungen unternehmen und Ihre Systeme für die Sicherheitsüberwachung verwalten.

### <a name="what-kind-of-alerts-does-threat-protection-for-azure-storage-provide"></a>Welche Art von Warnungen bietet Bedrohungsschutz für Azure Storage?

Sicherheitswarnungen werden ausgelöst, wenn Folgendes vorliegt:

- **Verdächtige Aktivitäten**: Auf das Speicherkonto wurde z. B. erfolgreich über eine IP-Adresse zugegriffen, bei der es sich um einen bekannten aktiven Exitknoten von Tor handelt.
- **Anormales Verhalten**: Beispielsweise Änderungen des Zugriffsmusters für ein Speicherkonto.
- **Upload potenzieller Malware**: Hashzuverlässigkeitsanalyse zeigt an, dass eine hochgeladene Datei Schadsoftware enthält.

Warnungen enthalten Details zum Incident, durch den sie ausgelöst wurden, sowie Empfehlungen zur Untersuchung und Eindämmung von Bedrohungen.

### <a name="what-is-hash-reputation-analysis-for-malware"></a>Was ist Hashzuverlässigkeitsanalyse für Schadsoftware?

Um zu ermitteln, ob eine hochgeladene Datei verdächtig ist, verwendet der Bedrohungsschutz für Azure Storage von [Microsoft Threat Intelligence](https://go.microsoft.com/fwlink/?linkid=2128684) unterstützte Hashzuverlässigkeitsanalyse. Die Tools zum Schutz vor Bedrohungen überprüfen die hochgeladenen Dateien nicht, sondern überprüfen die Speicherprotokolle und vergleichen die Hashes von neu hochgeladenen Dateien mit denjenigen bekannter Viren, Trojaner, Spyware und Ransomware. 

Wenn der Verdacht besteht, dass eine Datei Schadsoftware enthält, zeigt Security Center eine Warnung an und kann optional den Speicherbesitzer per E-Mail um Genehmigung bitten, die verdächtige Datei zu löschen. Um diese automatische Entfernung von Dateien festzulegen, die laut Hashzuverlässigkeitsanalyse Schadsoftware enthalten, stellen Sie [Workflowautomatisierung bereit, die bei Warnungen ausgelöst wird, die die Angabe „Upload potenzieller Schadsoftware in ein Speicherkonto“ enthalten](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-respond-to-potential-malware-uploaded-to-azure-storage/ba-p/1452005).



### <a name="next-steps"></a>Nächste Schritte 

Preisdetails, einschließlich einer kostenlosen 30-Tage-Testversion, finden Sie auf der [Seite mit der Preisübersicht zu Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/).

Weitere Informationen finden Sie unter

* [Aktivieren von Advanced Threat Protection für Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection)
* [Liste der Bedrohungsschutzwarnungen für Azure Storage](alerts-reference.md#alerts-azurestorage)
* [Microsoft Threat Intelligence-Funktionen](https://go.microsoft.com/fwlink/?linkid=2128684)

> [!TIP]
> Sie können Speicherwarnungen simulieren, indem Sie die Anweisungen in [diesem Blogbeitrag](https://techcommunity.microsoft.com/t5/azure-security-center/validating-atp-for-azure-storage-detections-in-azure-security/ba-p/1068131) befolgen.







## <a name="threat-protection-for-azure-cosmos-db-preview"></a>Threat Protection für Azure Cosmos DB (Vorschau) <a name="cosmos-db"></a>

Die Azure Cosmos DB-Warnungen werden bei ungewöhnlichen und potenziell schädlichen Zugriffsversuchen oder Exploit-Vorgängen für Azure Cosmos DB-Konten generiert.

Weitere Informationen finden Sie unter

* [Advanced Threat Protection für Azure Cosmos DB (Vorschau)](../cosmos-db/cosmos-db-advanced-threat-protection.md)
* [Liste der Bedrohungsschutzwarnungen für Azure Cosmos DB (Vorschau)](alerts-reference.md#alerts-azurecosmos)




## <a name="threat-protection-for-azure-network-layer"></a>Bedrohungsschutz für die Azure-Netzwerkebene <a name="network-layer"></a>

Die Netzwerkebenenanalysen von Security Center basieren auf exemplarischen [IPFIX-Daten](https://en.wikipedia.org/wiki/IP_Flow_Information_Export) (durch Azure-Kernrouter erfasste Paketheader). Auf der Grundlage dieses Datenfeeds werden schädliche Datenverkehrsaktivitäten von Security Center durch Machine Learning-Modelle identifiziert und gekennzeichnet. Security Center nutzt zudem die Threat Intelligence-Datenbank von Microsoft zur Anreicherung von IP-Adressen.

Einige Netzwerkkonfigurationen schränken Security Center möglicherweise in der Erstellung von Warnungen bei verdächtiger Netzwerkaktivität ein. Damit Security Center Netzwerkwarnungen generiert, stellen Sie Folgendes sicher:

- Ihr virtueller Computer verfügt über eine öffentliche IP-Adresse (oder befindet sich auf einem Load Balancer mit einer öffentlichen IP-Adresse).

- Der ausgehende Netzwerkdatenverkehr des virtuellen Computers wird nicht durch eine externe IDS-Lösung blockiert.

- Ihrem virtuellen Computer wurde für die gesamte Stunde, in der die verdächtige Kommunikation stattgefunden hat, dieselbe IP-Adresse zugewiesen. Dies gilt auch für als Teil eines verwalteten Diensts (z. B. AKS, Databricks) erstellte virtuelle Computer.

Eine Liste der Warnungen der Azure-Netzwerkebene finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-azurenetlayer).




## <a name="threat-protection-for-azure-management-layer-azure-resource-manager-preview"></a>Bedrohungsschutz für die Azure-Verwaltungsebene (Azure Resource Manager) (Vorschau) <a name ="management-layer"></a>

Die auf Azure Resource Manager basierende Sicherheitsstufe von Security Center befindet sich derzeit in der Vorschauphase.

Security Center nutzt Azure Resource Manager-Ereignisse (also gewissermaßen die Steuerungsebene von Azure), um eine zusätzliche Schutzebene zu bieten. Durch die Analyse der Azure Resource Manager-Datensätze erkennt Security Center ungewöhnliche bzw. potenziell schädliche Vorgänge in der Azure-Abonnementumgebung.

Eine Liste der Warnungen von Azure Resource Manager (Vorschau) finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-azureresourceman).



>[!NOTE]
> Einige der oben aufgeführten Analysen basieren auf Microsoft Cloud App Security. Um von diesen Analysen zu profitieren, müssen Sie eine Cloud App Security-Lizenz aktivieren. Wenn Sie über eine Cloud App Security-Lizenz verfügen, sind diese Warnungen standardmäßig aktiviert. So deaktivieren Sie die Warnungen:
>
> 1. Wählen Sie im Menü von Security Center **Preise und Einstellungen** aus.
> 1. Wählen Sie das Abonnement aus, das Sie ändern möchten.
> 1. Wählen Sie **Bedrohungserkennung** aus.
> 1. Deaktivieren Sie **Microsoft Cloud App Security Zugriff auf meine Daten erteilen**, und wählen Sie anschließend **Speichern** aus.

>[!NOTE]
>Security Center speichert sicherheitsbezogene Kundendaten im gleichen geografischen Raum, in dem sich auch die Ressource befindet. Falls Security Center von Microsoft noch nicht im geografischen Raum der Ressource bereitgestellt wurde, werden die Daten in den USA gespeichert. Wenn Cloud App Security aktiviert ist, werden diese Informationen gemäß den Regeln für geografische Standorte von Cloud App Security gespeichert. Weitere Informationen finden Sie unter [Data storage for non-regional services](https://azuredatacentermap.azurewebsites.net/) (Datenspeicherung für nicht regionale Dienste).

1. Legen Sie den Arbeitsbereich fest, für den Sie den Agent installieren. Stellen Sie sicher, dass sich der Arbeitsbereich im gleichen Abonnement befindet, das Sie in Security Center verwenden, und dass Sie über Lese-/Schreibberechtigungen für den Arbeitsbereich verfügen.

1. Legen Sie den Standardtarif fest, und klicken Sie auf **Speichern**.






## <a name="threat-protection-for-azure-key-vault-preview"></a>Bedrohungsschutz für Azure Key Vault (Vorschau) <a name="azure-keyvault"></a>

> [!NOTE]
> Dieser Dienst ist derzeit nicht in Azure Government- und Sovereign Cloud-Regionen verfügbar.

Der Azure Key Vault-Clouddienst schützt Verschlüsselungsschlüssel und Geheimnisse (wie Zertifikate, Verbindungszeichenfolgen und Kennwörter). 

Azure Security Center enthält den nativen Azure-Dienst Advanced Threat Protection für Azure Key Vault und bietet eine zusätzliche Ebene der Sicherheitsintelligenz. Security Center erkennt ungewöhnliche und potenziell schädliche Versuche, auf Key Vault-Konten zuzugreifen oder diese missbräuchlich zu nutzen. Aufgrund dieser Schutzebene können Sie Bedrohungen begegnen, ohne dass Sie ein Sicherheitsexperte sein oder Systeme von Drittanbietern für die Überwachung der Sicherheit verwalten müssen.  

Wenn anomale Aktivitäten auftreten, zeigt Security Center Warnungen an und sendet diese optional per E-Mail an die Abonnementadministratoren. Diese Warnungen enthalten Details zu verdächtigen Aktivitäten und Empfehlungen zur Untersuchung und Eindämmung von Bedrohungen. 

Eine Liste der Azure Key Vault-Warnungen finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-azurekv).





## <a name="threat-protection-for-other-microsoft-services"></a>Bedrohungsschutz für andere Microsoft-Dienste <a name="alerts-other"></a>

### <a name="threat-protection-for-azure-waf"></a>Bedrohungsschutz für Azure WAF <a name="azure-waf"></a>

Azure Application Gateway verfügt über eine Web Application Firewall (WAF), die den zentralisierten Schutz Ihrer Webanwendungen vor allgemeinen Exploits und Sicherheitsrisiken ermöglicht.

Webanwendungen sind zunehmend Ziele böswilliger Angriffe, die allgemein bekannte Sicherheitslücken ausnutzen. Die Application Gateway-WAF basiert auf Version 3.0 oder 2.2.9 des Kernregelsatzes aus dem Open Web Application Security Project. Die WAF wird automatisch aktualisiert und bietet ganz Schutz vor neuen Sicherheitsrisiken. 

Wenn Sie über eine Lizenz für Azure WAF verfügen, werden Ihre WAF-Warnungen ganz ohne zusätzliche Konfiguration an Security Center gestreamt. Weitere Informationen zu den von WAF generierten Warnungen finden Sie unter [CRS-Regelgruppen und -Regeln der Web Application Firewall](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md?tabs=owasp31#crs911-31).


### <a name="threat-protection-for-azure-ddos-protection"></a>Bedrohungsschutz für Azure DDoS Protection <a name="azure-ddos"></a>

Verteilte Denial-of-Service-Angriffe (Distributed Denial of Service, DDoS) sind bekanntlich einfach durchführbar. Sie haben sich zu einem wichtigen Sicherheitsthema entwickelt, insbesondere wenn Sie Ihre Anwendungen zur Cloud migrieren. 

Ein DDoS-Angriff hat das Ziel, die Ressourcen einer Anwendung zu verbrauchen, damit sie für berechtigte Benutzer nicht mehr verfügbar ist. DDoS-Angriffe können jeden beliebigen Endpunkt ins Visier nehmen, der über das Internet erreichbar sind.

Um sich vor DDoS-Angriffe zu schützen, erwerben Sie eine Lizenz für Azure DDoS Protection, und befolgen Sie unbedingt die bewährten Methoden für den Anwendungsentwurf. Azure DDoS Protection bietet verschiedene Dienstebenen. Weitere Informationen finden Sie in der [Übersicht über Azure DDoS Protection Standard](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

Eine Liste der Azure DDoS Protection-Warnungen finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-azureddos).


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Sicherheitswarnungen dieser Bedrohungsschutzfeatures finden Sie in den folgenden Artikeln:

* [Referenztabelle für alle Azure Security Center-Warnungen](alerts-reference.md)
* [Sicherheitswarnungen in Azure Security Center](security-center-alerts-overview.md)
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Exportieren von Sicherheitswarnungen und -empfehlungen (Vorschau)](continuous-export.md)