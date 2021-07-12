---
title: Versionshinweise für Azure Security Center
description: Enthält eine Beschreibung der Neuerungen und Änderungen in Azure Security Center.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: reference
ms.date: 06/14/2021
ms.author: memildin
ms.openlocfilehash: fff347c230b148548db94e97afd6a1ef3254f231
ms.sourcegitcommit: 23040f695dd0785409ab964613fabca1645cef90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112062584"
---
# <a name="whats-new-in-azure-security-center"></a>Neuerungen in Azure Security Center

Azure Security Center befindet sich in der aktiven Entwicklung und wird ständig verbessert. Damit Sie bezüglich der aktuellen Entwicklungen immer auf dem neuesten Stand sind, enthält diese Seite Informationen zu neuen Features, Fehlerbehebungen und veralteten Funktionen.

Es ist ratsam, diese Seite regelmäßig zu besuchen, da sie immer wieder aktualisiert wird. 

Weitere Informationen zu den *geplanten* Änderungen, die demnächst im Security Center anstehen, finden Sie unter [Wichtige bevorstehende Änderungen in Azure Security Center](upcoming-changes.md). 

> [!TIP]
> Elemente, die älter als sechs Monate sind, finden Sie im [Archiv zu den Neuerungen in Azure Security Center](release-notes-archive.md).


## <a name="june-2021"></a>Juni 2021

Zu den Updates im Juni gehören:

- [Neue Warnung für Azure Defender für Key Vault](#new-alert-for-azure-defender-for-key-vault)
- [Empfehlungen zur Verschlüsselung mit standardmäßig deaktivierten kundenseitig verwalteten Schlüsseln](#recommendations-to-encrypt-with-customer-managed-keys-cmks-disabled-by-default)
- [Änderung des Präfix für Kubernetes-Warnungen von „AKS_“ in „K8S_“](#prefix-for-kubernetes-alerts-changed-from-aks_-to-k8s_)
- [Zwei Empfehlungen der Sicherheitskontrolle „Systemupdates anwenden“ wurden als veraltet eingestuft](#deprecated-two-recommendations-from-apply-system-updates-security-control)


### <a name="new-alert-for-azure-defender-for-key-vault"></a>Neue Warnung für Azure Defender für Key Vault

Wir haben die folgende Warnung hinzugefügt, um den Bedrohungsschutz von Azure Defender für Key Vault zu erweitern:

| Warnung (Warnungstyp)                                                                 | Beschreibung                                                                                                                                                                                                                                                                                                                                                      | MITRE-Taktik | Schweregrad |
|------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------:|----------|
| Zugriff auf einen Schlüsseltresor von einer verdächtigen IP-Adresse<br>(KV_SuspiciousIPAccess)  | Auf einen Schlüsseltresor ist ein erfolgreicher Zugriff von einer IP-Adresse erfolgt, die von Microsoft Threat Intelligence als verdächtige IP-Adresse identifiziert wurde. Dies kann ggf. ein Hinweis darauf sein, dass Ihre Infrastruktur kompromittiert wurde. Wir empfehlen Ihnen, dies eingehender zu untersuchen. Weitere Informationen finden Sie unter [Threat Intelligence-Funktionen von Microsoft](https://go.microsoft.com/fwlink/?linkid=2128684). | Zugriff auf Anmeldeinformationen                            | Medium   |
|||

Weitere Informationen finden Sie unter
- [Einführung in Azure Defender für Key Vault](defender-for-resource-manager-introduction.md)
- [Reagieren auf Warnungen zu Azure Defender für Key Vault](defender-for-key-vault-usage.md)
- [Liste mit Warnungen von Azure Defender für Key Vault](alerts-reference.md#alerts-azurekv)


### <a name="recommendations-to-encrypt-with-customer-managed-keys-cmks-disabled-by-default"></a>Empfehlungen zur Verschlüsselung mit standardmäßig deaktivierten kundenseitig verwalteten Schlüsseln

Security Center enthält mehrere Empfehlungen zur Verschlüsselung von ruhenden Daten mit kundenseitig verwalteten Schlüsseln, z. B.:

- Containerregistrierungen müssen mit einem kundenseitig verwalteten Schlüssel (CMK) verschlüsselt werden
- Azure Cosmos DB-Konten sollten kundenseitig verwaltete Schlüssel zur Verschlüsselung ruhender Daten verwenden.
- Azure Machine Learning-Arbeitsbereiche müssen mit einem kundenseitig verwalteten Schlüssel (CMK) verschlüsselt werden

Daten werden in Azure automatisch mit plattformseitig verwalteten Schlüsseln verschlüsselt. Daher sollten kundenseitig verwaltete Schlüssel nur angewendet werden, wenn dies zur Einhaltung einer bestimmten Richtlinie erforderlich ist, die in Ihrer Organisation durchgesetzt werden soll.

Aufgrund dieser Änderung sind die Empfehlungen zur Nutzung von kundenseitig verwalteten Schlüsseln jetzt **standardmäßig deaktiviert**. Sie können sie wieder aktivieren, falls dies für Ihre Organisation relevant ist. Ändern Sie hierfür den Parameter *Effect* für die entsprechende Sicherheitsrichtlinie in **AuditIfNotExists** oder **Enforce**. Weitere Informationen finden Sie auf der Seite mit den Informationen zum [Aktivieren einer Sicherheitsrichtlinie](tutorial-security-policy.md#enable-a-security-policy).

Diese Änderung wird in den Namen der Empfehlung durch das neue Präfix **[Bei Bedarf aktivieren]** widergespiegelt. Dies wird in den folgenden Beispielen veranschaulicht:

- [Bei Bedarf aktivieren] Speicherkonten müssen für die Verschlüsselung von ruhenden Daten einen kundenseitig verwalteten Schlüssel verwenden
- [Bei Bedarf aktivieren] Containerregistrierungen müssen mit einem kundenseitig verwalteten Schlüssel verschlüsselt werden
- [Bei Bedarf aktivieren] Azure Cosmos DB-Konten müssen kundenseitig verwaltete Schlüssel zur Verschlüsselung ruhender Daten verwenden

:::image type="content" source="media/upcoming-changes/customer-managed-keys-disabled.png" alt-text="CMK-Empfehlungen von Security Center sind standardmäßig deaktiviert" lightbox="media/upcoming-changes/customer-managed-keys-disabled.png":::


### <a name="prefix-for-kubernetes-alerts-changed-from-aks_-to-k8s_"></a>Änderung des Präfix für Kubernetes-Warnungen von „AKS_“ in „K8S_“

Azure Defender für Kubernetes wurde kürzlich erweitert, um Kubernetes-Cluster zu schützen, die lokal und in Umgebungen mit mehreren Clouds gehostet werden. Weitere Informationen finden Sie unter [Verwenden von Azure Defender für Kubernetes zum Schutz von Hybrid- und Multi-Cloud-Bereitstellungen von Kubernetes (Vorschau)](release-notes.md#use-azure-defender-for-kubernetes-to-protect-hybrid-and-multi-cloud-kubernetes-deployments-in-preview).

Um deutlich zu machen, dass die von Azure Defender für Kubernetes bereitgestellten Sicherheitswarnungen nicht mehr auf Cluster in Azure Kubernetes Service beschränkt sind, haben wir das Präfix für die Warnungstypen von „AKS_“ in „K8S_“ geändert. Bei Bedarf wurden auch die Namen und Beschreibungen aktualisiert. Ein Beispiel:

|Warnung (Warnungstyp)|Beschreibung|
|----|----|
|Erkannte Tools für Kubernetes-Penetrationstests<br>(**AKS** _PenTestToolsKubeHunter)|Die Analyse des Kubernetes-Überwachungsprotokolls hat die Verwendung von Kubernetes-Penetrationstesttools im **AKS**-Cluster erkannt. Dieses Verhalten kann zwar legitim sein, aber Angreifer können solche öffentlichen Tools für böswillige Zwecke nutzen.
|||

wurde geändert in:

|Warnung (Warnungstyp)|Beschreibung|
|----|----|
|Erkannte Tools für Kubernetes-Penetrationstests<br>(**K8S** _PenTestToolsKubeHunter)|Die Analyse des Kubernetes-Überwachungsprotokolls hat die Verwendung von Kubernetes-Penetrationstesttools im **Kubernetes**-Cluster erkannt. Dieses Verhalten kann zwar legitim sein, aber Angreifer können solche öffentlichen Tools für böswillige Zwecke nutzen.|
|||

Unterdrückungsregeln, in denen auf mit „AKS_“ beginnende Warnungen verwiesen wird, wurden automatisch konvertiert. Wenn Sie SIEM-Exporte oder benutzerdefinierte Automatisierungsskripts eingerichtet haben, in denen anhand des Warnungstyps auf Kubernetes-Warnungen verwiesen wird, müssen diese mit den neuen Warnungstypen aktualisiert werden.

Eine vollständige Liste der Kubernetes-Warnungen finden Sie unter [Warnungen für Container: Kubernetes-Cluster](alerts-reference.md#alerts-k8scluster).

### <a name="deprecated-two-recommendations-from-apply-system-updates-security-control"></a>Zwei Empfehlungen der Sicherheitskontrolle „Systemupdates anwenden“ wurden als veraltet eingestuft

Die beiden folgenden Empfehlungen wurden als veraltet eingestuft:

- **Die Betriebssystemversion sollte für Ihre Clouddienstrollen aktualisiert werden**: Azure aktualisiert Ihr Gastbetriebssystem standardmäßig in regelmäßigen Abständen auf das neueste Image innerhalb der BS-Familie, die Sie in der Dienstkonfiguration (CSCFG-Datei) angegeben haben (z B. Windows Server 2016).
- **Für Kubernetes Service muss ein Upgrade auf eine Kubernetes-Version ohne Sicherheitsrisiko durchgeführt werden**: Die Evaluierungen dieser Empfehlung sind uns nicht weitreichend genug. Wir planen, die Empfehlung durch eine erweiterte Version zu ersetzen, die besser auf Ihre Sicherheitsanforderungen abgestimmt ist.


## <a name="may-2021"></a>Mai 2021

Zu den Updates im Mai gehören:

- [Azure Defender für DNS und Azure Defender für Resource Manager veröffentlicht (Allgemeine Verfügbarkeit, GA)](#azure-defender-for-dns-and-azure-defender-for-resource-manager-released-for-general-availability-ga)
- [Azure Defender für relationale Open-Source-Datenbanken veröffentlicht (Allgemeine Verfügbarkeit, GA)](#azure-defender-for-open-source-relational-databases-released-for-general-availability-ga)
- [Neue Warnungen für Azure Defender für Resource Manager](#new-alerts-for-azure-defender-for-resource-manager)
- [CI/CD-Sicherheitsrisikoüberprüfung von Containerimages mit GitHub-Workflows und Azure Defender (Vorschau)](#cicd-vulnerability-scanning-of-container-images-with-github-workflows-and-azure-defender-preview)
- [Verfügbarkeit weiterer Resource Graph-Abfragen für einige Empfehlungen](#more-resource-graph-queries-available-for-some-recommendations)
- [Schweregrad der Empfehlung zur SQL-Datenklassifizierung geändert](#sql-data-classification-recommendation-severity-changed)
- [Neue Empfehlungen zur Aktivierung von Funktionen für den vertrauenswürdigen Start (Vorschau)](#new-recommendations-to-enable-trusted-launch-capabilities-in-preview)
- [Neue Empfehlungen für die Härtung von Kubernetes-Clustern (Vorschau)](#new-recommendations-for-hardening-kubernetes-clusters-in-preview)
- [Die Bewertungs-API wurde um zwei neue Felder erweitert.](#assessments-api-expanded-with-two-new-fields)
- [Cloudumgebungsfilter für Ressourcenbestand](#asset-inventory-gets-a-cloud-environment-filter)


### <a name="azure-defender-for-dns-and-azure-defender-for-resource-manager-released-for-general-availability-ga"></a>Azure Defender für DNS und Azure Defender für Resource Manager veröffentlicht (Allgemeine Verfügbarkeit, GA)

Diese beiden breit gefächerten cloudnativen Bedrohungsschutzpläne befinden sich nun in der Phase der allgemeinen Verfügbarkeit.

Diese neuen Schutzmaßnahmen sorgen für eine erhebliche Verbesserung der Resilienz gegenüber Angriffen von Bedrohungsakteuren sowie für eine deutliche Erhöhung der Anzahl von Azure-Ressourcen, die durch Azure Defender geschützt werden.

- **Azure Defender für Resource Manager**: Überwacht automatisch alle in Ihrer Organisation ausgeführten Ressourcenverwaltungsvorgänge. Weitere Informationen finden Sie unter
    - [Einführung in Azure Defender für Resource Manager](defender-for-resource-manager-introduction.md)
    - [Reagieren auf Warnungen von Azure Defender für Resource Manager](defender-for-resource-manager-usage.md)
    - [Warnungen für Resource Manager](alerts-reference.md#alerts-resourcemanager)

- **Azure Defender für DNS**: Überwacht kontinuierlich alle DNS-Abfragen Ihrer Azure-Ressourcen. Weitere Informationen finden Sie unter
    - [Einführung in Azure Defender für DNS](defender-for-dns-introduction.md)
    - [Reagieren auf Warnungen von Azure Defender für DNS](defender-for-dns-usage.md)
    - [Warnungen für DNS](alerts-reference.md#alerts-dns)

Verwenden Sie die folgenden Empfehlungen, um den Prozess für die Aktivierung dieser Pläne zu vereinfachen:

- **Azure Defender für Resource Manager muss aktiviert sein**
- **Azure Defender für DNS muss aktiviert sein**

> [!NOTE]
> Bei Aktivierung dieser Azure Defender-Pläne fallen Gebühren an. Weitere Informationen zu den Preisen pro Region finden Sie in der Security Center-Preisübersicht unter https://aka.ms/pricing-security-center.


### <a name="azure-defender-for-open-source-relational-databases-released-for-general-availability-ga"></a>Azure Defender für relationale Open-Source-Datenbanken veröffentlicht (Allgemeine Verfügbarkeit, GA)

Das Angebot zum SQL-Schutz von Azure Security Center wird um ein neues Paket erweitert, das für Ihre relationalen Open-Source-Datenbanken gilt:

- **Azure Defender für Azure SQL-Datenbankserver** – schützt Ihre nativen Azure SQL Server.
- **Azure Defender für SQL Server auf Computern** – erweitert denselben Schutz auf Ihre SQL Server-Instanzen in Hybrid-, Multicloud- und lokalen Umgebungen
- **Azure Defender für relationale Open-Source-Datenbanken**: Dient zur Verteidigung Ihrer Single Server-Versionen von Azure Database for MySQL, PostgreSQL und MariaDB.

Mit Azure Defender für relationale Open-Source-Datenbanken werden Ihre Server ständig auf Sicherheitsbedrohungen überwacht, und es werden anomale Datenbankaktivitäten erkannt, die auf potenzielle Bedrohungen für Azure Database for MySQL, PostgreSQL und MariaDB hinweisen. Beispiele:

- **Präzise Erkennung von Brute-Force-Angriffen**: Azure Defender für relationale Open-Source-Datenbanken liefert ausführliche Informationen zu versuchten und erfolgreichen Brute-Force-Angriffen. So verfügen Sie über alle Informationen zur Art und zum Status des Angriffs auf Ihre Umgebung, die Sie benötigen, um die Untersuchung durchführen und entsprechend reagieren zu können.
- **Warnungen zur Erkennung von bestimmtem Verhalten**: Mit Azure Defender für relationale Open-Source-Datenbanken werden Sie vor verdächtigem und unerwartetem Verhalten auf Ihren Servern gewarnt, z. B. Änderungen beim Zugriffsmuster Ihrer Datenbank.
- **Auf Threat Intelligence basierende Erkennung**: Für Azure Defender werden die Threat Intelligence-Informationen und eine umfangreiche Wissensdatenbank von Microsoft für die Anzeige von Bedrohungswarnungen genutzt, damit Sie entsprechende Maßnahmen ergreifen können.

Weitere Informationen finden Sie unter [Einführung in Azure Defender für relationale Open-Source-Datenbanken](defender-for-databases-introduction.md).

### <a name="new-alerts-for-azure-defender-for-resource-manager"></a>Neue Warnungen für Azure Defender für Resource Manager

Wir haben die folgenden Warnungen hinzugefügt, um den Bedrohungsschutz von Azure Defender für Resource Manager zu erweitern:

| Warnung (Warnungstyp)                                                                                                                                                | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                              | MITRE-Taktiken | Schweregrad |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------:|----------|
|**In Ihrer Azure-Umgebung wurden für eine RBAC-Rolle auf ungewöhnliche Weise Berechtigungen gewährt (Vorschau)**<br>(ARM_AnomalousRBACRoleAssignment)|Azure Defender für Resource Manager hat die Zuweisung einer RBAC-Rolle entdeckt, die gegenüber anderen Zuweisungen derselben zuweisenden Person bzw. für dieselbe zuweisende Person oder auf Ihrem Mandanten ungewöhnlich ist, weil Anomalien in den folgenden Bereichen bestehen: Zuweisungszeitpunkt, Standort der zuweisenden Person, zuweisende Person, Authentifizierungsmethode, zugewiesene Entitäten, verwendete Clientsoftware, Umfang der Zuweisung. Dieser Vorgang wurde ggf. von einem legitimen Benutzer Ihrer Organisation durchgeführt. Unter Umständen kann dies auch ein Hinweis darauf sein, dass ein Konto Ihrer Organisation übernommen wurde und dass der Bedrohungsakteur versucht, Berechtigungen für ein weiteres Konto in seinem Besitz zu gewähren.|Seitwärtsbewegung, Umgehen von Verteidigungsmaßnahmen|Medium|
|**Für Ihr Abonnement wurde auf verdächtige Weise eine benutzerdefinierte privilegierte Rolle erstellt (Vorschau)**<br>(ARM_PrivilegedRoleDefinitionCreation)|Azure Defender für Resource Manager hat in Ihrem Abonnement eine verdächtige Erstellung der Definition einer benutzerdefinierten privilegierten Rolle erkannt. Dieser Vorgang wurde ggf. von einem legitimen Benutzer Ihrer Organisation durchgeführt. Unter Umständen kann dies auch ein Hinweis darauf sein, dass ein Konto in Ihrer Organisation übernommen wurde und der Bedrohungsakteur versucht, eine privilegierte Rolle für die zukünftige Verwendung zu erstellen, um eine Erkennung zu vermeiden.|Seitwärtsbewegung, Umgehen von Verteidigungsmaßnahmen|Niedrig|
|**Azure Resource Manager-Vorgang von einer verdächtigen IP-Adresse (Vorschau)**<br>(ARM_OperationFromSuspiciousIP)|Azure Defender für Resource Manager hat einen Vorgang über eine IP-Adresse erkannt, die in Threat Intelligence-Feeds als verdächtig gekennzeichnet wurde.|Ausführung|Medium|
|**Azure Resource Manager-Vorgang von einer verdächtigen Proxy-IP-Adresse (Vorschau)**<br>(ARM_OperationFromSuspiciousProxyIP)|Azure Defender für Resource Manager hat einen Ressourcenverwaltungsvorgang von einer IP-Adresse erkannt, die Proxydiensten zugeordnet ist, z. B. TOR. Dieses Verhalten kann legitim sein, aber es wird häufig mit bösartigen Aktivitäten in Verbindung gebracht, wenn Bedrohungsakteure versuchen, ihre Quell-IP-Adresse zu verbergen.|Umgehen von Verteidigungsmaßnahmen|Medium|
||||

Weitere Informationen finden Sie unter
- [Einführung in Azure Defender für Resource Manager](defender-for-resource-manager-introduction.md)
- [Reagieren auf Warnungen von Azure Defender für Resource Manager](defender-for-resource-manager-usage.md)
- [Warnungen für Resource Manager](alerts-reference.md#alerts-resourcemanager)


### <a name="cicd-vulnerability-scanning-of-container-images-with-github-workflows-and-azure-defender-preview"></a>CI/CD-Sicherheitsrisikoüberprüfung von Containerimages mit GitHub-Workflows und Azure Defender (Vorschau)

Azure Defender für Containerregistrierungen ermöglicht DevSecOps-Teams jetzt Einblicke in GitHub Actions-Workflows.

Das neue Feature für Sicherheitsrisikoüberprüfungen für Containerimages mit Nutzung von Trivy unterstützt Ihre Entwickler bei der Überprüfung auf häufige Sicherheitsrisiken in Containerimages, *bevor* für Images der Pushvorgang in Containerregistrierungen erfolgt.

Die Berichte zu Containerüberprüfungen sind in Azure Security Center zusammengefasst und ermöglichen Sicherheitsteams einen besseren Einblick und ein tieferes Verständnis der Ursache für anfällige Containerimages und die zugehörigen ursprünglichen Workflows und Repositorys.

Weitere Informationen finden Sie unter [Identifizieren von anfälligen Containerimages in Ihren CI/CD-Workflows](defender-for-container-registries-cicd.md).

### <a name="more-resource-graph-queries-available-for-some-recommendations"></a>Verfügbarkeit weiterer Resource Graph-Abfragen für einige Empfehlungen

Für alle Empfehlungen von Security Center gibt es die Option, die Informationen zum Status der betroffenen Ressourcen mit Azure Resource Graph über **Abfrage öffnen** anzuzeigen. Ausführliche Informationen zu diesem leistungsstarken Feature finden Sie unter [Überprüfen von Empfehlungsdaten im Azure Resource Graph-Explorer (ARG)](security-center-recommendations.md#review-recommendation-data-in-azure-resource-graph-explorer-arg).

Security Center umfasst integrierte Überprüfungen auf Sicherheitsrisiken, um Ihre VMs, SQL Server-Instanzen und zugehörigen Hosts sowie die Containerregistrierungen auf Sicherheitsrisiken zu überprüfen. Die Ergebnisse werden als Empfehlungen zurückgegeben, wobei alle Einzelergebnisse für die einzelnen Ressourcentypen jeweils in einer Ansicht zusammengefasst sind. Die Empfehlungen lauten wie folgt:

- Sicherheitsrisiken in Azure Container Registry-Images müssen behoben werden (unterstützt von Qualys).
- Sicherheitsrisiken für VMs müssen behoben werden
-  Bei SQL-Datenbanken sollten gefundene Sicherheitsrisiken behoben werden.
-  Bei SQL Servern auf Computern sollten gefundene Sicherheitsrisiken behoben werden.

Dank dieser Änderung können Sie die Schaltfläche **Abfrage öffnen** verwenden, um auch die Abfrage mit den Sicherheitsergebnissen zu öffnen.

:::image type="content" source="media/release-notes/open-query-menu-security-findings.png" alt-text="Schaltfläche „Abfrage öffnen“ umfasst jetzt auch Optionen für eine tiefer gehende Abfrage, mit der die Sicherheitsergebnisse für Empfehlungen angezeigt werden, die auf Sicherheitsrisikoüberprüfungen basieren":::

Die Schaltfläche **Abfrage öffnen** umfasst zudem noch weitere Optionen für einige andere Empfehlungen, die ebenfalls relevant sind.

Weitere Informationen zu den Sicherheitsrisikoüberprüfungen von Security Center:

- [Integrierte Azure Defender-Überprüfung zur Sicherheitsrisikobewertung für Azure- und Hybridcomputer](deploy-vulnerability-assessment-vm.md)
- [Integrierte Azure Defender-Überprüfung zur Sicherheitsrisikobewertung für SQL Server-Instanzen](defender-for-sql-on-machines-vulnerability-assessment.md)
- [Integrierte Azure Defender-Überprüfung zur Sicherheitsrisikobewertung für Containerregistrierungen](defender-for-container-registries-usage.md)

### <a name="sql-data-classification-recommendation-severity-changed"></a>Schweregrad der Empfehlung zur SQL-Datenklassifizierung geändert

Der Schweregrad der Empfehlung **Vertrauliche Daten in Ihren SQL-Datenbanken sollten klassifiziert werden** wurde von **Hoch** in **Niedrig** geändert.

Dies ist Teil der laufenden Änderungen an dieser Empfehlung, die unter [Verbesserung der SQL-Datenklassifizierungsempfehlung](upcoming-changes.md#enhancements-to-sql-data-classification-recommendation) angekündigt werden. 


### <a name="new-recommendations-to-enable-trusted-launch-capabilities-in-preview"></a>Neue Empfehlungen zur Aktivierung von Funktionen für den vertrauenswürdigen Start (Vorschau)

Azure bietet den vertrauenswürdigen Start als nahtlose Möglichkeit zur Verbesserung der Sicherheit von VMs der [Generation 2](../virtual-machines/generation-2.md) an. Der vertrauenswürdige Start bietet Schutz vor komplexen und permanenten Angriffstechniken. Der vertrauenswürdige Start besteht aus mehreren koordinierten Infrastrukturtechnologien, die unabhängig voneinander aktiviert werden können. Jede Technologie bietet eine eigene Schutzschicht gegen komplexe Bedrohungen. Weitere Informationen finden Sie unter [Vertrauenswürdiger Start für Azure-VMs](../virtual-machines/trusted-launch.md).

> [!IMPORTANT]
> Der vertrauenswürdige Start erfordert die Erstellung neuer VMs. Sie können den vertrauenswürdigen Start nicht für vorhandene VMs aktivieren, die ursprünglich ohne vertrauenswürdigen Start erstellt wurden.
> 
> Der vertrauenswürdige Start befindet sich derzeit in der öffentlichen Vorschau. Die Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.

Mit der Empfehlung **Für unterstützte VMs muss ein virtuelles TPM-Gerät aktiviert werden** von Security Center wird sichergestellt, dass für Ihre Azure-VMs ein virtuelles TPM-Gerät genutzt wird. Diese virtualisierte Version einer Trusted Platform Module-Hardwareeinheit ermöglicht die Nachweiserbringung, indem die gesamte Startkette Ihrer VM (UEFI, Betriebssystem, System und Treiber) gemessen wird.

Wenn das virtuelle TPM-Gerät aktiviert ist, kann der sichere Start von der **Erweiterung für den Gastnachweis** per Remotezugriff überprüft werden. Mit den folgenden Empfehlungen wird sichergestellt, dass diese Erweiterung bereitgestellt wird:

- **Für unterstützte Windows-VMs muss der sichere Start aktiviert werden**
- **Für unterstützte Windows-VMs muss die Erweiterung für den Gastnachweis installiert sein**
- **Für unterstützte Windows-VM-Skalierungsgruppen muss die Erweiterung für den Gastnachweis installiert sein**
- **Für unterstützte Linux-VMs muss die Erweiterung für den Gastnachweis installiert sein**
- **Für unterstützte Linux-VM-Skalierungsgruppen muss die Erweiterung für den Gastnachweis installiert sein**

Weitere Informationen finden Sie unter [Vertrauenswürdiger Start für Azure-VMs](../virtual-machines/trusted-launch.md). 

### <a name="new-recommendations-for-hardening-kubernetes-clusters-in-preview"></a>Neue Empfehlungen für die Härtung von Kubernetes-Clustern (Vorschau)

Mit den folgenden Empfehlungen können Sie die weitere Härtung Ihrer Kubernetes-Cluster durchführen.

- **Kubernetes-Cluster dürfen nicht den Standardnamespace verwenden**: Verhindern Sie die Verwendung des Standardnamespace in Kubernetes-Clustern, um ConfigMap-, Pod-, Geheimnis-, Dienst- und Dienstkontoressourcen vor einem nicht autorisierten Zugriff zu schützen.
- **Kubernetes-Cluster müssen die automatische Bereitstellung von API-Anmeldeinformationen deaktivieren**: Deaktivieren Sie die automatische Bereitstellung von API-Anmeldeinformationen, um für eine potenziell kompromittierte Podressource die Ausführung von API-Befehlen für Kubernetes-Cluster zu verhindern.
- **Kubernetes-Cluster sollten keine CAPSYSADMIN-Sicherheitsfunktionen gewähren**

Informationen dazu, wie Sie Ihre containerisierten Umgebungen mit Security Center schützen können, finden Sie unter [Containersicherheit in Security Center](container-security.md).

### <a name="assessments-api-expanded-with-two-new-fields"></a>Die Bewertungs-API wurde um zwei neue Felder erweitert.

Wir haben die folgenden beiden Felder zur [Bewertungs-REST-API](/rest/api/securitycenter/assessments) hinzugefügt:

- **FirstEvaluationDate**: Der Zeitpunkt, zu dem die Empfehlung erstellt und zum ersten Mal ausgewertet wurde. Wird als UTC-Zeit im ISO 8601-Format zurückgegeben.
- **StatusChangeDate**: Der Zeitpunkt, zu dem sich der Status der Empfehlung zuletzt geändert hat. Wird als UTC-Zeit im ISO 8601-Format zurückgegeben.

Der anfängliche Standardwert für diese Felder ist für alle Empfehlungen `2021-03-14T00:00:00+0000000Z`.

Um auf diese Informationen zuzugreifen, können Sie eine der Methoden in der folgenden Tabelle verwenden.

| Tool                 | Details                                                                                                                                                                |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| REST-API-Aufruf        | GET https://management.azure.com/subscriptions/<SUBSCRIPTION_ID>/providers/Microsoft.Security/assessments?api-version=2019-01-01-preview& $expand=statusEvaluationDates |
| Azure Resource Graph | `securityresources`<br>`where type == "microsoft.security/assessments"`                                                                                                |
| Fortlaufendem Export    | Die beiden dedizierten Felder sind für die Log Analytics-Arbeitsbereichsdaten verfügbar.                                                                                            |
| [CSV-Export](continuous-export.md#manual-one-time-export-of-alerts-and-recommendations) | Die beiden Felder sind in den CSV-Dateien enthalten.                                                        |
|                      |                                                                                                                                                                        |


Erfahren Sie mehr zur [Bewertungs-REST-API](/rest/api/securitycenter/assessments).


### <a name="asset-inventory-gets-a-cloud-environment-filter"></a>Cloudumgebungsfilter für Ressourcenbestand

Die Security Center-Seite für den Ressourcenbestand enthält einige Filter, mit denen die Liste mit den angezeigten Ressourcen schnell eingegrenzt werden kann. Weitere Informationen finden Sie unter [Untersuchen und Verwalten Ihrer Ressourcen mit dem Ressourcenbestand und Verwaltungstools](asset-inventory.md).

Ein neuer Filter bietet die Möglichkeit, die Liste gemäß den Cloudkonten einzugrenzen, mit denen Sie über die Multi-Cloud-Features von Security Center eine Verbindung hergestellt haben:

:::image type="content" source="media/asset-inventory/filter-environment.png" alt-text="Umgebungsfilter für Bestand":::

Weitere Informationen zu den Multi-Cloud-Features:

- [Verbinden Ihrer AWS-Konten mit Azure Security Center](quickstart-onboard-aws.md)
- [Herstellen einer Verbindung zwischen Ihren GCP-Konten und Azure Security Center](quickstart-onboard-gcp.md)


## <a name="april-2021"></a>April 2021

Zu den Updates im April gehören:
- [Aktualisierte Seite „Ressourcenintegrität“ (Vorschau)](#refreshed-resource-health-page-in-preview)
- [Containerregistrierungsimages, die vor Kurzem gepullt wurden, werden jetzt wöchentlich erneut überprüft (veröffentlicht für allgemeine Verfügbarkeit – GA).](#container-registry-images-that-have-been-recently-pulled-are-now-rescanned-weekly-released-for-general-availability-ga)
- [Verwenden von Azure Defender für Kubernetes zum Schutz von Hybrid- und Multi-Cloud-Bereitstellungen von Kubernetes (Vorschau)](#use-azure-defender-for-kubernetes-to-protect-hybrid-and-multi-cloud-kubernetes-deployments-in-preview)
- [Microsoft Defender für Endpunkt-Integration mit Azure Defender unterstützt jetzt Windows Server 2019 und Windows 10 Virtual Desktop (WVD) (veröffentlicht für allgemeine Verfügbarkeit – GA).](#microsoft-defender-for-endpoint-integration-with-azure-defender-now-supports-windows-server-2019-and-windows-10-virtual-desktop-wvd-released-for-general-availability-ga)
- [Empfehlungen zum Aktivieren von Azure Defender für DNS und Resource Manager (Vorschau)](#recommendations-to-enable-azure-defender-for-dns-and-resource-manager-in-preview)
- [Drei Standards zur Einhaltung gesetzlicher Bestimmungen wurden hinzugefügt: „Azure CIS 1.3.0“, „CMMC Level 3“ und „Durch New Zealand ISM eingeschränkt“.](#three-regulatory-compliance-standards-added-azure-cis-130-cmmc-level-3-and-new-zealand-ism-restricted)
- [Vier neue Empfehlungen im Zusammenhang mit der Gastkonfiguration (Vorschau)](#four-new-recommendations-related-to-guest-configuration-in-preview)
- [CMK-Empfehlungen wurden in bewährte Methoden für die Sicherheitskontrolle verschoben](#cmk-recommendations-moved-to-best-practices-security-control)
- [Elf Azure Defender-Warnungen als veraltet eingestuft](#11-azure-defender-alerts-deprecated)
- [Zwei Empfehlungen der Sicherheitskontrolle „Systemupdates anwenden“ wurden als veraltet eingestuft](#two-recommendations-from-apply-system-updates-security-control-were-deprecated)
- [Kachel für Azure Defender für SQL auf Computern vom Azure Defender-Dashboard entfernt](#azure-defender-for-sql-on-machine-tile-removed-from-azure-defender-dashboard)
- [21 Empfehlungen zwischen Sicherheitskontrollen verschoben](#21-recommendations-moved-between-security-controls)

### <a name="refreshed-resource-health-page-in-preview"></a>Aktualisierte Seite „Ressourcenintegrität“ (Vorschau)

Die Ressourcenintegrität von Security Center wurde erweitert und verbessert, um eine Momentaufnahmenansicht der Gesamtintegrität einer einzelnen Ressource zu erhalten. 

Sie können ausführliche Informationen zur Ressource und sich alle Empfehlungen im Zusammenhang mit der Ressource ansehen. Bei Verwendung von [Azure Defender](azure-defender.md) werden außerdem auch ausstehende Sicherheitswarnungen für die jeweilige Ressource angezeigt.

Wählen Sie auf der Seite [Ressourcenbestand](asset-inventory.md) eine beliebige Ressource aus, um die Seite „Ressourcenintegrität“ für eine Ressource zu öffnen.

Diese Vorschauseite auf Portalseiten im Security Center zeigt:

1. **Ressourceninformationen:** Zugehörige Ressourcengruppe, zugehöriges Abonnement, geografischer Standort und Ähnliches.
1. **Angewendetes Sicherheitsfeature:** Gibt an, ob Azure Defender für die Ressource aktiviert ist.
1. **Anzahl ausstehenden Empfehlungen und Warnungen:** Die Anzahl ausstehender Sicherheitsempfehlungen und Azure Defender-Warnungen.
1. **Umsetzbare Empfehlungen und handlungsrelevante Warnungen:** Auf zwei Registerkarten werden die Empfehlungen und Warnungen für die Ressource aufgeführt.

:::image type="content" source="media/investigate-resource-health/resource-health-page-virtual-machine.gif" alt-text="Ressourcenintegritätsseite von Azure Security Center mit den Integritätsinformationen für einen virtuellen Computer":::

Weitere Informationen finden Sie unter [Tutorial: Untersuchen der Integrität Ihrer Ressourcen](investigate-resource-health.md).


### <a name="container-registry-images-that-have-been-recently-pulled-are-now-rescanned-weekly-released-for-general-availability-ga"></a>Containerregistrierungsimages, die vor Kurzem gepullt wurden, werden jetzt wöchentlich erneut überprüft (veröffentlicht für allgemeine Verfügbarkeit – GA)

Azure Defender für Containerregistrierungen enthält eine integrierte Überprüfung auf Sicherheitsrisiken. Bei dieser Überprüfung werden alle Images, die Sie in Ihre Registrierung pushen oder per Pullvorgang innerhalb der letzten 30 Tage abgerufen haben, sofort überprüft.

Jeden Tag werden neue Sicherheitsrisiken entdeckt. Nach diesem Update werden Containerimages, die innerhalb der letzten 30 Tage per Pullvorgang aus Ihren Registrierungen abgerufen wurden, wöchentlich **erneut überprüft**. So wird sichergestellt, dass neu ermittelte Sicherheitsrisiken in Ihren Images erkannt werden.

Da die Kosten für die Überprüfung pro Image berechnet werden, fallen für diese erneuten Überprüfungen keine zusätzlichen Kosten an.

Weitere Informationen finden Sie unter [Verwenden von Azure Defender für Containerregistrierungen zum Überprüfen Ihrer Images auf Sicherheitsrisiken](defender-for-container-registries-usage.md).


### <a name="use-azure-defender-for-kubernetes-to-protect-hybrid-and-multi-cloud-kubernetes-deployments-in-preview"></a>Verwenden von Azure Defender für Kubernetes zum Schutz von Hybrid- und Multi-Cloud-Bereitstellungen von Kubernetes (Vorschau)

Azure Defender für Kubernetes erweitert seine Funktionen zum Schutz vor Bedrohungen, um Ihre Cluster unabhängig von Ihrem Bereitstellungsort zu schützen. Dies wurde durch die Integration von [Kubernetes mit Azure Arc-Aktivierung](../azure-arc/kubernetes/overview.md) und der neuen [Erweiterungsfunktionen](../azure-arc/kubernetes/extensions.md) ermöglicht. 

Wenn Sie Azure Arc in ihren Nicht-Azure Kubernetes-Clustern aktiviert haben, bietet eine neue Empfehlung von Azure Security Center an, die Azure Defender-Erweiterung mit nur wenigen Klicks für Sie bereitzustellen.

Verwenden Sie die Empfehlung (**für Kubernetes-Cluster mit Azure Arc-Aktivierung muss die Azure Defender-Erweiterung installiert sein**) und die Erweiterung, um Kubernetes-Clustern zu schützen, die in anderen Cloudanbietern, jedoch nicht in ihren verwalteten Kubernetes-Diensten bereitgestellt werden.

Diese Integration zwischen Azure Security Center, Azure Defender und Kubernetes mit Azure Arc-Aktivierung ermöglicht:

- Einfache Bereitstellung der Azure Defender-Erweiterung für ungeschützte Kubernetes-Cluster mit Azure Arc-Aktivierung (manuell und skalierbar)
- Überwachung der Azure Defender-Erweiterung und ihres Bereitstellungsstatus über das Azure Arc-Portal
- Sicherheitsempfehlungen von Security Center werden auf der neuen Seite "Sicherheit" des Azure Arc-Portals angezeigt
- Von Azure Defender erkannte Sicherheitsbedrohungen werden auf der neuen Seite "Sicherheit" des Azure Arc-Portals angezeigt
- Kubernetes-Cluster mit Azure Arc-Aktivierung werden in die Azure Security Center-Plattform und Benutzeroberfläche integriert

Weitere Informationen finden Sie unter [Verwenden von Azure Defender für Kubernetes mit Ihren lokalen und Multi-Cloud Kubernetes-Clustern](defender-for-kubernetes-azure-arc.md).

:::image type="content" source="media/defender-for-kubernetes-azure-arc/extension-recommendation.png" alt-text="Azure Security Center-Empfehlung zur Bereitstellung der Azure Defender-Erweiterung für Azure Arc-fähige Kubernetes-Cluster" lightbox="media/defender-for-kubernetes-azure-arc/extension-recommendation.png":::


### <a name="microsoft-defender-for-endpoint-integration-with-azure-defender-now-supports-windows-server-2019-and-windows-10-virtual-desktop-wvd-released-for-general-availability-ga"></a>Microsoft Defender für Endpunkt-Integration mit Azure Defender unterstützt jetzt Windows Server 2019 und Windows 10 Virtual Desktop (WVD) (veröffentlicht für allgemeine Verfügbarkeit – GA)

Microsoft Defender für den Endpunkt ist eine ganzheitliche, cloudbasierte Lösung für die Endpunktsicherheit. Sie ermöglicht die risikobasierte Verwaltung und Bewertung von Sicherheitsrisiken sowie Endpunkterkennung und -reaktion (Endpoint Detection and Response, EDR). Eine vollständige Liste mit den Vorteilen der gemeinsamen Nutzung von Defender für Endpunkt und Azure Security Center finden Sie unter [Schützen Sie Ihre Endpunkte mit der in Security Center integrierten EDR-Lösung: Microsoft Defender für Endpunkt](security-center-wdatp.md).

Wenn Sie Azure Defender für Server auf einem Windows-Server aktivieren, ist im Plan eine Lizenz für Defender für Endpunkt enthalten. Falls Sie Azure Defender für Server bereits aktiviert haben und unter Ihrem Abonnement Windows 2019-Server nutzen, wird Defender für Endpunkt mit diesem Update darauf automatisch bereitgestellt. Es ist keine manuelle Aktion erforderlich. 

Die Unterstützung wurde nun auf Windows Server 2019 und [Windows Virtual Desktop (WVD)](../virtual-desktop/overview.md) erweitert.

> [!NOTE]
> Stellen Sie beim Aktivieren von Defender für Endpunkt auf einem Windows Server 2019-Computer sicher, dass die unter [Aktivieren der Integration von Microsoft Defender für Endpunkt](security-center-wdatp.md#enable-the-microsoft-defender-for-endpoint-integration) beschriebenen Voraussetzungen erfüllt sind.


### <a name="recommendations-to-enable-azure-defender-for-dns-and-resource-manager-in-preview"></a>Empfehlungen zum Aktivieren von Azure Defender für DNS und Resource Manager (Vorschau)

Zwei neue Empfehlungen wurden hinzugefügt, um den Prozess zum Aktivieren von [Azure Defender für Resource Manager](defender-for-resource-manager-introduction.md) und [Azure Defender für DNS](defender-for-dns-introduction.md) zu vereinfachen:

- **Azure Defender für Resource Manager muss aktiviert sein**: Defender für Resource Manager überwacht automatisch die Ressourcenverwaltungsvorgänge in Ihrer Organisation. Azure Defender erkennt Bedrohungen und warnt Sie bei verdächtigen Aktivitäten.
- **Azure Defender für DNS muss aktiviert sein**: Defender für DNS bietet eine zusätzliche Schutzebene für Ihre Cloudressourcen, indem alle DNS-Abfragen aus Ihren Azure-Ressourcen fortlaufend überwacht werden. Azure Defender warnt Sie bei verdächtigen Aktivitäten auf der DNS-Ebene.

Bei Aktivierung dieser Azure Defender-Pläne fallen Gebühren an. Weitere Informationen zu den Preisen pro Region finden Sie in der Security Center-Preisübersicht unter https://aka.ms/pricing-security-center.

> [!TIP]
> Empfehlungen der Vorschau versetzen keine Ressourcen in einen fehlerhaften Zustand, und sie werden nicht in die Berechnungen Ihrer Sicherheitsbewertung einbezogen. Setzen Sie sie trotzdem um, wann immer möglich, damit sie nach Ablauf des Vorschauzeitraums zu Ihrer Bewertung beitragen. Informationen zum Umgang mit diesen Empfehlungen finden Sie unter [Umsetzen von Empfehlungen in Azure Security Center](security-center-remediate-recommendations.md).


### <a name="three-regulatory-compliance-standards-added-azure-cis-130-cmmc-level-3-and-new-zealand-ism-restricted"></a>Drei Standards zur Einhaltung gesetzlicher Bestimmungen wurden hinzugefügt: „Azure CIS 1.3.0“, „CMMC Level 3“ und „Durch New Zealand ISM eingeschränkt“.

Wir haben drei Standards für die Verwendung mit Azure Security Center hinzugefügt. Mithilfe des Dashboards zur Einhaltung gesetzlicher Bestimmungen können Sie jetzt Ihre Konformität mit Folgendem nachverfolgen:

- [CIS Microsoft Azure Foundations Benchmark 1.3.0](../governance/policy/samples/cis-azure-1-3-0.md)
- [CMMC Level 3](../governance/policy/samples/cmmc-l3.md)
- [Durch New Zealand ISM eingeschränkt](../governance/policy/samples/new-zealand-ism.md)

Sie können diese Standards Ihren Abonnements zuweisen, wie unter [Anpassen der Standards in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen](update-regulatory-compliance-packages.md) beschrieben.

:::image type="content" source="media/release-notes/additional-regulatory-compliance-standards.png" alt-text="Drei Standards für die Verwendung mit dem Azure Security Center-Dashboard zur Einhaltung gesetzlicher Bestimmungen wurden hinzugefügt." lightbox="media/release-notes/additional-regulatory-compliance-standards.png":::

Weitere Informationen finden Sie hier:
- [Anpassen der Standards in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen](update-regulatory-compliance-packages.md)
- [Tutorial: Verbessern der Einhaltung gesetzlicher Vorschriften](security-center-compliance-dashboard.md)
- [Häufig gestellte Fragen: Dashboard für die Einhaltung gesetzlicher Bestimmungen](security-center-compliance-dashboard.md#faq---regulatory-compliance-dashboard)

### <a name="four-new-recommendations-related-to-guest-configuration-in-preview"></a>Vier neue Empfehlungen im Zusammenhang mit der Gastkonfiguration (Vorschau)

Die [Erweiterung für Gastkonfigurationen](../governance/policy/concepts/guest-configuration.md) von Azure sendet Meldungen an Security Center, um sicherzustellen, dass die In-Guest-Einstellungen Ihrer virtuellen Computer festgeschrieben werden. Die Erweiterung ist für Computer mit Arc-Unterstützung nicht erforderlich, da sie im Arc Connected Machine-Agenten enthalten ist. Die Erweiterung erfordert eine vom System verwaltete Identität auf dem Computer.

Wir haben vier neue Empfehlungen zum Security Center hinzugefügt, damit Sie diese Erweiterung optimal nutzen können.

- In zwei Empfehlungen werden Sie aufgefordert, die Erweiterung und die erforderliche vom System verwaltete Identität zu installieren:
    - **Erweiterung „Gastkonfiguration“ muss auf Ihren Computern installiert sein**
    - **VM-Erweiterung „Gastkonfiguration“ muss mit einer systemseitig zugewiesenen verwalteten Identität bereitgestellt werden**

- Wenn die Erweiterung installiert ist und ausgeführt wird, beginnt Sie mit der Überprüfung ihrer Computer, und Sie werden aufgefordert, Einstellungen wie die Konfiguration der Betriebssystem- und Umgebungseinstellungen festzuschreiben. Mit diesen beiden Empfehlungen werden Sie dazu aufgefordert, Ihre Windows-und Linux-Computer wie beschrieben festzuschreiben:
    - **Windows Defender Exploit Guard muss auf Ihren Computern aktiviert sein**
    - **Für die Authentifizierung bei Linux-Computern muss ein SSH-Schlüssel erforderlich sein**

Weitere Informationen finden Sie in [Grundlegendes zur Gastkonfiguration von Azure Policy](../governance/policy/concepts/guest-configuration.md).

### <a name="cmk-recommendations-moved-to-best-practices-security-control"></a>CMK-Empfehlungen wurden in bewährte Methoden für die Sicherheitskontrolle verschoben

Das Sicherheitsprogramm jeder Organisation enthält Anforderungen an die Datenverschlüsselung. Die Daten von Azure-Kunden werden im Ruhezustand standardmäßig mit dienstseitig verwalteten Schlüsseln verschlüsselt. Kundenseitig verwaltete Schlüssel (CMK) sind aber häufig zur Einhaltung gesetzlicher Bestimmungen erforderlich. Mit CMKs können Sie Ihre Daten mit einem [Azure Key Vault](../key-vault/general/overview.md)-Schlüssel verschlüsseln, der von Ihnen erstellt wird und sich in Ihrem Besitz befindet. So haben Sie die volle Kontrolle über den Schlüssellebenszyklus, einschließlich der Rotation und Verwaltung, und sind dafür entsprechend verantwortlich.

Bei Sicherheitskontrollen von Azure Security Center handelt es sich um logische Gruppen mit verwandten Sicherheitsempfehlungen, die Ihren anfälligen Angriffsflächen entsprechen. Jede Sicherheitskontrolle verfügt über eine maximale Anzahl von Punkten, die Ihrer Sicherheitsbewertung hinzugefügt wird, wenn Sie alle in der Sicherheitskontrolle aufgeführten Empfehlungen für Ihre gesamten Ressourcen umsetzen. Die Sicherheitskontrolle **Best Practices für die Sicherheit implementieren** hat einen Wert von null Punkten. Daher wirken sich die Empfehlungen dieser Sicherheitskontrolle nicht auf Ihre Sicherheitsbewertung aus.

Die unten angegebenen Empfehlungen werden in die Sicherheitskontrolle **Best Practices für die Sicherheit implementieren** verschoben, um besser zu verdeutlichen, dass sie optional sind. Durch die Verschiebung wird sichergestellt, dass sich diese Empfehlungen in der Sicherheitskontrolle befinden, die zur Erreichung des Ziels am besten geeignet ist.

- Azure Cosmos DB-Konten sollten kundenseitig verwaltete Schlüssel zur Verschlüsselung ruhender Daten verwenden.
- Azure Machine Learning-Arbeitsbereiche müssen mit einem kundenseitig verwalteten Schlüssel (CMK) verschlüsselt werden
- Cognitive Services-Konten müssen eine Datenverschlüsselung mit einem kundenseitig verwalteten Schlüssel (CMK) aktivieren
- Containerregistrierungen müssen mit einem kundenseitig verwalteten Schlüssel (CMK) verschlüsselt werden
- SQL Managed Instance-Instanzen müssen kundenseitig verwaltete Schlüssel zur Verschlüsselung ruhender Daten verwenden
- SQL Server-Instanzen müssen kundenseitig verwaltete Schlüssel zur Verschlüsselung ruhender Daten verwenden
- Speicherkonten sollten einen kundenseitig verwalteten Schlüssel (Customer-Managed Key, CMK) zur Verschlüsselung verwenden.

Informationen dazu, welche Empfehlungen in den einzelnen Sicherheitssteuerungen enthalten sind, finden Sie unter [Sicherheitskontrollen und deren Empfehlungen](secure-score-security-controls.md#security-controls-and-their-recommendations).


### <a name="11-azure-defender-alerts-deprecated"></a>Elf Azure Defender-Warnungen als veraltet eingestuft

Die elf nachfolgend aufgeführten Azure Defender-Warnungen wurden als veraltet eingestuft.

- Neue Warnungen werden diese beiden Warnungen ersetzt und für eine bessere Abdeckung sorgen:

    | AlertType                | AlertDisplayName                                                         |
    |--------------------------|--------------------------------------------------------------------------|
    | ARM_MicroBurstDomainInfo | PREVIEW – MicroBurst toolkit „Get-AzureDomainInfo“ function run detected (VORSCHAU – Ausführung der MicroBurst-Toolkitfunktion „Get-AzureDomainInfo“ erkannt) |
    | ARM_MicroBurstRunbook    | PREVIEW – MicroBurst toolkit „Get-AzurePasswords“ function run detected (VORSCHAU – Ausführung der MicroBurst-Funktion „Get-AzurePasswords“ erkannt)  |
    |                          |                                                                          |

- Die folgenden neun Warnungen beziehen sich auf einen Azure Active Directory Identity Protection-Connector (IPC), der bereits veraltet ist:

    | AlertType           | AlertDisplayName              |
    |---------------------|-------------------------------|
    | UnfamiliarLocation  | Ungewöhnliche Anmeldeeigenschaften |
    | AnonymousLogin      | Anonyme IP-Adresse          |
    | InfectedDeviceLogin | Mit Schadsoftware verknüpfte IP-Adresse     |
    | ImpossibleTravel    | Ungewöhnlicher Ortswechsel               |
    | MaliciousIP         | Schädliche IP-Adresse          |
    | LeakedCredentials   | Kompromittierte Anmeldeinformationen            |
    | PasswordSpray       | Kennwortspray                |
    | LeakedCredentials   | Azure AD Threat Intelligence  |
    | AADAI               | Azure AD-KI                   |
    |                     |                               |
 
    > [!TIP]
    > Bei diesen neun IPC-Warnungen hat es sich niemals um Security Center-Warnungen gehandelt. Sie sind Teil des Azure Active Directory (AAD) Identity Protection Connector (IPC), der Sie an Security Center gesendet hat. In den letzten zwei Jahren haben nur solche Kunden diese Warnungen gesehen, die den Export (vom Connector zu ASC) im Jahr 2019 oder früher konfiguriert haben. AAD IPC hat Sie weiterhin in den eigenen Warnsystemen angezeigt und waren weiterhin in Azure Sentinel verfügbar. Die einzige Veränderung besteht darin, dass sie nicht mehr in Security Center angezeigt werden.

### <a name="two-recommendations-from-apply-system-updates-security-control-were-deprecated"></a>Zwei Empfehlungen der Sicherheitskontrolle „Systemupdates anwenden“ wurden als veraltet eingestuft 

Die folgenden beiden Empfehlungen wurden als veraltet eingestuft und die Änderungen können geringfügige Auswirkungen auf Ihre Sicherheitsbewertung haben:

- **Ihre Computer sollten zur Anwendung von Systemupdates neu gestartet werden**
- **Der Überwachungs-Agent sollte auf Ihren Computern installiert werden.** Diese Empfehlung gilt nur für lokale Computer. Ein Teil der Logik wird in eine andere Empfehlung übertragen: **Log Analytics-Agent-Integritätsprobleme müssen auf Computern gelöst werden**

Wir empfehlen Ihnen, Ihre Konfigurationen für den fortlaufenden Export und die Workflowautomatisierung zu überprüfen, um zu ermitteln, ob diese Empfehlungen darin enthalten sind. Darüber hinaus sollten Sie alle Dashboards oder anderen Überwachungstools, für die diese ggf. genutzt werden, entsprechend aktualisieren.

Weitere Informationen zu diesen Empfehlungen finden Sie auf der [Referenzseite zu Sicherheitsempfehlungen](recommendations-reference.md).

### <a name="azure-defender-for-sql-on-machine-tile-removed-from-azure-defender-dashboard"></a>Kachel für Azure Defender für SQL auf Computern vom Azure Defender-Dashboard entfernt

Der Abdeckungsbereich des Azure Defender-Dashboards enthält Kacheln für die relevanten Azure Defender-Pläne für Ihre Umgebung. Aufgrund eines Problems mit der Meldung der Anzahl geschützter und nicht geschützter Ressourcen haben wir uns entschieden, den Ressourcenabdeckungsstatus für **Azure Defender für SQL auf Computern** vorübergehend zu entfernen, bis das Problem behoben ist.


### <a name="21-recommendations-moved-between-security-controls"></a>21 Empfehlungen zwischen Sicherheitskontrollen verschoben 

Die folgenden Empfehlungen wurden in andere Sicherheitskontrollen verschoben. Bei Sicherheitskontrollen handelt es sich um logische Gruppen verwandter Sicherheitsempfehlungen, die anfällige Angriffsflächen widerspiegeln. Durch die Verschiebung wird sichergestellt, dass sich jede dieser Empfehlungen in der am besten geeigneten Kontrolle befindet, um das jeweilige Ziel zu erreichen.

Informationen dazu, welche Empfehlungen in den einzelnen Sicherheitssteuerungen enthalten sind, finden Sie unter [Sicherheitskontrollen und deren Empfehlungen](secure-score-security-controls.md#security-controls-and-their-recommendations).

|Empfehlung |Änderung und Auswirkung  |
|---------|---------|
|Für Ihre SQL Server-Instanzen muss eine Sicherheitsrisikobewertung aktiviert sein.<br>Bewertung von Sicherheitsrisiken für verwaltete SQL-Instanzen aktivieren<br>Sicherheitsrisiken in Ihren SQL-Datenbanken müssen entschärft werden (neu)<br>Die Sicherheitsrisiken für Ihre SQL-Datenbanken in VMs müssen entschärft werden.     |Verschiebung aus „Sicherheitsrisiken entschärfen“ (Wert: sechs Punkte)<br>in „Sicherheitskonfigurationen bereinigen“ (Wert: vier Punkte).<br>Diese Empfehlungen haben je nach Umgebung eine geringere Auswirkung auf Ihre Bewertung.|
|Ihrem Abonnement sollte mehr als ein Besitzer zugewiesen sein.<br>Automation-Kontovariablen sollten verschlüsselt werden.<br> IoT-Geräte: Überwachter Prozess hat das Senden von Ereignissen beendet<br> IoT-Geräte: Fehler bei Validierung von Baseline von Betriebssystem<br> IoT-Geräte:Upgrade der TLS-Verschlüsselungssammlung erforderlich<br> IoT-Geräte: offene Ports auf Gerät<br> IoT-Geräte: In einer der Ketten wurde eine zu wenig einschränkende Firewallrichtlinie gefunden<br> IoT-Geräte: In der Eingabekette wurde eine zu wenig einschränkende Firewallregel gefunden<br> IoT-Geräte: In der Ausgabekette wurde eine zu wenig einschränkende Firewallregel gefunden<br>In IoT Hub sollten Diagnoseprotokolle aktiviert sein.<br> IoT-Geräte: Agent sendet Nachrichten zu Unterauslastung<br>IoT-Geräte: Die Standard-IP-Filterrichtlinie sollte auf „Verweigern“ festgelegt werden<br>IoT-Geräte: Großer IP-Adressbereich für IP-Filterregel<br>IoT-Geräte: Agent-Nachrichtenintervalle und -größe müssen angepasst werden<br>IoT-Geräte: Identische Anmeldeinformationen für die Authentifizierung<br>IoT-Geräte – Auditd-Prozess hat das Senden von Ereignissen beendet<br>IoT-Geräte: Baselinekonfiguration des Betriebssystems (OS) muss korrigiert werden|Verschiebung in **Bewährte Sicherheitsmethoden implementieren**.<br>Wenn eine Empfehlung in die Sicherheitskontrolle „Bewährte Sicherheitsmethoden implementieren“ (Wert: null Punkte) verschoben wird, wirkt sie sich nicht mehr auf Ihre Sicherheitsbewertung aus.|
|||


## <a name="march-2021"></a>März 2021

Zu den Updates im März gehören:

- [In Security Center integrierte Azure Firewall-Verwaltung](#azure-firewall-management-integrated-into-security-center)
- [SQL-Sicherheitsrisikobewertung enthält jetzt die Option „Regel deaktivieren“ (Vorschauversion)](#sql-vulnerability-assessment-now-includes-the-disable-rule-experience-preview)
- [In Security Center integrierte Azure Monitor-Arbeitsmappen und drei verfügbare Vorlagen](#azure-monitor-workbooks-integrated-into-security-center-and-three-templates-provided)
- [Dashboard für die Einhaltung gesetzlicher Bestimmungen enthält jetzt Azure-Überwachungsberichte (Vorschauversion)](#regulatory-compliance-dashboard-now-includes-azure-audit-reports-preview)
- [Empfehlungsdaten können mit „In ARG untersuchen“ in Azure Resource Graph angezeigt werden](#recommendation-data-can-be-viewed-in-azure-resource-graph-with-explore-in-arg)
- [Updates der Richtlinien für die Bereitstellung der Workflowautomatisierung](#updates-to-the-policies-for-deploying-workflow-automation)
- [Von zwei Legacyempfehlungen werden keine Daten mehr direkt in das Azure-Aktivitätsprotokoll geschrieben](#two-legacy-recommendations-no-longer-write-data-directly-to-azure-activity-log)
- [Verbesserungen der Seite „Empfehlungen“](#recommendations-page-enhancements)


### <a name="azure-firewall-management-integrated-into-security-center"></a>In Security Center integrierte Azure Firewall-Verwaltung

Wenn Sie Azure Security Center öffnen, wird als Erstes die Übersichtsseite angezeigt. 

Dieses interaktive Dashboard ermöglicht eine einheitliche Übersicht über den Sicherheitsstatus Ihrer Hybrid Cloud-Workloads. Darüber hinaus werden darauf Sicherheitswarnungen, Informationen zur Abdeckung und andere Infos angezeigt.

Um Sie beim Anzeigen Ihres Sicherheitsstatus über eine zentrale Benutzeroberfläche zu unterstützen, haben wir u. a. Azure Firewall Manager in dieses Dashboard integriert. Sie können den Firewall-Abdeckungsstatus jetzt für alle Netzwerke überprüfen und über Security Center die Azure Firewall-Richtlinien an einem zentralen Ort verwalten.

Weitere Informationen zu diesem Dashboard finden Sie auf der [Übersichtsseite für Azure Security Center](overview-page.md).

:::image type="content" source="media/release-notes/overview-dashboard-firewall-manager.png" alt-text="Übersichtsdashboard von Security Center mit einer Kachel für Azure Firewall":::


### <a name="sql-vulnerability-assessment-now-includes-the-disable-rule-experience-preview"></a>SQL-Sicherheitsrisikobewertung enthält jetzt die Option „Regel deaktivieren“ (Vorschauversion)

Security Center enthält eine integrierte Überprüfung auf Sicherheitsrisiken, mit der Sie potenzielle Sicherheitsrisiken für Datenbanken ermitteln, nachverfolgen und beseitigen können. Die Ergebnisse Ihrer Überprüfungen für die Bewertung ermöglichen einen Überblick über den Sicherheitszustand Ihrer SQL-Computer und enthalten Einzelheiten zu allen Sicherheitsergebnissen.

Wenn in Ihrer Organisation eine Suche ignoriert werden muss, anstatt sie zu beheben, können Sie sie optional deaktivieren. Deaktivierte Ergebnisse haben keine Auswirkung auf Ihre Sicherheitsbewertung und erzeugen kein unerwünschtes Rauschen.

Weitere Informationen finden Sie unter [Deaktivieren bestimmter Ergebnisse](defender-for-sql-on-machines-vulnerability-assessment.md#disable-specific-findings-preview).



### <a name="azure-monitor-workbooks-integrated-into-security-center-and-three-templates-provided"></a>In Security Center integrierte Azure Monitor-Arbeitsmappen und drei verfügbare Vorlagen

Bei der Ignite im Frühjahr 2021 haben wir eine integrierte Benutzeroberfläche für Azure Monitor-Arbeitsmappen in Security Center angekündigt.

Sie können diese neue Integration nutzen, um mit der Verwendung der vordefinierten Vorlagen aus dem Security Center-Katalog zu beginnen. Mit den Arbeitsmappenvorlagen können Sie auf dynamische und grafische Berichte zugreifen bzw. diese erstellen, um den Sicherheitsstatus Ihrer Organisation nachzuverfolgen. Darüber hinaus können Sie auch neue Arbeitsmappen, die auf Security Center-Daten basieren, oder alle anderen unterstützten Datentypen erstellen und in kurzer Zeit Community-Arbeitsmappen aus der GitHub-Community für Security Center bereitstellen.

Es sind drei Vorlagenberichte verfügbar:

- **Sicherheitsbewertung im Zeitverlauf**: Nutzen Sie die Nachverfolgung der Bewertungen Ihrer Abonnements und der Änderungen von Empfehlungen für Ihre Ressourcen.
- **Systemupdates**: Zeigen Sie fehlende Systemupdates nach Ressource, Betriebssystem, Schweregrad usw. an.
- **Ergebnisse der Sicherheitsrisikobewertung**: Zeigen Sie die Ergebnisse der Sicherheitsrisikobewertungen Ihrer Azure-Ressourcen an.

Informieren Sie sich über die Nutzung dieser Berichte oder die Erstellung eigener Berichte (unter [Erstellen umfassender interaktiver Berichte für Security Center-Daten](custom-dashboards-azure-workbooks.md)).

:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-snip.png" alt-text="Bericht „Sicherheitsbewertung im Zeitverlauf“":::


### <a name="regulatory-compliance-dashboard-now-includes-azure-audit-reports-preview"></a>Dashboard für die Einhaltung gesetzlicher Bestimmungen enthält jetzt Azure-Überwachungsberichte (Vorschauversion)

In der Symbolleiste im Dashboard für die Einhaltung gesetzlicher Bestimmungen können Sie jetzt Zertifizierungsberichte für Azure und Dynamics herunterladen. 

:::image type="content" source="media/release-notes/audit-reports-regulatory-compliance-dashboard.png" alt-text="Symbolleiste des Dashboards für die Einhaltung gesetzlicher Bestimmungen":::

Sie können die Registerkarte für die relevanten Berichtstypen (PCI, SOC, ISO usw.) auswählen und Filter verwenden, um nach den benötigten spezifischen Berichten zu suchen.

Informieren Sie sich über das [Verwalten der Standards in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen](update-regulatory-compliance-packages.md).

:::image type="content" source="media/release-notes/audit-reports-list-regulatory-compliance-dashboard.png" alt-text="Filtern der Liste mit den verfügbaren Azure-Überwachungsberichten":::



### <a name="recommendation-data-can-be-viewed-in-azure-resource-graph-with-explore-in-arg"></a>Empfehlungsdaten können mit „In ARG untersuchen“ in Azure Resource Graph angezeigt werden

Auf den Empfehlungsdetailseiten steht die Schaltfläche „In ARG untersuchen“ zur Verfügung. Verwenden Sie diese Schaltfläche, um eine Azure Resource Graph-Abfrage zu öffnen und die Daten der Empfehlung zu erkunden, zu exportieren und weiterzugeben.

Azure Resource Graph (ARG) bietet mit zuverlässigen Funktionen zum Filtern, Gruppieren und Sortieren sofortigen Zugriff auf Ressourceninformationen in Ihren Cloudumgebungen. Es ist eine schnelle und effiziente Möglichkeit, Informationen über Azure-Abonnements programmgesteuert oder aus dem Azure-Portal heraus abzufragen.

Weitere Informationen zu Azure Resource Graph (ARG) finden Sie [hier](../governance/resource-graph/index.yml).

:::image type="content" source="media/release-notes/explore-in-resource-graph.png" alt-text="Erkunden Sie Empfehlungsdaten in Azure Resource Graph.":::


### <a name="updates-to-the-policies-for-deploying-workflow-automation"></a>Updates der Richtlinien für die Bereitstellung der Workflowautomatisierung

Die Automatisierung der Prozesse Ihrer Organisation zur Überwachung und Reaktion auf Vorfälle kann die Zeit, die zum Untersuchen von Sicherheitsvorfällen und zur Durchführung entsprechender Gegenmaßnahmen benötigt wird, erheblich verkürzen.

Wir stellen drei Azure Policy-Richtlinien vom Typ „DeployIfNotExist“ bereit, mit denen Verfahren für die Workflowautomatisierung erstellt und konfiguriert werden, damit Sie Ihre Automatisierungen in Ihrer gesamten Organisation bereitstellen können:

|Zielsetzung  |Richtlinie  |Richtlinien-ID  |
|---------|---------|---------|
|Workflowautomatisierung für Sicherheitswarnungen|[Bereitstellen der Workflowautomatisierung für Azure Security Center-Warnungen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2ff1525828-9a90-4fcf-be48-268cdd02361e)|f1525828-9a90-4fcf-be48-268cdd02361e|
|Workflowautomatisierung für Sicherheitsempfehlungen|[Bereitstellen der Workflowautomatisierung für Azure Security Center-Empfehlungen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f73d6ab6c-2475-4850-afd6-43795f3492ef)|73d6ab6c-2475-4850-afd6-43795f3492ef|
|Workflowautomatisierung für Änderungen bei der Einhaltung gesetzlicher Bestimmungen|[Workflowautomatisierung für die Einhaltung gesetzlicher Bestimmungen in Azure Security Center bereitstellen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f73d6ab6c-509122b9-ddd9-47ba-a5f1-d0dac20be63c)|509122b9-ddd9-47ba-a5f1-d0dac20be63c|
||||

Für die Features dieser Richtlinien wurden zwei Aktualisierungen durchgeführt:

- Wenn sie zugewiesen werden, wird erzwungen, dass sie aktiviert bleiben.
- Sie können diese Richtlinien jetzt anpassen und alle Parameter auch nach der Bereitstellung noch aktualisieren. Wenn ein Benutzer beispielsweise einen weiteren Bewertungsschlüssel hinzufügen oder einen vorhandenen Bewertungsschlüssel bearbeiten möchte, ist dies möglich.

Beginnen Sie mit [Vorlagen zur Workflowautomatisierung](https://github.com/Azure/Azure-Security-Center/tree/master/Workflow%20automation).

Informieren Sie sich über das [Automatisieren der Reaktionen auf Security Center-Trigger](workflow-automation.md).


### <a name="two-legacy-recommendations-no-longer-write-data-directly-to-azure-activity-log"></a>Von zwei Legacyempfehlungen werden keine Daten mehr direkt in das Azure-Aktivitätsprotokoll geschrieben 

Von Security Center werden die Daten für nahezu alle Sicherheitsempfehlungen an Azure Advisor übergeben und anschließend von Azure Advisor in das [Azure-Aktivitätsprotokoll](../azure-monitor/essentials/activity-log.md) geschrieben.

Bei zwei Empfehlungen werden die Daten gleichzeitig direkt in das Azure-Aktivitätsprotokoll geschrieben. Diese Änderung sorgt dafür, dass Daten für diese Legacysicherheitsempfehlungen von Security Center nicht mehr direkt in das Aktivitätsprotokoll geschrieben werden. Stattdessen werden die Daten genau wie bei allen anderen Empfehlungen nach Azure Advisor exportiert.

Die beiden Legacyempfehlungen sind:
-  Integritätsprobleme in Endpoint Protection sollten auf Ihren Computern behoben werden.
- Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer sollten beseitigt werden.

Wenn Sie auf Informationen für diese beiden Empfehlungen in der Kategorie „Empfehlung vom Typ "TaskDiscovery"“ zugegriffen haben, ist dies nicht mehr möglich.


### <a name="recommendations-page-enhancements"></a>Verbesserungen der Seite „Empfehlungen“ 

Wir haben eine verbesserte Version der Empfehlungsliste veröffentlicht, um mehr Informationen auf einen Blick zu präsentieren.

Auf der Seite sehen Sie nun Folgendes:

1. Die maximale Bewertung und die aktuelle Bewertung für jede Sicherheitskontrolle
1. Symbole, die Tags ersetzen, etwa **Fix** und **Vorschau**
1. Eine neue Spalte mit der [Richtlinieninitiative](security-policy-concept.md) für jede Empfehlung (sichtbar, wenn „Nach Kontrollen gruppieren“ deaktiviert ist)

:::image type="content" source="media/release-notes/recommendations-grid-enhancements.png" alt-text="Verbesserungen der Seite „Empfehlungen“ für Azure Security Center: März 2021" lightbox="media/release-notes/recommendations-grid-enhancements.png":::

:::image type="content" source="media/release-notes/recommendations-grid-enhancements-initiatives.png" alt-text="Verbesserungen der flachen Liste „Empfehlungen“ für Azure Security Center: März 2021" lightbox="media/release-notes/recommendations-grid-enhancements-initiatives.png":::

Weitere Informationen finden Sie unter [Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md).


## <a name="february-2021"></a>Februar 2021

Updates im Februar:

- [Neue Seite „Sicherheitswarnungen“ im Azure-Portal nun allgemein verfügbar](#new-security-alerts-page-in-the-azure-portal-released-for-general-availability-ga)
- [Empfehlungen zum Schutz von Kubernetes-Workloads veröffentlicht (allgemeine Verfügbarkeit)](#kubernetes-workload-protection-recommendations-released-for-general-availability-ga)
- [Microsoft Defender für Endpunkt-Integration mit Azure Defender unterstützt jetzt Windows Server 2019 und Windows 10 Virtual Desktop (WVD) (Vorschau)](#microsoft-defender-for-endpoint-integration-with-azure-defender-now-supports-windows-server-2019-and-windows-10-virtual-desktop-wvd-in-preview)
- [Direkter Link zur Richtlinie auf der Seite mit den Empfehlungsdetails](#direct-link-to-policy-from-recommendation-details-page)
- [Empfehlung zur SQL-Datenklassifizierung hat keine Auswirkung auf Ihre Sicherheitsbewertung mehr](#sql-data-classification-recommendation-no-longer-affects-your-secure-score)
- [Workflowautomatisierungen können durch Änderungen an Bewertungen der Einhaltung gesetzlicher Bestimmungen ausgelöst werden (Vorschau)](#workflow-automations-can-be-triggered-by-changes-to-regulatory-compliance-assessments-in-preview)
- [Erweiterungen für die Seite „Ressourcenbestand“](#asset-inventory-page-enhancements)


### <a name="new-security-alerts-page-in-the-azure-portal-released-for-general-availability-ga"></a>Neue Seite „Sicherheitswarnungen“ im Azure-Portal nun allgemein verfügbar

Die Seite „Sicherheitswarnungen“ von Azure Security Center wurde neu gestaltet. Sie bietet jetzt Folgendes:

- **Eine verbesserte Selektierungsoberfläche für Warnungen**: Die Liste enthält anpassbare Filter und Gruppierungsoptionen und trägt dadurch zur Reduzierung der „Warnungsmüdigkeit“ bei, sodass Sie sich einfacher auf die relevantesten Bedrohungen konzentrieren können.
- **Weitere Informationen in der Warnungsliste**, beispielsweise MITRE ATT&CK-Taktiken
- **Schaltfläche zum Erstellen von Beispielwarnungen**: Zum Evaluieren von Azure Defender-Funktionen und Testen Ihrer Warnungen. Sie können Beispielwarnungen aus allen Azure Defender-Plänen erstellen (Konfiguration für SIEM-Integration, E-Mail-Benachrichtigungen und Workflowautomatisierungen).
- **Angleichung an die Azure Sentinel-Incidentoberfläche**: Für Kunden, die beide Produkte verwenden, ist der Wechsel zwischen den Produkten jetzt unkomplizierter, und es ist einfach, das eine vom anderen zu übernehmen.
- **Bessere Leistung** für große Warnungslisten
- **Tastaturnavigation** durch die Warnungsliste
- **Warnungen aus Azure Resource Graph**: Sie können Warnungen in Azure Resource Graph abfragen, der Kusto-ähnlichen API für alle Ihre Ressourcen. Dies ist auch nützlich, wenn Sie eigene Warnungsdashboards erstellen. [Erfahren Sie mehr über Azure Resource Graph](../governance/resource-graph/index.yml).
- **Feature zum Erstellen von Beispielwarnungen**: Informationen zum Erstellen von Beispielwarnungen über die neue Oberfläche für Warnungen finden Sie unter [Generieren von Azure Defender-Beispielwarnungen](security-center-alert-validation.md#generate-sample-azure-defender-alerts).

:::image type="content" source="media/security-center-managing-and-responding-alerts/alerts-page.png" alt-text="Liste der Sicherheitswarnungen von Azure Security Center":::


### <a name="kubernetes-workload-protection-recommendations-released-for-general-availability-ga"></a>Empfehlungen zum Schutz von Kubernetes-Workloads veröffentlicht (Allgemeine Verfügbarkeit)

Wir freuen uns, ankündigen zu können, dass die Empfehlungen für den Schutz von Kubernetes-Workloads jetzt allgemein verfügbar sind.

Um sicherzustellen, dass Kubernetes-Workloads standardmäßig sicher sind, wurden über Security Center Härtungsempfehlungen auf Kubernetes-Ebene hinzugefügt, z. B. auch Erzwingungsoptionen mit Kubernetes-Zugangskontrolle.

Wenn das Azure Policy-Add-On für Kubernetes in Ihrem AKS-Cluster (Azure Kubernetes Service) installiert ist, wird jede Anforderung an den Kubernetes-API-Server anhand der vordefinierten bewährten Methoden (Anzeige in Form von 13 Sicherheitsempfehlungen) überwacht, bevor sie im Cluster gespeichert wird. Anschließend können Sie das Erzwingen der bewährten Methoden konfigurieren und auf zukünftige Workloads anwenden.

Beispielsweise können Sie vorschreiben, dass keine privilegierten Container erstellt werden sollen und dass zukünftige Anforderungen für diese Aktion blockiert werden sollen.

Weitere Informationen finden Sie unter [Bewährte Methoden zum Schutz von Workloads mithilfe der Kubernetes-Zugangssteuerung](container-security.md#workload-protection-best-practices-using-kubernetes-admission-control).

> [!NOTE]
> Während sich die Empfehlungen in der Vorschauphase befunden haben, haben diese nicht dazu geführt, dass eine AKS-Clusterressource als fehlerhaft gekennzeichnet wurde, und sind nicht in die Berechnung Ihrer Sicherheitsbewertung eingegangen. Ab dem Zeitpunkt dieser Ankündigung zur allgemeinen Verfügbarkeit wirken sie sich aber auf die Bewertungsberechnung aus. Falls Sie sie noch nicht umgesetzt haben, kann sich dies geringfügig auf Ihre Sicherheitsbewertung auswirken. Setzen Sie sie nach Möglichkeit um, wie dies unter [Umsetzen von Empfehlungen in Azure Security Center](security-center-remediate-recommendations.md) beschrieben ist.


### <a name="microsoft-defender-for-endpoint-integration-with-azure-defender-now-supports-windows-server-2019-and-windows-10-virtual-desktop-wvd-in-preview"></a>Microsoft Defender für Endpunkt-Integration mit Azure Defender unterstützt jetzt Windows Server 2019 und Windows 10 Virtual Desktop (WVD) (Vorschau)

Microsoft Defender für den Endpunkt ist eine ganzheitliche, cloudbasierte Lösung für die Endpunktsicherheit. Sie ermöglicht die risikobasierte Verwaltung und Bewertung von Sicherheitsrisiken sowie Endpunkterkennung und -reaktion (Endpoint Detection and Response, EDR). Eine vollständige Liste mit den Vorteilen der gemeinsamen Nutzung von Defender für Endpunkt und Azure Security Center finden Sie unter [Schützen Sie Ihre Endpunkte mit der in Security Center integrierten EDR-Lösung: Microsoft Defender für Endpunkt](security-center-wdatp.md).

Wenn Sie Azure Defender für Server auf einem Windows-Server aktivieren, ist im Plan eine Lizenz für Defender für Endpunkt enthalten. Falls Sie Azure Defender für Server bereits aktiviert haben und unter Ihrem Abonnement Windows 2019-Server nutzen, wird Defender für Endpunkt mit diesem Update darauf automatisch bereitgestellt. Es ist keine manuelle Aktion erforderlich. 

Die Unterstützung wurde nun auf Windows Server 2019 und [Windows Virtual Desktop (WVD)](../virtual-desktop/overview.md) erweitert.

> [!NOTE]
> Stellen Sie beim Aktivieren von Defender für Endpunkt auf einem Windows Server 2019-Computer sicher, dass die unter [Aktivieren der Integration von Microsoft Defender für Endpunkt](security-center-wdatp.md#enable-the-microsoft-defender-for-endpoint-integration) beschriebenen Voraussetzungen erfüllt sind.

### <a name="direct-link-to-policy-from-recommendation-details-page"></a>Direkter Link zur Richtlinie auf der Seite mit den Empfehlungsdetails

Wenn Sie die Details einer Empfehlung überprüfen, ist es häufig hilfreich, die zugrunde liegende Richtlinie zu kennen. Für jede Empfehlung, die von einer Richtlinie unterstützt wird, enthält die Seite mit den Empfehlungsdetails einen neuen Link:

:::image type="content" source="media/release-notes/view-policy-definition.png" alt-text="Link zur Azure Policy-Seite für eine bestimmte Richtlinie, die eine Empfehlung unterstützt":::

Verwenden Sie diesen Link, um die Richtliniendefinition anzuzeigen und die Bewertungslogik zu überprüfen. 

In der Liste mit den Empfehlungen in unserem [Referenzhandbuch für Sicherheitsempfehlungen](recommendations-reference.md) finden Sie auch Links zu den Seiten mit Richtliniendefinitionen:

:::image type="content" source="media/release-notes/view-policy-definition-from-documentation.png" alt-text="Zugriff auf die Azure Policy-Seite für eine bestimmte Richtlinie direkt über die Seite mit der Azure Security Center-Empfehlungsreferenz" lightbox="media/release-notes/view-policy-definition-from-documentation.png":::


### <a name="sql-data-classification-recommendation-no-longer-affects-your-secure-score"></a>Empfehlung zur SQL-Datenklassifizierung hat keine Auswirkung auf Ihre Sicherheitsbewertung mehr
Die Empfehlung **Sensible Daten in Ihren SQL-Datenbanken müssen klassifiziert werden** hat keine Auswirkung auf Ihre Sicherheitsbewertung mehr. Da dies die einzige Empfehlung unter der Sicherheitskontrolle **Datenklassifizierung anwenden** ist, gilt für diese Kontrolle in Bezug auf die Sicherheitsbewertung jetzt der Wert „0“.

Eine vollständige Liste mit allen Sicherheitskontrollen in Security Center und die zugehörigen Bewertungen und Empfehlungen finden Sie unter [Sicherheitskontrollen und deren Empfehlungen](secure-score-security-controls.md#security-controls-and-their-recommendations).

### <a name="workflow-automations-can-be-triggered-by-changes-to-regulatory-compliance-assessments-in-preview"></a>Workflowautomatisierungen können durch Änderungen an Bewertungen der Einhaltung gesetzlicher Bestimmungen ausgelöst werden (Vorschau)
Wir haben den Triggeroptionen für Ihre Workflowautomatisierungen jetzt einen dritten Datentyp hinzugefügt: Änderungen an Bewertungen zur Einhaltung gesetzlicher Bestimmungen.

Informieren Sie sich unter [Automatisieren der Reaktionen auf Security Center-Trigger](workflow-automation.md) über die Nutzung der Tools für die Workflowautomatisierung.

:::image type="content" source="media/release-notes/regulatory-compliance-triggers-workflow-automation.png" alt-text="Verwenden von Änderungen bei Bewertungen der Einhaltung gesetzlicher Bestimmungen zum Auslösen der Workflowautomatisierung" lightbox="media/release-notes/regulatory-compliance-triggers-workflow-automation.png":::


### <a name="asset-inventory-page-enhancements"></a>Erweiterungen für die Seite „Ressourcenbestand“
Die Security Center-Seite „Ressourcenbestand“ wurde wie folgt verbessert:

- Zusammenfassungen am oberen Rand der Seite enthalten jetzt **Nicht registrierte Abonnements** und geben Aufschluss über die Anzahl von Abonnements ohne Security Center-Aktivierung.

    :::image type="content" source="media/release-notes/unregistered-subscriptions.png" alt-text="Anzahl nicht registrierter Abonnements in den Zusammenfassungen am oberen Rand der Seite „Ressourcenbestand“":::

- Filter wurden erweitert und verbessert, um Folgendes einzuschließen:
    - **Anzahl**: Für die Filter wird jeweils die Anzahl von Ressourcen angezeigt, die die Kriterien der jeweiligen Kategorie erfüllen.

        :::image type="content" source="media/release-notes/counts-in-inventory-filters.png" alt-text="Anzahlangabe in den Filtern der Seite „Ressourcenbestand“ von Azure Security Center":::

    - **Ausnahmenfilter** (optional): Ermöglicht das Eingrenzen der Ergebnisse auf Ressourcen mit bzw. ohne Ausnahmen. Dieser Filter wird standardmäßig nicht angezeigt, ist aber über die Schaltfläche **Filter hinzufügen** verfügbar.

        :::image type="content" source="media/release-notes/adding-contains-exemption-filter.gif" alt-text="Hinzufügen des Ausnahmenfilters auf der Seite „Ressourcenbestand“ von Azure Security Center":::

Weitere Informationen zum Erkunden und Verwalten Ihrer Ressourcen mithilfe des Ressourcenbestands und finden Sie [hier](asset-inventory.md).

## <a name="january-2021"></a>Januar 2021

Die Updates im Januar umfassen Folgendes:

- [Der Azure-Sicherheitsvergleichstest ist jetzt die Standardrichtlinieninitiative für Azure Security Center.](#azure-security-benchmark-is-now-the-default-policy-initiative-for-azure-security-center)
- [Die Sicherheitsrisikobewertung für lokale Computer und Multi-Cloud-Computer ist jetzt allgemein verfügbar.](#vulnerability-assessment-for-on-premise-and-multi-cloud-machines-is-released-for-general-availability-ga)
- [Die Sicherheitsbewertung für Verwaltungsgruppen ist jetzt als Vorschau verfügbar.](#secure-score-for-management-groups-is-now-available-in-preview)
- [Die Sicherheitsbewertungs-API ist jetzt allgemein verfügbar.](#secure-score-api-is-released-for-general-availability-ga)
- [Azure Defender für App Service wurde Schutz vor verwaisten DNS-Einträgen hinzugefügt.](#dangling-dns-protections-added-to-azure-defender-for-app-service)
- [Multi-Cloud-Connectors sind jetzt allgemein verfügbar.](#multi-cloud-connectors-are-released-for-general-availability-ga)
- [Möglichkeit, bei Ihrer Sicherheitsbewertung für Abonnements und Verwaltungsgruppen ganze Empfehlungen auszunehmen](#exempt-entire-recommendations-from-your-secure-score-for-subscriptions-and-management-groups)
- [Benutzer können beim globalen Administrator jetzt mandantenweite Sichtbarkeit anfordern.](#users-can-now-request-tenant-wide-visibility-from-their-global-administrator)
- [35 Vorschauempfehlungen werden hinzugefügt, um die Abdeckung des Azure-Sicherheitsvergleichstests zu erhöhen](#35-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark)
- [CSV-Export der gefilterten Liste mit Empfehlungen](#csv-export-of-filtered-list-of-recommendations)
- [Nicht anwendbare Ressourcen werden in Azure Policy-Bewertungen jetzt als „Konform“ gemeldet.](#not-applicable-resources-now-reported-as-compliant-in-azure-policy-assessments)
- [Exportieren wöchentlicher Momentaufnahmen der Daten für die Sicherheitsbewertung und die Einhaltung gesetzlicher Bestimmungen per fortlaufendem Export (Vorschau)](#export-weekly-snapshots-of-secure-score-and-regulatory-compliance-data-with-continuous-export-preview)


### <a name="azure-security-benchmark-is-now-the-default-policy-initiative-for-azure-security-center"></a>Der Azure-Sicherheitsvergleichstest ist jetzt die Standardrichtlinieninitiative für Azure Security Center.

Beim Azure-Sicherheitsvergleichstest handelt es sich um einen von Microsoft erstellten Satz Azure-spezifischer Richtlinien zu bewährten Methoden für Sicherheit und Compliance, die auf allgemeinen Complianceframeworks basieren. Diese weit verbreitete Benchmark basiert auf den Kontrollen des [Center for Internet Security (CIS)](https://www.cisecurity.org/benchmark/azure/) und des [National Institute of Standards and Technology (NIST)](https://www.nist.gov/) und konzentriert sich auf cloudzentrierte Sicherheit.

In den letzten Monaten wurde die Liste der integrierten Sicherheitsempfehlungen von Security Center erheblich erweitert, um unsere Abdeckung dieser Benchmark auszudehnen.

Ab diesem Release bildet die Benchmark die Grundlage für Security Center-Empfehlungen und ist vollständig als Standardrichtlinieninitiative integriert. 

Die Dokumentation jedes Azure-Diensts enthält eine Seite mit der Sicherheitsbaseline. [Hier](security-baseline.md) finden Sie beispielsweise die Baseline von Security Center. Diese Baselines basieren auf dem Vergleichstest für die Azure-Sicherheit.

Auf dem Security Center-Dashboard für die Einhaltung gesetzlicher Bestimmungen werden während eines Übergangszeitraums zwei Instanzen der Benchmark angezeigt:

:::image type="content" source="media/release-notes/regulatory-compliance-with-azure-security-benchmark.png" alt-text="Azure Security Center-Dashboard für die Einhaltung gesetzlicher Bestimmungen mit dem Vergleichstest für die Azure-Sicherheit":::

Auf bereits vorhandene Empfehlungen hat dies keine Auswirkungen, und im Zuge des weiteren Ausbaus der Benchmark werden Änderungen automatisch in Security Center berücksichtigt. 

Weitere Informationen finden Sie auf den folgenden Seiten:

- [Weitere Informationen zum Vergleichstest für die Azure-Sicherheit](/security/benchmark/azure/introduction)
- [Anpassen der Standards in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen](update-regulatory-compliance-packages.md)

### <a name="vulnerability-assessment-for-on-premise-and-multi-cloud-machines-is-released-for-general-availability-ga"></a>Die Sicherheitsrisikobewertung für lokale Computer und Multi-Cloud-Computer ist jetzt allgemein verfügbar.

Im Oktober haben wir eine Vorschauversion für die Überprüfung von Azure Arc-fähigen Servern mit der Lösung für die Überprüfung auf Sicherheitsrisiken (von Qualys) angekündigt, die in [Azure Defender für Server](defender-for-servers-introduction.md) integriert ist.

Dieses Feature ist jetzt allgemein verfügbar.

Wenn Sie Azure Arc auf Ihren Nicht-Azure-Computern aktiviert haben, bietet Security Center eine manuelle und skalierbare Bereitstellung des integrierten Sicherheitsrisikoscanners auf diesen Computern.

Mit diesem Update können Sie die Leistungsfähigkeit von **Azure Defender für Server** nutzen, um Ihr Programm zur Verwaltung von Sicherheitsrisiken auf allen Azure- und Nicht-Azure-Ressourcen zu konsolidieren.

Hauptfunktionen:

- Überwachen des Bereitstellungsstatus des Scanners für die Sicherheitsrisikobewertung auf Azure Arc-Computern
- Bereitstellen des integrierten Agents für die Sicherheitsrisikobewertung auf ungeschützten Azure Arc-Computern unter Windows und Linux (manuell und skalierbar)
- Empfangen und Analysieren ermittelter Sicherheitsrisiken von bereitgestellten Agents (manuell und skalierbar)
- Einheitliche Benutzeroberfläche für Azure-VMs und Azure Arc-Computer

[Erfahren Sie mehr über das Bereitstellen des integrierten Sicherheitsrisikoscanners auf Ihren Hybridcomputern](deploy-vulnerability-assessment-vm.md#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines).

[Erfahren Sie mehr über Server mit Azure Arc-Unterstützung](../azure-arc/servers/index.yml).


### <a name="secure-score-for-management-groups-is-now-available-in-preview"></a>Die Sicherheitsbewertung für Verwaltungsgruppen ist jetzt als Vorschau verfügbar.

Zusätzlich zur Abonnementebene werden auf der Seite „Sicherheitsbewertung“ nun auch die aggregierten Sicherheitsbewertungen für Ihre Verwaltungsgruppen angezeigt. Nun können Sie sich also die Liste der Verwaltungsgruppen in Ihrer Organisation sowie die Bewertung für die jeweilige Verwaltungsgruppe ansehen.

:::image type="content" source="media/secure-score-security-controls/secure-score-management-groups.png" alt-text="Anzeigen der Sicherheitsbewertungen für Ihre Verwaltungsgruppen":::

Weitere Informationen finden Sie unter [Erweiterter Secure Score (Vorschau) in Azure Security Center](secure-score-security-controls.md).

### <a name="secure-score-api-is-released-for-general-availability-ga"></a>Die Sicherheitsbewertungs-API ist jetzt allgemein verfügbar.

Sie können jetzt über die [Sicherheitsbewertungs-API](/rest/api/securitycenter/securescores/) auf Ihre Bewertung zugreifen. Die API-Methoden bieten die Flexibilität, die Daten abzufragen und im Laufe der Zeit einen eigenen Berichtsmechanismus für Ihre Sicherheitsbewertungen zu erstellen. Beispiel:

- Verwenden Sie die API für **Sicherheitsbewertungen**, um die Bewertung für ein bestimmtes Abonnement zu erhalten.
- Verwenden Sie die API für **Sicherheitsbewertungs-Steuerelemente**, um die Sicherheitssteuerelemente und die aktuelle Bewertung Ihrer Abonnements aufzulisten.

Informieren Sie sich im [Bereich zu den Sicherheitsbewertungen in unserer GitHub-Community](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score) über die externen Tools, die mit der Sicherheitsbewertungs-API verwendet werden können.

Weitere Informationen finden Sie unter [Erweiterter Secure Score (Vorschau) in Azure Security Center](secure-score-security-controls.md).


### <a name="dangling-dns-protections-added-to-azure-defender-for-app-service"></a>Azure Defender für App Service wurde Schutz vor verwaisten DNS-Einträgen hinzugefügt.

Unterdomänenübernahmen sind eine weit verbreitete Bedrohung mit hohem Schweregrad für Organisationen. Eine Unterdomänenübernahme ist möglich, wenn Sie über einen DNS-Eintrag verfügen, der auf Website verweist, deren Bereitstellung aufgehoben wurde. Solche DNS-Einträge werden auch als „verwaiste DNS“-Einträge bezeichnet. CNAME-Einträge sind besonders anfällig für diese Bedrohung. 

Durch die Übernahme von Unterdomänen können Bedrohungsakteure Datenverkehr, der für die Domäne eines Unternehmens bestimmt ist, an eine Website für schädliche Aktivitäten umleiten.

Von Azure Defender für App Service werden nun verwaiste DNS-Einträge erkannt, wenn eine App Service-Website eingestellt wird. Dies ist der Zeitpunkt, zu dem der DNS-Eintrag auf eine nicht vorhandene Ressource verweist und Ihre Website für eine Unterdomänenübernahme anfällig ist. Diese Schutzmaßnahmen sind unabhängig davon verfügbar, ob Ihre Domänen mit Azure DNS oder mit einer externen Domänenregistrierungsstelle verwaltet werden, und sie gelten sowohl für App Service unter Windows als auch für App Service unter Linux.

Weitere Informationen:

- Referenztabelle [Warnungen für Azure App Service](alerts-reference.md#alerts-azureappserv): Enthält zwei neue Azure Defender-Warnungen, die ausgelöst werden, wenn ein verwaister DNS-Eintrag erkannt wird.
- [Verhindern verwaister DNS-Einträge und Vermeiden von Unterdomänenübernahmen](../security/fundamentals/subdomain-takeover.md): Hier finden Sie Informationen zur Gefahr von Unterdomänenübernahmen sowie zu verwaisten DNS-Einträgen.
- [Einführung in Azure Defender für App Service](defender-for-app-service-introduction.md)


### <a name="multi-cloud-connectors-are-released-for-general-availability-ga"></a>Multi-Cloud-Connectors sind jetzt allgemein verfügbar.

Cloudworkloads umfassen in der Regel mehrere Cloudplattformen, daher muss das auch für Cloudsicherheitsdienste gelten.

Azure Security Center schützt Workloads in Azure, Amazon Web Services (AWS) und Google Cloud Platform (GCP).

Wenn Sie eine Verbindung mit Ihren AWS- oder GCP-Konten herstellen, werden ihre nativen Sicherheitstools wie AWS Security Hub und GCP Security Command Center in Azure Security Center integriert.

Das bedeutet, dass Security Center Transparenz und Schutz für alle gängigen Cloudumgebungen bereitstellt. Diese Integration hat unter anderem folgende Vorteile:

- Automatische Agent-Bereitstellung: Security Center verwendet Azure Arc, um den Log Analytics-Agent für Ihre AWS-Instanzen bereitzustellen.
- Richtlinienverwaltung
- Verwaltung von Sicherheitsrisiken
- Integrierte Endpunkterkennung und -antwort (Endpoint Detection and Response, EDR)
- Erkennung von Sicherheitsfehlkonfigurationen
- Eine zentrale Ansicht mit Sicherheitsempfehlungen von allen Cloudanbietern
- Einbindung all Ihrer Ressourcen in die Berechnung von Sicherheitsbewertungen durch Security Center
- Bewertung der Einhaltung gesetzlicher Bestimmungen für Ihre AWS- und GPC-Ressourcen

Wählen Sie im Menü von Security Center die Option **Multi cloud connectors** (Connectors mehrerer Clouds) aus, um die Optionen zum Erstellen neuer Connectors anzuzeigen:

:::image type="content" source="./media/quickstart-onboard-aws/add-aws-account.png" alt-text="Schaltfläche zum Hinzufügen eines AWS-Kontos auf der Seite für Connectors mehrerer Clouds in Security Center":::

Weitere Informationen finden Sie hier:
- [Verbinden Ihrer AWS-Konten mit Azure Security Center](quickstart-onboard-aws.md)
- [Herstellen einer Verbindung zwischen Ihren GCP-Konten und Azure Security Center](quickstart-onboard-gcp.md)


### <a name="exempt-entire-recommendations-from-your-secure-score-for-subscriptions-and-management-groups"></a>Möglichkeit, bei Ihrer Sicherheitsbewertung für Abonnements und Verwaltungsgruppen ganze Empfehlungen auszunehmen

Wir erweitern die Ausnahmenfunktion um die Möglichkeit, vollständige Empfehlungen einzuschließen. Dazu stellen wir weitere Optionen für die Feinabstimmung der Sicherheitsempfehlungen zur Verfügung, die Security Center für Ihre Abonnements, Verwaltungsgruppe oder Ressourcen bereitstellt.

Gelegentlich kann es vorkommen, dass eine Ressource als fehlerhaft aufgeführt wird, obwohl Sie wissen, dass das Problem durch ein Drittanbietertool behoben und dies von Security Center nicht erkannt wurde. Auch kann es bisweilen passieren, dass eine Empfehlung in einem Bereich angezeigt wird, zu dem sie Ihrer Meinung nach nicht gehört. Möglicherweise ist die Empfehlung für ein bestimmtes Abonnement nicht geeignet. Oder vielleicht hat Ihr Unternehmen auch die Entscheidung getroffen, die Risiken im Zusammenhang mit der jeweiligen Ressource oder Empfehlung zu akzeptieren.

Mit dieser Previewfunktion können Sie jetzt eine Ausnahme für eine Empfehlung erstellen. Dabei haben Sie folgende Möglichkeiten:

- Sie können **eine Ressource ausnehmen**, um sicherzustellen, dass sie in Zukunft nicht mehr als fehlerhafte Ressourcen aufgeführt wird und keine Auswirkung auf Ihre Sicherheitsbewertung hat. Die Ressource wird als nicht anwendbar aufgeführt, und als Grund wird „Ausgenommen“ mit der spezifischen, von Ihnen ausgewählten Begründung angezeigt.

- Sie können **ein Abonnement oder eine Verwaltungsgruppe ausnehmen**, um sicherzustellen, dass die Empfehlung keine Auswirkung auf Ihre Sicherheitsbewertung hat und in Zukunft nicht mehr für das Abonnement oder die Verwaltungsgruppe angezeigt wird. Das gilt sowohl für bereits vorhandene Ressourcen als auch für alle zukünftig erstellten Ressourcen. Die Empfehlung wird mit der spezifischen Begründung gekennzeichnet, die Sie für den ausgewählten Bereich auswählen.

Weitere Informationen finden Sie unter [Ausschließen einer Ressource aus Empfehlungen und der Sicherheitsbewertung](exempt-resource.md).



### <a name="users-can-now-request-tenant-wide-visibility-from-their-global-administrator"></a>Benutzer können beim globalen Administrator jetzt mandantenweite Sichtbarkeit anfordern.

Wenn ein Benutzer nicht über Berechtigungen zum Anzeigen von Security Center-Daten verfügt, wird ihm jetzt ein Link angezeigt, über den er Berechtigungen vom globalen Administrator der Organisation anfordern kann. Die Anforderung enthält die gewünschte Rolle sowie eine Begründung, warum die Rolle erforderlich ist.

:::image type="content" source="media/security-center-management-groups/request-tenant-permissions.png" alt-text="Banner mit der Information, dass der Benutzer mandantenweite Berechtigungen anfordern kann":::

Weitere Informationen finden Sie unter [Anfordern mandantenweiter Berechtigungen, wenn die eigenen Berechtigungen nicht ausreichen](tenant-wide-permissions-management.md#request-tenant-wide-permissions-when-yours-are-insufficient).


### <a name="35-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark"></a>35 Vorschauempfehlungen werden hinzugefügt, um die Abdeckung des Azure-Sicherheitsvergleichstests zu erhöhen

Der [Azure-Sicherheitsvergleichstest](/security/benchmark/azure/introduction) ist die Standardrichtlinieninitiative in Azure Security Center. 

Die folgenden 35 Vorschauempfehlungen wurden zu Security Center hinzugefügt, um die Abdeckung dieser Benchmark zu erhöhen.

> [!TIP]
> Empfehlungen der Vorschau versetzen keine Ressourcen in einen fehlerhaften Zustand, und sie werden nicht in die Berechnungen Ihrer Sicherheitsbewertung einbezogen. Setzen Sie sie trotzdem um, wann immer möglich, damit sie nach Ablauf des Vorschauzeitraums zu Ihrer Bewertung beitragen. Informationen zum Umgang mit diesen Empfehlungen finden Sie unter [Umsetzen von Empfehlungen in Azure Security Center](security-center-remediate-recommendations.md).

| Sicherheitskontrolle                     | Neue Empfehlungen                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Aktivieren der Verschlüsselung ruhender Daten            | - Azure Cosmos DB-Konten sollten kundenseitig verwaltete Schlüssel zur Verschlüsselung ruhender Daten verwenden.<br>- Azure Machine Learning-Arbeitsbereiche sollten mit einem kundenseitig verwalteten Schlüssel (Customer-Managed Key, CMK) verschlüsselt werden.<br>- BYOK-Datenschutz (Bring Your Own Key) sollte für MySQL-Server aktiviert sein.<br>- BYOK-Datenschutz (Bring Your Own Key) sollte für PostgreSQL-Server aktiviert sein.<br>- Cognitive Services-Konten sollten die Datenverschlüsselung mit einem kundenseitig verwalteten Schlüssel (Customer-Managed Key, CMK) aktivieren.<br>- Containerregistrierungen sollten mit einem kundenseitig verwalteten Schlüssel (Customer-Managed Key, CMK) verschlüsselt werden.<br>- Verwaltete SQL-Instanzen sollten kundenseitig verwaltete Schlüssel zur Verschlüsselung ruhender Daten verwenden.<br>- Computer mit SQL Server sollten kundenseitig verwaltete Schlüssel zur Verschlüsselung ruhender Daten verwenden.<br>- Speicherkonten sollten einen kundenseitig verwalteten Schlüssel (Customer-Managed Key, CMK) zur Verschlüsselung verwenden.                                                                                                                                                              |
| Bewährte Sicherheitsmethoden implementieren    | - In Abonnements sollte eine Kontakt-E-Mail-Adresse für Sicherheitsprobleme angegeben sein.<br> - Für Ihr Abonnement sollte die automatische Bereitstellung des Log Analytics-Agents aktiviert sein.<br> - E-Mail-Benachrichtigungen bei Warnungen mit hohem Schweregrad sollten aktiviert sein.<br> - Das Senden von E-Mail-Benachrichtigungen an den Abonnementbesitzers bei Warnungen mit hohem Schweregrad sollte aktiviert sein.<br> - Für Schlüsseltresore sollte der Löschschutz aktiviert sein.<br> - Für Schlüsseltresore sollte vorläufiges Löschen aktiviert sein. |
| Zugriff und Berechtigungen verwalten        | - Für Funktions-Apps sollte „Clientzertifikate (eingehende Clientzertifikate)“ aktiviert sein. |
| Anwendungen vor DDoS-Angriffen schützen | - Web Application Firewall (WAF) sollte für Application Gateway aktiviert sein.<br> - Web Application Firewall (WAF) sollte für Azure Front Door Service aktiviert sein. |
| Nicht autorisierten Netzwerkzugriff einschränken | - Für Key Vault sollte eine Firewall aktiviert sein.<br> - Für Key Vault sollte ein privater Endpunkt konfiguriert werden.<br> - App Configuration sollte eine private Verbindung verwenden.<br> - Azure Cache for Redis sollte sich in einem virtuellen Netzwerk befinden.<br> - Azure Event Grid-Domänen sollten eine private Verbindung verwenden.<br> - Azure Event Grid-Themen sollten eine private Verbindung verwenden.<br> - Azure Machine Learning-Arbeitsbereiche sollten eine private Verbindung verwenden.<br> - Azure SignalR Service sollte eine private Verbindung verwenden.<br> - Azure Spring Cloud sollte Netzwerkinjektion verwenden.<br> - Containerregistrierungen sollten keinen uneingeschränkten Netzwerkzugriff zulassen.<br> - Containerregistrierungen sollten eine private Verbindung verwenden.<br> - Öffentlicher Netzwerkzugriff sollte für MariaDB-Server deaktiviert sein.<br> - Öffentlicher Netzwerkzugriff sollte für MySQL-Server deaktiviert sein.<br> - Öffentlicher Netzwerkzugriff sollte für PostgreSQL-Server deaktiviert sein.<br> - Das Speicherkonto sollte eine Private Link-Verbindung verwenden.<br> - Speicherkonten sollten den Netzwerkzugriff mithilfe von VNET-Regeln einschränken.<br> - VM Image Builder-Vorlagen sollten eine private Verbindung verwenden.|
|                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

Verwandte Links:

- [Weitere Informationen zum Vergleichstest für die Azure-Sicherheit](/security/benchmark/azure/introduction)
- [Weitere Informationen zu Azure Database for MariaDB](../mariadb/overview.md)
- [Weitere Informationen zu Azure Database for MySQL](../mysql/overview.md)
- [Weitere Informationen zu Azure Database for PostgreSQL](../postgresql/overview.md)




### <a name="csv-export-of-filtered-list-of-recommendations"></a>CSV-Export der gefilterten Liste mit Empfehlungen 

Im November 2020 haben wir der Seite mit den Empfehlungen Filter hinzugefügt ([Die Empfehlungsliste beinhaltet nun Filter](release-notes-archive.md#recommendations-list-now-includes-filters)). Im Dezember haben wir diese Filter erweitert ([Empfehlungsseite enthält neue Filter für Umgebung, Schweregrad und verfügbare Antworten](release-notes-archive.md#recommendations-page-has-new-filters-for-environment-severity-and-available-responses)). 

Mit dieser Ankündigung wird das Verhalten der Schaltfläche für den **Download als CSV** so geändert, dass der CSV-Export nur die Empfehlungen enthält, die derzeit in der gefilterten Liste angezeigt werden. 

In der Abbildung unten ist beispielsweise erkennbar, dass die Liste nach zwei Empfehlungen gefiltert wurde. Die generierte CSV-Datei enthält die Statusdetails für jede Ressource, auf die sich diese beiden Empfehlungen auswirken.   

:::image type="content" source="media/security-center-managing-and-responding-alerts/export-to-csv-with-filters.png" alt-text="Exportieren von gefilterten Empfehlungen als CSV-Datei":::

Weitere Informationen finden Sie unter [Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md).


### <a name="not-applicable-resources-now-reported-as-compliant-in-azure-policy-assessments"></a>Nicht anwendbare Ressourcen werden in Azure Policy-Bewertungen jetzt als „Konform“ gemeldet.

Bislang wurden Ressourcen, die für eine Empfehlung ausgewertet und als **nicht anwendbar** befunden wurden, in Azure Policy als „Nicht konform“ angezeigt. Ihr Zustand kann durch keine Benutzeraktion in „Konform“ geändert werden. Mit dieser Änderung werden sie zur besseren Verständlichkeit als „Konform“ gemeldet.

Die einzige Auswirkung wird in Azure Policy zu sehen sein, weil die Anzahl der konformen Ressourcen dort steigt. Ihre Sicherheitsbewertung in Azure Security Center wird dadurch nicht beeinflusst.


### <a name="export-weekly-snapshots-of-secure-score-and-regulatory-compliance-data-with-continuous-export-preview"></a>Exportieren wöchentlicher Momentaufnahmen der Daten für die Sicherheitsbewertung und die Einhaltung gesetzlicher Bestimmungen per fortlaufendem Export (Vorschau)

Wir haben den Tools für den [fortlaufenden Export](continuous-export.md) eine neue Previewfunktion hinzugefügt, mit der wöchentliche Momentaufnahmen der Daten für die Sicherheitsbewertung und die Einhaltung gesetzlicher Bestimmungen exportiert werden können.

Legen Sie beim Definieren eines Vorgangs für den fortlaufenden Export die Exporthäufigkeit fest:

:::image type="content" source="media/release-notes/export-frequency.png" alt-text="Auswählen der Häufigkeit des fortlaufenden Exports":::

- **Streaming**: Bewertungen werden gesendet, wenn der Integritätszustand einer Ressource aktualisiert wird (wenn keine Updates durchgeführt werden, werden keine Daten gesendet).
- **Momentaufnahmen**: Einmal pro Woche wird eine Momentaufnahme des aktuellen Zustands aller Bewertungen der Einhaltung gesetzlicher Bestimmungen gesendet. (Dies ist eine Previewfunktion für wöchentliche Momentaufnahmen der Daten für Sicherheitsbewertungen und die Einhaltung gesetzlicher Bestimmungen.)

Erfahren Sie mehr zu allen Funktionen dieses Features unter [Fortlaufendes Exportieren von Security Center-Daten](continuous-export.md).