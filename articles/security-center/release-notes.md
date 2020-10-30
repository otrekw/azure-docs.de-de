---
title: Versionshinweise für Azure Security Center
description: Enthält eine Beschreibung der Neuerungen und Änderungen in Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2020
ms.author: memildin
ms.openlocfilehash: 3ea8e944a004dc89dadc74e4ab2e3e4b295b3a9b
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2020
ms.locfileid: "92900233"
---
# <a name="whats-new-in-azure-security-center"></a>Neuerungen in Azure Security Center

Azure Security Center befindet sich in der aktiven Entwicklung und wird ständig verbessert. Damit Sie bezüglich der aktuellen Entwicklungen immer auf dem neuesten Stand sind, enthält diese Seite Informationen zu neuen Features, Fehlerbehebungen und veralteten Funktionen.

Es ist ratsam, diese Seite regelmäßig zu besuchen, da sie immer wieder aktualisiert wird. 

Weitere Informationen zu den *geplanten* Änderungen, die demnächst im Security Center anstehen, finden Sie unter [Wichtige bevorstehende Änderungen in Azure Security Center](upcoming-changes.md). 

> [!TIP]
> Elemente, die älter als sechs Monate sind, finden Sie im [Archiv zu den Neuerungen in Azure Security Center](release-notes-archive.md).



## <a name="october-2020"></a>Oktober 2020

- [Sicherheitsrisikobewertung für lokale und Multi-Cloud-Computer (Vorschau)](#vulnerability-assessment-for-on-premise-and-multi-cloud-machines-preview)
- [Empfehlung für Azure Firewall hinzugefügt (Vorschau)](#azure-firewall-recommendation-added-preview)
- [Die Empfehlung „Für Kubernetes-Dienste sollten autorisierte IP-Adressbereiche definiert werden“ wurde mit einer schnellen Problembehebung aktualisiert](#authorized-ip-ranges-should-be-defined-on-kubernetes-services-recommendation-updated-with-quick-fix)
- [Das Dashboard zur Einhaltung gesetzlicher Bestimmungen enthält jetzt die Option, Standards zu entfernen](#regulatory-compliance-dashboard-now-includes-option-to-remove-standards)
- [Microsoft.Security/securityStatuses-Tabelle aus Azure Resource Graph (ARG) entfernt](#microsoftsecuritysecuritystatuses-table-removed-from-azure-resource-graph-arg)

### <a name="vulnerability-assessment-for-on-premise-and-multi-cloud-machines-preview"></a>Sicherheitsrisikobewertung für lokale und Multi-Cloud-Computer (Vorschau)

Der in [Azure Defender für Server](defender-for-servers-introduction.md) integrierte Scanner für die Sicherheitsrisikobewertung (von Qualys) überprüft jetzt Server mit Azure Arc-Unterstützung.

Wenn Sie Azure Arc auf Ihren Nicht-Azure-Computern aktiviert haben, bietet Security Center eine manuelle und skalierbare Bereitstellung des integrierten Sicherheitsrisikoscanners auf diesen Computern.

Mit diesem Update können Sie die Leistungsfähigkeit von **Azure Defender für Server** nutzen, um Ihr Programm zur Verwaltung von Sicherheitsrisiken auf allen Azure- und Nicht-Azure-Ressourcen zu konsolidieren.

Hauptfunktionen:

- Überwachen des Bereitstellungsstatus des Scanners für die Sicherheitsrisikobewertung auf Azure Arc-Computern
- Bereitstellen des integrierten Agents für die Sicherheitsrisikobewertung auf ungeschützten Azure Arc-Computern unter Windows und Linux (manuell und skalierbar)
- Empfangen und Analysieren ermittelter Sicherheitsrisiken von bereitgestellten Agents (manuell und skalierbar)
- Einheitliche Benutzeroberfläche für Azure-VMs und Azure Arc-Computer

[Erfahren Sie mehr über das Bereitstellen des integrierten Sicherheitsrisikoscanners auf Ihren Hybridcomputern](deploy-vulnerability-assessment-vm.md#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines).

[Erfahren Sie mehr über Server mit Azure Arc-Unterstützung](https://docs.microsoft.com/azure/azure-arc/servers/).


### <a name="azure-firewall-recommendation-added-preview"></a>Empfehlung für Azure Firewall hinzugefügt (Vorschau)

Es wurde eine neue Empfehlung zum Schützen aller virtuellen Netzwerke mit Azure Firewall hinzugefügt.

In der Empfehlung **Virtuelle Netzwerke müssen durch Azure Firewall geschützt werden** wird Ihnen geraten, mithilfe der Azure-Firewall den Zugriff auf Ihre virtuellen Netzwerke einzuschränken und potenzielle Bedrohungen zu verhindern.

Erfahren Sie mehr über [Azure Firewall](https://azure.microsoft.com/services/azure-firewall/).


### <a name="authorized-ip-ranges-should-be-defined-on-kubernetes-services-recommendation-updated-with-quick-fix"></a>Die Empfehlung „Für Kubernetes-Dienste sollten autorisierte IP-Adressbereiche definiert werden“ wurde mit einer schnellen Problembehebung aktualisiert

Für die Empfehlung **Für Kubernetes-Dienste sollten autorisierte IP-Adressbereiche definiert werden** gibt es jetzt eine Option zur schnellen Problembehebung.

Weitere Informationen zu dieser Empfehlung und allen anderen Security Center-Empfehlungen finden Sie unter [Sicherheitsempfehlungen: Referenzhandbuch](recommendations-reference.md).

:::image type="content" source="./media/release-notes/authorized-ip-ranges-recommendation.png" alt-text="Die Empfehlung „Für Kubernetes-Dienste sollten autorisierte IP-Adressbereiche definiert werden“ mit einer Option zur schnellen Problembehebung":::


### <a name="regulatory-compliance-dashboard-now-includes-option-to-remove-standards"></a>Das Dashboard zur Einhaltung gesetzlicher Bestimmungen enthält jetzt die Option, Standards zu entfernen

Das Security Center-Dashboard für die Einhaltung gesetzlicher Bestimmungen bietet Erkenntnisse zu Ihrem Compliancestatus basierend auf der Erfüllung bestimmter Compliancevorgaben und -anforderungen.

Das Dashboard enthält einen Standardsatz gesetzlicher Standards. Falls einer der angegebenen Standards für Ihre Organisation nicht relevant ist, ist es nun problemlos möglich, ihn einfach aus der Benutzeroberfläche für ein Abonnement zu entfernen. Standards können nur auf der Ebene des *Abonnements* entfernt werden, nicht der für den Verwaltungsgruppenbereich.

Weitere Informationen finden Sie unter [Entfernen eines Standards aus Ihrem Dashboard](update-regulatory-compliance-packages.md#removing-a-standard-from-your-dashboard).


### <a name="microsoftsecuritysecuritystatuses-table-removed-from-azure-resource-graph-arg"></a>Microsoft.Security/securityStatuses-Tabelle aus Azure Resource Graph (ARG) entfernt

Azure Resource Graph ist ein Dienst in Azure, der eine effiziente Ressourcenuntersuchung mit der Fähigkeit bereitstellt, übergreifend für eine bestimmte Menge von Abonnements nach Bedarf Abfragen durchzuführen, sodass Sie Ihre Umgebung effektiv beherrschen können. 

Für Azure Security Center können Sie ARG und die [Kusto-Abfragesprache (KQL)](https://docs.microsoft.com/azure/data-explorer/kusto/query/) verwenden, um eine Vielzahl von Daten zum Sicherheitsstatus abzufragen. Zum Beispiel:

- Asset Inventory nutzt (ARG)
- Wir haben eine ARG-Beispielabfrage zum [Identifizieren von Konten ohne aktivierte mehrstufige Authentifizierung (Multi-Factor Authentication, MFA)](security-center-identity-access.md#identify-accounts-without-multi-factor-authentication-mfa-enabled) dokumentiert.

In ARG gibt es Datentabellen, die Sie in Ihren Abfragen verwenden können.

:::image type="content" source="./media/release-notes/azure-resource-graph-tables.png" alt-text="Die Empfehlung „Für Kubernetes-Dienste sollten autorisierte IP-Adressbereiche definiert werden“ mit einer Option zur schnellen Problembehebung"
}
```
Microsoft.Security/Assessments enthält hingegen einen Datensatz für jede Richtlinienbewertung:

```
{
type: "Microsoft.Security/assessments",
id:  "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourceGroups/mico-rg/providers/Microsoft. Network/virtualNetworks/mico-rg-vnet/providers/Microsoft.Security/assessments/e3delcce-f4dd-3b34-e496-8b5381ba2d70",
name: "e3deicce-f4dd-3b34-e496-8b5381ba2d70",
properties:  {
    resourceDetails: {Source: "Azure", Id: "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourceGroups/mico-rg/providers/Microsoft.Network/virtualNetworks/mico-rg-vnet"...},
    displayName: "Azure DDOS Protection Standard should be enabled",
    status: (code: "NotApplicable", cause: "VnetHasNOAppGateways", description: "There are no Application Gateway resources attached to this Virtual Network"...}
}

{
type: "Microsoft.Security/assessments",
id:  "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourcegroups/mico-rg/providers/microsoft.network/virtualnetworks/mico-rg-vnet/providers/Microsoft.Security/assessments/80fac66a-1ec5-be63-a824-eb28671dc527",
name: "8efac66a-1ec5-be63-a824-eb28671dc527",
properties: {
    resourceDetails: (Source: "Azure", Id: "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourcegroups/mico-rg/providers/microsoft.network/virtualnetworks/mico-rg-vnet"...),
    displayName: "Audit diagnostic setting",
    status:  {code: "Unhealthy"}
}
```

**Beispiel für das Konvertieren einer vorhandenen ARG-Abfrage mit securityStatuses für die Verwendung der Assessments-Tabelle:**

Abfrage, die auf SecurityStatuses verweist:

```kusto
SecurityResources 
| where type == 'microsoft.security/securitystatuses' and properties.type == 'virtualMachine'
| where name in ({vmnames}) 
| project name, resourceGroup, policyAssesments = properties.policyAssessments, resourceRegion = location, id, resourceDetails = properties.resourceDetails
```

Ersatzabfrage für die Assessments-Tabelle:

```kusto
securityresources
| where type == "microsoft.security/assessments" and id contains "virtualMachine"
| extend resourceName = extract(@"(?i)/([^/]*)/providers/Microsoft.Security/assessments", 1, id)
| extend source = tostring(properties.resourceDetails.Source)
| extend resourceId = trim(" ", tolower(tostring(case(source =~ "azure", properties.resourceDetails.Id,
source =~ "aws", properties.additionalData.AzureResourceId,
source =~ "gcp", properties.additionalData.AzureResourceId,
extract("^(.+)/providers/Microsoft.Security/assessments/.+$",1,id)))))
| extend resourceGroup = tolower(tostring(split(resourceId, "/")[4]))
| where resourceName in ({vmnames}) 
| project resourceName, resourceGroup, resourceRegion = location, id, resourceDetails = properties.additionalData
```

Weitere Informationen finden Sie unter den folgenden Links:
- [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](../governance/resource-graph/first-query-portal.md)
- [Kusto-Abfragesprache (KQL)](https://docs.microsoft.com/azure/data-explorer/kusto/query/)


## <a name="september-2020"></a>September 2020

Updates im September:
- [Security Center erhält ein neues Aussehen!](#security-center-gets-a-new-look)
- [Azure Defender veröffentlicht](#azure-defender-released)
- [Azure Defender für Key Vault ist allgemein verfügbar.](#azure-defender-for-key-vault-is-generally-available)
- [Azure Defender für Storage ist für Files und ADLS Gen2 allgemein verfügbar.](#azure-defender-for-storage-protection-for-files-and-adls-gen2-is-generally-available)
- [Asset Inventory-Tools sind jetzt allgemein verfügbar.](#asset-inventory-tools-are-now-generally-available)
- [Deaktivieren der Erkennung eines bestimmten Sicherheitsrisikos bei Scans von Containerregistrierungen und virtuellen Computern](#disable-a-specific-vulnerability-finding-for-scans-of-container-registries-and-virtual-machines)
- [Ausschließen einer Ressource aus einer Empfehlung](#exempt-a-resource-from-a-recommendation)
- [AWS- und GCP-Connectors in Security Center für Szenarien mit mehreren Clouds](#aws-and-gcp-connectors-in-security-center-bring-a-multi-cloud-experience)
- [Empfehlungsbündel für Kubernetes-Workloadschutz](#kubernetes-workload-protection-recommendation-bundle)
- [Ergebnisse der Sicherheitsrisikobewertung sind jetzt im fortlaufenden Export verfügbar](#vulnerability-assessment-findings-are-now-available-in-continuous-export)
- [Verhindern von sicherheitsbezogenen Fehlkonfigurationen durch Erzwingen von Empfehlungen beim Erstellen neuer Ressourcen](#prevent-security-misconfigurations-by-enforcing-recommendations-when-creating-new-resources)
- [Empfehlungen für Netzwerksicherheitsgruppen verbessert](#network-security-group-recommendations-improved)
- [Veraltete AKS-Vorschauempfehlung „Für Kubernetes Service müssen Podsicherheitsrichtlinien definiert werden“](#deprecated-preview-aks-recommendation-pod-security-policies-should-be-defined-on-kubernetes-services)
- [E-Mail-Benachrichtigungen von Azure Security Center verbessert](#email-notifications-from-azure-security-center-improved)
- [Sicherheitsbewertung ohne Vorschauempfehlungen](#secure-score-doesnt-include-preview-recommendations)
- [Empfehlungen jetzt mit Schweregradindikator und Aktualisierungsintervall](#recommendations-now-include-a-severity-indicator-and-the-freshness-interval)


### <a name="security-center-gets-a-new-look"></a>Security Center erhält ein neues Aussehen!

Wir haben eine aktualisierte Benutzeroberfläche für die Portalseiten von Security Center veröffentlicht. Die neuen Seiten enthalten eine neue Übersicht sowie Dashboards für Sicherheitsbewertung, Ressourcenbestand und Azure Defender.

Die neu gestaltete Übersicht verfügt jetzt über eine Kachel für den Zugriff auf die Sicherheitsbewertung, den Ressourcenbestand und Azure Defender-Dashboards. Außerdem bietet eine neue Kachel eine Verknüpfung mit dem Compliance-Dashboard.

Erfahren Sie mehr über die [Übersicht](overview-page.md).


### <a name="azure-defender-released"></a>Azure Defender veröffentlicht

**Azure Defender** ist die in Security Center integrierte Cloud Workload Protection Platform (CWPP), die einen erweiterten, intelligenten Schutz Ihrer Azure- und Hybridworkloads bietet. Sie ersetzt den Standard-Tarif von Security Center. 

Wenn Sie Azure Defender über den Bereich **Preise und Einstellungen** von Azure Security Center aktivieren, werden alle folgenden Defender-Pläne aktiviert und bieten umfassende Schutzmechanismen für die Compute-, Daten- und Dienstebenen Ihrer Umgebung:

- [Azure Defender für Server](defender-for-servers-introduction.md)
- [Azure Defender für App Service](defender-for-app-service-introduction.md)
- [Azure Defender für Storage](defender-for-storage-introduction.md)
- [Azure Defender für SQL](defender-for-sql-introduction.md)
- [Azure Defender für Key Vault](defender-for-key-vault-introduction.md)
- [Azure Defender für Kubernetes](defender-for-kubernetes-introduction.md)
- [Azure Defender für Containerregistrierungen](defender-for-container-registries-introduction.md)

Jeder dieser Pläne wird in der Security Center-Dokumentation einzeln erläutert.

Über ein dediziertes Dashboard bietet Azure Defender Sicherheitswarnungen und erweiterten Bedrohungsschutz für virtuelle Computer, SQL-Datenbanken, Container, Webanwendungen, Ihr Netzwerk u. v. m.

Weitere Informationen zu [Azure Defender](azure-defender.md).

### <a name="azure-defender-for-key-vault-is-generally-available"></a>Azure Defender für Key Vault ist allgemein verfügbar

Der Azure Key Vault-Clouddienst schützt Verschlüsselungsschlüssel und Geheimnisse (wie Zertifikate, Verbindungszeichenfolgen und Kennwörter). 

**Azure Defender für Key Vault** bietet in Azure nativen erweiterten Bedrohungsschutz für Azure Key Vault und dadurch eine zusätzliche Ebene der Sicherheitsintelligenz. Durch die Erweiterung schützt Azure Defender für Key Vault daher viele der Ressourcen, die von Ihren Key Vault-Konten abhängig sind.

Der optionale Plan ist nun allgemein verfügbar. Dieses Feature wurde in der Vorschau als „Advanced Threat Protection für Azure Key Vault“ bezeichnet.

Außerdem enthalten die Key Vault-Seiten im Azure-Portal nun eine dedizierte Seite **Sicherheit** für Empfehlungen und Warnungen von **Security Center** .

Weitere Informationen finden Sie unter [Azure Defender für Key Vault](defender-for-key-vault-introduction.md).


### <a name="azure-defender-for-storage-protection-for-files-and-adls-gen2-is-generally-available"></a>Azure Defender für Storage ist für Files und ADLS Gen2 allgemein verfügbar 

**Azure Defender für Storage** erkennt potenziell schädliche Aktivitäten in Ihren Azure Storage-Konten. Ihre Daten können geschützt werden, und zwar unabhängig davon, ob Sie als Blobcontainer, Dateifreigaben oder Data Lakes gespeichert werden.

Die Unterstützung für [Azure Files](../storage/files/storage-files-introduction.md) und [Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md) ist jetzt allgemein verfügbar.

Ab dem 1. Oktober 2020 beginnen wir damit, den Schutz von Ressourcen für diese Dienste abzurechnen.

Weitere Informationen finden Sie unter [Azure Defender für Storage](defender-for-storage-introduction.md).


### <a name="asset-inventory-tools-are-now-generally-available"></a>Asset Inventory-Tools sind jetzt allgemein verfügbar

Auf der Seite „Ressourcenbestand“ von Azure Security Center können Sie auf einer Seite den gesamten Sicherheitsstatus der Ressourcen anzeigen, die Sie mit Azure Security Center verbunden haben.

Azure Security Center analysiert in regelmäßigen Abständen den Sicherheitsstatus der Azure-Ressourcen, um mögliche Sicherheitsrisiken zu erkennen. Anschließend erhalten Sie Empfehlungen dazu, wie Sie diese Sicherheitsrisiken beheben können.

Wenn eine Ressource ausstehende Empfehlungen hat, werden diese im Inventar angezeigt.

Weitere Informationen finden Sie unter [Untersuchen und Verwalten Ihrer Ressourcen mit dem Ressourcenbestand und Verwaltungstools](asset-inventory.md).



### <a name="disable-a-specific-vulnerability-finding-for-scans-of-container-registries-and-virtual-machines"></a>Deaktivieren der Erkennung eines bestimmten Sicherheitsrisikos bei Scans von Containerregistrierungen und virtuellen Computern

Azure Defender enthält Sicherheitsrisikoscanner, mit denen Sie Images in Azure Container Registry und auf Ihren virtuellen Computern scannen können.

Wenn in Ihrer Organisation eine Suche ignoriert werden muss, anstatt sie zu beheben, können Sie sie optional deaktivieren. Deaktivierte Ergebnisse haben keine Auswirkung auf Ihre Sicherheitsbewertung und erzeugen kein unerwünschtes Rauschen.

Wenn eine Suche mit den in Ihren Deaktivierungsregeln definierten Kriterien übereinstimmt, wird sie nicht in der Liste der Ergebnisse angezeigt.

Diese Option ist auf den Detailseiten der Empfehlungen verfügbar:

- **Sicherheitsrisiken in Azure Container Registry-Images müssen behoben werden**
- **Sicherheitsrisiken für VMs müssen behoben werden**

Weitere Informationen finden Sie unter [Deaktivieren bestimmter Ergebnisse für Containerimages](defender-for-container-registries-usage.md#disable-specific-findings-preview) und [Deaktivieren bestimmter Ergebnisse für virtuelle Computer](remediate-vulnerability-findings-vm.md#disable-specific-findings-preview).


### <a name="exempt-a-resource-from-a-recommendation"></a>Ausschließen einer Ressource aus einer Empfehlung

Gelegentlich wird eine Ressource in Bezug auf eine bestimmte Empfehlung als fehlerhaft aufgeführt (und damit Ihre Sicherheitsbewertung gesenkt), obwohl Sie anderer Ansicht sind. Unter Umständen wurde dafür eine Lösungsmaßnahme mit einem Prozess durchgeführt, der von Security Center nicht nachverfolgt wird. Es kann auch sein, dass Ihre Organisation die Entscheidung getroffen hat, das Risiko für die entsprechende Ressource zu akzeptieren. 

In diesen Fällen können Sie eine Ausnahmeregel erstellen und sicherstellen, dass die Ressource in Zukunft nicht mehr in der Liste mit den fehlerhaften Ressourcen enthalten ist. Diese Regeln können dokumentierte Begründungen enthalten, wie unten beschrieben.

Weitere Informationen finden Sie unter [Ausschließen einer Ressource aus Empfehlungen und der Sicherheitsbewertung](exempt-resource.md).


### <a name="aws-and-gcp-connectors-in-security-center-bring-a-multi-cloud-experience"></a>AWS- und GCP-Connectors in Security Center für Szenarien mit mehreren Clouds

Cloudworkloads umfassen in der Regel mehrere Cloudplattformen, daher muss das auch für Cloudsicherheitsdienste gelten.

Azure Security Center schützt nun Workloads in Azure, Amazon Web Services (AWS) und Google Cloud Platform (GCP).

Beim Onboarding Ihrer AWS- und GCP-Konten in Security Center werden AWS Security Hub, GCP Security Command und Azure Security Center integriert. 

Weitere Informationen finden Sie unter [Herstellen einer Verbindung zwischen Ihren AWS-Konten und Azure Security Center](quickstart-onboard-aws.md) und [Herstellen einer Verbindung zwischen Ihren GCP-Konten und Azure Security Center](quickstart-onboard-gcp.md).


### <a name="kubernetes-workload-protection-recommendation-bundle"></a>Empfehlungsbündel für Kubernetes-Workloadschutz

Um sicherzustellen, dass Kubernetes-Workloads standardmäßig sicher sind, fügt Security Center Härtungsempfehlungen auf Kubernetes-Ebene hinzu, einschließlich Erzwingungsoptionen mit Kubernetes-Zugangskontrolle.

Wenn Sie das Azure Policy-Add-On für Kubernetes in Ihrem AKS-Cluster installiert haben, wird jede Anforderung an den Kubernetes-API-Server anhand der vordefinierten bewährten Methoden überwacht, bevor sie im Cluster persistent gespeichert wird. Anschließend können Sie das Erzwingen der bewährten Methoden konfigurieren und auf zukünftige Workloads anwenden.

Beispielsweise können Sie vorschreiben, dass keine privilegierten Container erstellt werden sollen und dass zukünftige Anforderungen für diese Aktion blockiert werden sollen.

Weitere Informationen finden Sie unter [Bewährte Methoden zum Schutz von Workloads mithilfe der Kubernetes-Zugangssteuerung](container-security.md#workload-protection-best-practices-using-kubernetes-admission-control).


### <a name="vulnerability-assessment-findings-are-now-available-in-continuous-export"></a>Ergebnisse der Sicherheitsrisikobewertung sind jetzt im fortlaufenden Export verfügbar

Verwenden Sie den fortlaufenden Export, um Warnungen und Empfehlungen in Echtzeit an Azure Event Hubs, Log Analytics-Arbeitsbereiche oder Azure Monitor zu streamen. Von dort aus können Sie diese Daten in SIEM-Lösungen integrieren (z. B. Azure Sentinel, Power BI, Azure Data Explorer usw.).

Die integrierten Tools für die Sicherheitsrisikobewertung von Security Center geben Ergebnisse zu Ihren Ressourcen als handlungsrelevante Empfehlungen innerhalb einer übergeordneten Empfehlung zurück, z. B. als „Sicherheitsrisiken für VMs müssen behoben werden“. 

Die sicherheitsrelevanten Ergebnisse sind jetzt über den fortlaufenden Export verfügbar, wenn Sie Empfehlungen auswählen und die Option **Sicherheitsrelevante Ergebnisse einbeziehen** aktivieren.

:::image type="content" source="./media/continuous-export/include-security-findings-toggle.png" alt-text="Die Empfehlung „Für Kubernetes-Dienste sollten autorisierte IP-Adressbereiche definiert werden“ mit einer Option zur schnellen Problembehebung" :::

Verwandte Seiten:

- [Integrierte Security Center-Lösung zur Sicherheitsrisikobewertung für virtuelle Computer](deploy-vulnerability-assessment-vm.md)
- [Integrierte Security Center-Lösung zur Sicherheitsrisikobewertung für Azure Container Registry-Images](defender-for-container-registries-usage.md)
- [Fortlaufendem Export](continuous-export.md)

### <a name="prevent-security-misconfigurations-by-enforcing-recommendations-when-creating-new-resources"></a>Verhindern von sicherheitsbezogenen Fehlkonfigurationen durch Erzwingen von Empfehlungen beim Erstellen neuer Ressourcen

Sicherheitsbezogene Fehlkonfigurationen sind eine häufige Ursache für Sicherheitsincidents. Security Center verfügt jetzt über ein Feature zum *Verhindern* von Fehlkonfigurationen neuer Ressourcen für bestimmte Empfehlungen. 

Dieses Feature kann dazu beitragen, dass Ihre Workloads geschützt sind und Ihre Sicherheitsbewertung stabil bleibt.

Für die Erzwingung einer sicheren Konfiguration basierend auf einer bestimmten Empfehlung gibt es zwei Modi:

- Mit dem Effekt **Deny** (Ablehnen) von Azure Policy können Sie verhindern, dass fehlerhafte Ressourcen erstellt werden.

- Mit der Option **Erzwingen** können Sie den Effekt **DeployIfNotExist** von Azure Policy nutzen und nicht konforme Ressourcen bei der Erstellung automatisch korrigieren.
 
Diese sind für ausgewählte Sicherheitsempfehlungen oben auf der Seite mit den Ressourcendetails verfügbar.

Weitere Informationen finden Sie unter [Verhindern von Fehlkonfigurationen mit den Optionen zum Erzwingen/Ablehnen für Empfehlungen](prevent-misconfigurations.md).

###  <a name="network-security-group-recommendations-improved"></a>Empfehlungen für Netzwerksicherheitsgruppen verbessert

Die folgenden Sicherheitsempfehlungen im Zusammenhang mit Netzwerksicherheitsgruppen wurden verbessert, um einige Arten von False Positives zu verringern.

- Alle Netzwerkports sollten auf NSGs eingeschränkt werden, die ihrer VM zugeordnet sind.
- Verwaltungsports sollten auf Ihren virtuellen Computern geschlossen werden.
- Virtuelle Computer mit Internetzugang sollten über Netzwerksicherheitsgruppen geschützt werden.
- Subnetze sollten einer Netzwerksicherheitsgruppe zugeordnet werden


### <a name="deprecated-preview-aks-recommendation-pod-security-policies-should-be-defined-on-kubernetes-services"></a>Veraltete AKS-Vorschauempfehlung „Für Kubernetes Service müssen Podsicherheitsrichtlinien definiert werden“

Die Vorschauempfehlung „Für Kubernetes Service müssen Podsicherheitsrichtlinien definiert werden“ ist veraltet. Eine Beschreibung finden Sie in der Dokumentation zu [Azure Kubernetes Service](../aks/use-pod-security-policies.md).

Das Feature Podsicherheitsrichtlinie (Vorschauversion) wird als veraltet eingestuft und steht nach dem 15. Oktober 2020 zugunsten von Azure Policy für AKS nicht mehr zur Verfügung.

Wenn die Podsicherheitsrichtlinie (Vorschauversion) veraltet ist, müssen Sie das Feature für alle vorhandenen Cluster deaktivieren, die das veraltete Feature verwenden, um zukünftige Clusterupgrades ausführen und weiterhin Azure-Support erhalten zu können.


### <a name="email-notifications-from-azure-security-center-improved"></a>E-Mail-Benachrichtigungen von Azure Security Center verbessert

Die folgenden Bereiche von E-Mails zu Sicherheitswarnungen wurden verbessert: 

- Möglichkeit zum Senden von E-Mail-Benachrichtigungen zu Warnungen für alle Schweregrade hinzugefügt
- Möglichkeit zum Benachrichtigen von Benutzern mit unterschiedlichen RBAC-Rollen für das Abonnement hinzugefügt
- Wir benachrichtigen standardmäßig Abonnementbesitzer bei Warnungen mit hohem Schweregrad (bei denen es sich mit hohen Wahrscheinlichkeit um echte Verstöße handelt).
- Das Feld für die Telefonnummer wurde von der Konfigurationsseite für E-Mail-Benachrichtigungen entfernt.

Weitere Informationen finden Sie unter [Einrichten von E-Mail-Benachrichtigungen für Sicherheitswarnungen](security-center-provide-security-contact-details.md).


### <a name="secure-score-doesnt-include-preview-recommendations"></a>Sicherheitsbewertung ohne Vorschauempfehlungen 

Security Center führt eine ständige Bewertung Ihrer Ressourcen, Abonnements und Organisation in Bezug auf Sicherheitsprobleme durch. Anschließend werden alle Ergebnisse in einer einzigen Bewertung zusammengefasst, sodass Sie auf einen Blick Ihre aktuelle Sicherheitssituation erkennen können: je höher die Bewertung, desto geringer das ermittelte Risiko.

Wenn neue Bedrohungen erkannt werden, werden neue Sicherheitsempfehlungen in Security Center über neue Empfehlungen verfügbar gemacht. Um unerwartete Änderungen an Ihrer Sicherheitsbewertung zu vermeiden und eine Toleranzperiode zu gewähren, in der Sie neue Empfehlungen erkunden können, bevor diese sich auf Ihre Bewertungen auswirken, werden Empfehlungen, die als **Vorschau** gekennzeichnet sind, nicht mehr in die Berechnungen Ihrer Sicherheitsbewertung einbezogen. Sie sollten nach Möglichkeit weiterhin korrigiert werden, damit sie nach Ablauf des Vorschauzeitraums zu Ihrer Bewertung beitragen.

Außerdem führen Empfehlungen in der **Vorschauphase** nicht dazu, dass eine Ressource als „Fehlerhaft“ gekennzeichnet wird.

Beispiel für eine Vorschauempfehlung:

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="Die Empfehlung „Für Kubernetes-Dienste sollten autorisierte IP-Adressbereiche definiert werden“ mit einer Option zur schnellen Problembehebung":::

[Weitere Informationen zur Sicherheitsbewertung.](secure-score-security-controls.md)


### <a name="recommendations-now-include-a-severity-indicator-and-the-freshness-interval"></a>Empfehlungen jetzt mit Schweregradindikator und Aktualisierungsintervall

Die Detailseite für Empfehlungen enthält jetzt einen Indikator für das Aktualisierungsintervall (sofern relevant) und eine deutliche Anzeige des Schweregrads der Empfehlung.

:::image type="content" source="./media/release-notes/recommendations-severity-freshness-indicators.png" alt-text="Die Empfehlung „Für Kubernetes-Dienste sollten autorisierte IP-Adressbereiche definiert werden“ mit einer Option zur schnellen Problembehebung":::



## <a name="august-2020"></a>August 2020

Updates im August:

- [Ressourcenbestand – leistungsstarke neue Ansicht des Sicherheitsstatus ihrer Ressourcen](#asset-inventory---powerful-new-view-of-the-security-posture-of-your-assets)
- [Unterstützung für Azure Active Directory-Sicherheitsstandards (für die mehrstufige Authentifizierung) hinzugefügt](#added-support-for-azure-active-directory-security-defaults-for-multi-factor-authentication)
- [Empfehlung zu Dienstprinzipale hinzugefügt](#service-principals-recommendation-added)
- [Sicherheitsrisikobewertung bei virtuellen Computern: Empfehlungen und Richtlinien wurden konsolidiert](#vulnerability-assessment-on-vms---recommendations-and-policies-consolidated)
- [Der Initiative ASC_default wurden neue AKS-Sicherheitsrichtlinien hinzugefügt – nur für Kunden der privaten Vorschau.](#new-aks-security-policies-added-to-asc_default-initiative--for-use-by-private-preview-customers-only)


### <a name="asset-inventory---powerful-new-view-of-the-security-posture-of-your-assets"></a>Ressourcenbestand – leistungsstarke neue Ansicht des Sicherheitsstatus ihrer Ressourcen

Der Ressourcenbestand in Security Center (derzeit in der Vorschauphase) bietet eine Möglichkeit, den Sicherheitsstatus der Ressourcen anzuzeigen, die Sie mit Security Center verbunden haben.

Azure Security Center analysiert in regelmäßigen Abständen den Sicherheitsstatus der Azure-Ressourcen, um mögliche Sicherheitsrisiken zu erkennen. Anschließend erhalten Sie Empfehlungen dazu, wie Sie diese Sicherheitsrisiken beheben können. Wenn eine Ressource ausstehende Empfehlungen hat, werden diese im Inventar angezeigt.

Sie können die Ansicht und ihre Filter verwenden, um Ihre Daten zum Sicherheitsstatus zu untersuchen und auf der Grundlage der Ergebnisse weitere Maßnahmen zu ergreifen.

Weitere Informationen: [Ressourcenbestand](asset-inventory.md).


### <a name="added-support-for-azure-active-directory-security-defaults-for-multi-factor-authentication"></a>Unterstützung für Azure Active Directory-Sicherheitsstandards (für die mehrstufige Authentifizierung) hinzugefügt

Azure Security Center bietet vollständige Unterstützung für [Sicherheitsstandards](../active-directory/fundamentals/concept-fundamentals-security-defaults.md), die kostenlosen Schutzfunktionen von Microsoft für die Identitätssicherheit.

Die Sicherheitsstandards bieten vorkonfigurierte Identitätssicherheitseinstellungen, um Ihre Organisation vor häufigen identitätsbezogenen Angriffen zu schützen. Sicherheitsstandards schützen bereits mehr als 5 Millionen Mandanten insgesamt; 50.000 Mandanten werden zusätzlich durch Azure Security Center geschützt.

Azure Security Center stellt jetzt eine Sicherheitsempfehlung bereit, sobald ein Azure-Abonnement ohne aktivierte Sicherheitsstandards identifiziert wird. Bisher empfahl Azure Security Center die Aktivierung der mehrstufigen Authentifizierung (MFA) mittels bedingtem Zugriff, was Teil der Lizenz von Azure Active Directory (AD) Premium ist. Für Kunden, die Azure AD Free verwenden, empfehlen wir nun, die Sicherheitsstandards zu aktivieren. 

Unser Ziel ist es, mehr Kunden zu ermutigen, ihre Cloudumgebungen mit MFA abzusichern und damit eines der höchsten Risiken zu mindern, das sich auch am stärksten auf ihre [Sicherheitsbewertung](secure-score-security-controls.md) auswirkt.

Weitere Informationen: [Sicherheitsstandards](../active-directory/fundamentals/concept-fundamentals-security-defaults.md).


### <a name="service-principals-recommendation-added"></a>Empfehlung zu Dienstprinzipale hinzugefügt

Es wurde eine neue Empfehlung hinzugefügt, die Azure Security Center-Kunden, die Verwaltungszertifikate zum Verwalten ihrer Abonnements verwenden, empfiehlt, zu Dienstprinzipalen zu wechseln.

Die Empfehlung, **Zum Schutz Ihrer Abonnements Dienstprinzipale anstelle von Verwaltungszertifikaten verwenden** besagt, für eine sicherere Verwaltung Ihrer Abonnements Dienstprinzipale oder Azure Resource Manager zu verwenden. 

Weitere Informationen: [Anwendungs- und Dienstprinzipalobjekten in Azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object).


### <a name="vulnerability-assessment-on-vms---recommendations-and-policies-consolidated"></a>Sicherheitsrisikobewertung bei virtuellen Computern: Empfehlungen und Richtlinien wurden konsolidiert

Security Center untersucht Ihre virtuellen Computer, um festzustellen, ob darauf eine Lösung zur Sicherheitsrisikobewertung ausgeführt wird. Wenn keine Lösung zur Sicherheitsrisikobewertung gefunden wird, gibt Security Center eine Empfehlung zur Vereinfachung der Bereitstellung.

Werden Sicherheitsrisiken gefunden, gibt Security Center eine Empfehlung, die die Ergebnisse zusammenfasst, damit Sie die Sicherheitsrisiken untersuchen und gegebenenfalls beheben können.

Um für alle Benutzer, unabhängig vom verwendeten Scannertyp, eine einheitliche Umgebung zu gewährleisten, haben wir vier Empfehlungen zu den folgenden beiden Empfehlungen vereinheitlicht:

|Vereinheitlichte Empfehlung|Änderungsbeschreibung|
|----|:----|
|**Auf Ihren virtuellen Computern muss eine Lösung zur Sicherheitsrisikobewertung installiert werden**|Ersetzt die folgenden beiden Empfehlungen:<br> **•** Integrierte Lösung zur Sicherheitsrisikobewertung auf virtuellen Computern aktivieren (unterstützt von Qualys – jetzt veraltet) (Empfehlung wird in Standard-Tarifen angezeigt)<br> **•** Die Lösung zur Sicherheitsrisikobewertung sollte auf Ihren virtuellen Computern installiert werden (jetzt veraltet) (Empfehlung wird sowohl in Standard- als auch Free-Tarifen angezeigt)|
|**Sicherheitsrisiken für VMs müssen behoben werden**|Ersetzt die folgenden beiden Empfehlungen:<br>**•** Auf Ihren virtuellen Computern gefundene Sicherheitsrisiken beheben (unterstützt von Qualys – jetzt veraltet)<br>**•** Sicherheitsrisiken sollten durch eine Lösung zur Sicherheitsrisikobewertung entschärft werden (unterstützt von Qualys – jetzt veraltet)|
|||

Nun wenden Sie dieselbe Empfehlung an, um die Security Center-Erweiterung für die Sicherheitsrisikobewertung oder eine privat lizenzierte Lösung („BYOL“) von einem Partner wie Qualys oder Rapid7 bereitzustellen.

Wurden Sicherheitsrisiken gefunden und an Security Center gemeldet, werden Sie unabhängig von der Lösung zur Sicherheitsrisikobewertung, die diese Sicherheitsrisiken identifiziert hat, mit einer einzigen Empfehlung auf die Ergebnisse aufmerksam gemacht.

#### <a name="updating-dependencies"></a>Aktualisieren von Abhängigkeiten

Wenn Sie über Skripts, Abfragen oder Automatisierungen verfügen, die sich auf die früheren Empfehlungen oder Richtlinienschlüssel/-namen beziehen, verwenden Sie die folgenden Tabellen, um die Verweise zu aktualisieren:

##### <a name="before-august-2020"></a>Vor August 2020

|Empfehlung|`Scope`|
|----|:----|
|**Integrierte Lösung zur Sicherheitsrisikobewertung (unterstützt von Qualys) auf virtuellen Computern aktivieren**<br>Schlüssel: 550e890b-e652-4d22-8274-60b3bdb24c63|Integriert|
|**Auf Ihren virtuellen Computern gefundene Sicherheitsrisiken beheben (unterstützt von Qualys)**<br>Schlüssel: 1195afff-c881-495e-9bc5-1486211ae03f|Integriert|
|**Die Lösung zur Sicherheitsrisikobewertung sollte auf Ihren virtuellen Computern installiert werden**<br>Schlüssel: 01b1ed4c-b733-4fee-b145-f23236e70cf3|BYOL|
|**Sicherheitsrisiken sollten durch eine Lösung zur Sicherheitsrisikobewertung beseitigt werden**<br>Schlüssel: 71992a2a-d168-42e0-b10e-6b45fa2ecddb|BYOL|
||||


|Richtlinie|`Scope`|
|----|:----|
|**Sicherheitsrisikobewertung für VMs muss aktiviert sein**<br>Richtlinien-ID: 501541f7-f7e7-4cd6-868c-4190fdad3ac9|Integriert|
|**Sicherheitsrisiken sollten durch eine Lösung zur Sicherheitsrisikobewertung entschärft werden**<br>Richtlinien-ID: 760a85ff-6162-42b3-8d70-698e268f648c|BYOL|
||||


##### <a name="from-august-2020"></a>Ab August 2020

|Empfehlung|`Scope`|
|----|:----|
|**Auf Ihren virtuellen Computern muss eine Lösung zur Sicherheitsrisikobewertung installiert werden**<br>Schlüssel: ffff0522-1e88-47fc-8382-2a80ba848f5d|Integriert + BYOL|
|**Sicherheitsrisiken für VMs müssen behoben werden**<br>Schlüssel: 1195afff-c881-495e-9bc5-1486211ae03f|Integriert + BYOL|
||||

|Richtlinie|`Scope`|
|----|:----|
|[**Sicherheitsrisikobewertung für virtuelle Computer muss aktiviert sein**](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f501541f7-f7e7-4cd6-868c-4190fdad3ac9)<br>Richtlinien-ID: 501541f7-f7e7-4cd6-868c-4190fdad3ac9 |Integriert + BYOL|
||||


### <a name="new-aks-security-policies-added-to-asc_default-initiative--for-use-by-private-preview-customers-only"></a>Der Initiative ASC_default wurden neue AKS-Sicherheitsrichtlinien hinzugefügt – nur für Kunden der privaten Vorschau.

Um sicherzustellen, dass Kubernetes-Workloads standardmäßig sicher sind, fügt Security Center Richtlinien auf Kubernetes-Ebene und Härtungsempfehlungen hinzu, einschließlich Erzwingungsoptionen mit Kubernetes-Zugangskontrolle.

Die frühe Phase dieses Projekts schließt eine private Vorschau und neue (standardmäßig deaktivierte) Richtlinien für die Initiative ASC_default ein.

Sie können diese Richtlinien gefahrlos und ohne Auswirkungen auf Ihre Umgebung ignorieren. Wenn Sie sie aktivieren möchten, registrieren Sie sich unter https://aka.ms/SecurityPrP für die Vorschauversion, und wählen Sie eine der folgenden Optionen aus:

1. **Einzelne Vorschau** , um nur dieser privaten Vorschau beizutreten. Nennen Sie „ASC Continuous Scan“ ausdrücklich als Vorschauversion, der Sie beitreten möchten.
1. **Fortlaufendes Programm** , um dieser und zukünftigen privaten Vorschauversionen hinzugefügt zu werden. Sie müssen ein Profil und eine Datenschutzvereinbarung ausfüllen.


## <a name="july-2020"></a>Juli 2020

Zu den Updates im Juli gehören:
- [Sicherheitsrisikobewertung für virtuelle Computer jetzt verfügbar für Nicht-Marketplace-Images](#vulnerability-assessment-for-virtual-machines-is-now-available-for-non-marketplace-images)
- [Erweiterung des Bedrohungsschutzes für Azure Storage durch Einbeziehen von Azure Files und Azure Data Lake Storage Gen2 (Vorschau)](#threat-protection-for-azure-storage-expanded-to-include-azure-files-and-azure-data-lake-storage-gen2-preview)
- [Acht neue Empfehlungen zum Aktivieren von Bedrohungsschutzfeatures](#eight-new-recommendations-to-enable-threat-protection-features)
- [Verbesserungen der Containersicherheit: schnellere Überprüfung der Registrierung und aktualisierte Dokumentation](#container-security-improvements---faster-registry-scanning-and-refreshed-documentation)
- [Neue Empfehlung zum Aktualisieren Ihrer Regeln für die adaptive Anwendungssteuerung](#adaptive-application-controls-updated-with-a-new-recommendation-and-support-for-wildcards-in-path-rules)
- [Sechs Richtlinien für SQL Advanced Data Security als veraltet markiert](#six-policies-for-sql-advanced-data-security-deprecated)




### <a name="vulnerability-assessment-for-virtual-machines-is-now-available-for-non-marketplace-images"></a>Sicherheitsrisikobewertung für virtuelle Computer jetzt verfügbar für Nicht-Marketplace-Images

Beim Bereitstellen einer Lösung zur Sicherheitsrisikobewertung führte Security Center bisher vorab eine Überprüfung durch. Die Überprüfung diente der Bestätigung einer Marketplace-SKU des virtuellen Zielcomputers. 

Ab diesem Update wurde die Überprüfung entfernt, und Sie können nun Tools zur Sicherheitsrisikobewertung auf benutzerdefinierten Windows- und Linux-Computern bereitstellen. Benutzerdefinierte Images sind solche, in denen Sie die Marketplace-Standardeinstellungen geändert haben.

Auch wenn Sie nun die integrierte Erweiterung für die Sicherheitsrisikobewertung (bereitgestellt durch Qualys) auf vielen weiteren Computern bereitstellen können, ist Support nur verfügbar, wenn Sie ein Betriebssystem verwenden, das unter [Bereitstellen der integrierten für Überprüfung auf Sicherheitsrisiken auf VMs im Standard-Tarif](deploy-vulnerability-assessment-vm.md#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines) aufgeführt ist.

Erfahren Sie mehr über die [integrierte Lösung für die Überprüfung auf Sicherheitsrisiken für virtuelle Computer (erfordert Azure Defender)](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner).

Erfahren Sie mehr über das Verwenden Ihrer eigenen privat lizenzierten Lösung zur Sicherheitsrisikobewertung von Qualys oder Rapid7 unter [Bereitstellen einer Partnerlösung zur Überprüfung auf Sicherheitsrisiken](deploy-vulnerability-assessment-vm.md).


### <a name="threat-protection-for-azure-storage-expanded-to-include-azure-files-and-azure-data-lake-storage-gen2-preview"></a>Erweiterung des Bedrohungsschutzes für Azure Storage durch Einbeziehen von Azure Files und Azure Data Lake Storage Gen2 (Vorschau)

Der Bedrohungsschutz für Azure Storage erkennt potenziell schädliche Aktivitäten in Ihren Azure Storage-Konten. Security Center zeigt Warnungen an, wenn Versuche erkannt werden, auf Ihre Speicherkonten zuzugreifen oder diese auszunutzen. 

Ihre Daten können geschützt werden, und zwar unabhängig davon, ob Sie als Blobcontainer, Dateifreigaben oder Data Lakes gespeichert werden.




### <a name="eight-new-recommendations-to-enable-threat-protection-features"></a>Acht neue Empfehlungen zum Aktivieren von Bedrohungsschutzfeatures

Es wurden acht neue Empfehlungen hinzugefügt, um eine einfache Möglichkeit zum Aktivieren der Bedrohungsschutzfeatures von Azure Security Center für die folgenden Ressourcentypen bereitzustellen: virtuelle Computer, App Service-Pläne, Azure SQL-Datenbank-Server, SQL-Server auf Computern, Azure Storage-Konten, Azure Kubernetes Service-Cluster, Azure Container Registry-Registrierungen und Azure Key Vault-Tresore.

Die neuen Empfehlungen sind:

- **Advanced Data Security muss für Azure SQL-Datenbank-Server aktiviert sein**
- **Advanced Data Security muss für SQL Server-Instanzen auf Computern aktiviert sein.**
- **Advanced Threat Protection muss in Azure App Service-Plänen aktiviert sein.**
- **Advanced Threat Protection muss in Azure Container Registry-Instanzen aktiviert sein**
- **Advanced Threat Protection muss in Azure Key Vault-Instanzen aktiviert sein**
- **Advanced Threat Protection muss in Azure Kubernetes Service-Clustern aktiviert sein**
- **Advanced Threat Protection muss für Azure Storage-Konten aktiviert sein**
- **Advanced Threat Protection muss für virtuelle Computer aktiviert sein.**

Diese neuen Empfehlungen gehören zur Sicherheitssteuerung **Azure Defender aktivieren** .

Die Empfehlungen schließen auch die Möglichkeit schneller Korrekturen ein. 

> [!IMPORTANT]
> Wenn Sie eine dieser Empfehlungen umsetzen, fallen Gebühren für den Schutz der relevanten Ressourcen an. Diese Kosten beginnen sofort, wenn Sie über zugehörige Ressourcen im aktuellen Abonnement verfügen. Das gilt auch, wenn Sie sie zu einem späteren Zeitpunkt hinzufügen.
> 
> Wenn Sie z. B. nicht über Azure Kubernetes Service-Cluster in Ihrem Abonnement verfügen und den Bedrohungsschutz aktivieren, fallen keine Gebühren an. Wenn Sie in Zukunft einen Cluster im selben Abonnement hinzufügen, wird dieser automatisch geschützt, und die Gebühren beginnen ab diesem Zeitpunkt.

Weitere Informationen zu den einzelnen Angaben finden Sie auf der [Referenzseite zu Sicherheitsempfehlungen](recommendations-reference.md).

Erfahren Sie mehr über [Bedrohungsschutz in Azure Security Center](azure-defender.md).




### <a name="container-security-improvements---faster-registry-scanning-and-refreshed-documentation"></a>Verbesserungen bei der Containersicherheit: schnellere Überprüfung der Registrierung und aktualisierte Dokumentation

Im Rahmen der kontinuierlichen Investitionen im Bereich Containersicherheit geben wir gerne eine bedeutende Leistungsverbesserung im Security Center-Feature für dynamische Scans von Containerimages in Azure Container Registry weiter. Scans werden nun in der Regel in ungefähr zwei Minuten abgeschlossen. In einigen Fällen kann es bis zu 15 Minuten dauern.

Zur Verbesserung der Klarheit und der Beschreibungen zu den Sicherheitsfunktionen von Azure Security Center haben wir auch die Dokumentationsseiten für die Containersicherheit aktualisiert. 

Weitere Informationen zur Containersicherheit in Security Center finden Sie in den folgenden Artikeln:

- [Übersicht über die Containersicherheitsfeatures von Security Center](container-security.md)
- [Ausführliche Informationen zur Integration mit Azure Container Registry](defender-for-container-registries-introduction.md)
- [Ausführliche Informationen zur Integration mit Azure Kubernetes Service](defender-for-kubernetes-introduction.md)
- [Überprüfen Ihrer Registrierungen und Härten Ihrer Docker-Hosts](container-security.md)
- [Sicherheitswarnungen von den Features zum Schutz vor Bedrohungen für Azure Kubernetes Service-Cluster](alerts-reference.md#alerts-akscluster)
- [Sicherheitswarnungen von den Features zum Schutz vor Bedrohungen für Azure Kubernetes Service-Hosts](alerts-reference.md#alerts-containerhost)
- [Sicherheitsempfehlungen für Container](recommendations-reference.md#recs-containers)



### <a name="adaptive-application-controls-updated-with-a-new-recommendation-and-support-for-wildcards-in-path-rules"></a>Neue Empfehlung zum Aktualisieren Ihrer Regeln für die adaptive Anwendungssteuerung

Das Feature für die adaptive Anwendungssteuerung hat zwei bedeutende Updates erhalten:

* Eine neue Empfehlung identifiziert potenziell legitimes Verhalten, das bisher noch nicht zulässig war. Die neue Empfehlung **Zulassungslistenregeln in der Richtlinie für die adaptive Anwendungssteuerung müssen aktualisiert werden** fordert Sie auf, der vorhandenen Richtlinie neue Regeln hinzuzufügen, um die Anzahl der falsch positiven Ergebnisse bei adaptiven Warnungen zu Anwendungssteuerungen zu verringern.

* Pfadregeln unterstützen jetzt Platzhalter. Mit diesem Update können Sie zulässige Pfadregeln mithilfe von Platzhaltern konfigurieren. Es werden zwei Szenarios unterstützt:

    * Verwenden eines Platzhalters am Ende eines Pfads, um alle ausführbaren Dateien in diesem Ordner und Unterordnern zuzulassen

    * Verwenden eines Platzhalters in der Mitte eines Pfads zum Aktivieren des Namens einer bekannten ausführbaren Datei mit einem sich ändernden Ordnernamen (z. B. persönliche Benutzerordner mit einer bekannten ausführbaren Datei, automatisch generierte Ordnernamen usw.)


[Weitere Informationen über Adaptive Anwendungssteuerungen](security-center-adaptive-application.md)



### <a name="six-policies-for-sql-advanced-data-security-deprecated"></a>Sechs Richtlinien für SQL Advanced Data Security als veraltet markiert

Sechs Richtlinien im Zusammenhang mit Advanced Data Security für SQL-Computer werden als veraltet markiert:

- Advanced Threat Protection-Typen müssen in den Advanced Data Security-Einstellungen von SQL Managed Instance auf „Alle“ festgelegt werden
- Advanced Threat Protection-Typen müssen in den Advanced Data Security-Einstellungen der SQL Server-Instanz auf „Alle“ festgelegt werden
- Advanced Data Security-Einstellungen für verwaltete SQL-Instanzen sollten eine E-Mail-Adresse für den Empfang von Sicherheitswarnungen enthalten.
- Advanced Data Security-Einstellungen für SQL Server sollten eine E-Mail-Adresse für den Empfang von Sicherheitswarnungen enthalten.
- In den Advanced Data Security-Einstellungen für die verwaltete SQL-Instanz müssen E-Mail-Benachrichtigungen an Administratoren und Abonnementbesitzer aktiviert sein
- E-Mail-Benachrichtigungen an Administratoren und Abonnementbesitzer sollten in den erweiterten Einstellungen für Datensicherheit in SQL Server aktiviert werden.

Weitere Informationen zu [integrierten Richtlinien](./policy-reference.md)





## <a name="june-2020"></a>Juni 2020

Zu den Updates im Juni gehören:
- [Sicherheitsbewertungs-API (Vorschau)](#secure-score-api-preview)
- [Erweiterte Datensicherheit für SQL-Computer (Azure, andere Clouds und lokal) (Vorschau)](#advanced-data-security-for-sql-machines-azure-other-clouds-and-on-prem-preview)
- [Zwei neue Empfehlungen zum Bereitstellen des Log Analytics-Agent auf Azure Arc-Computern (Vorschau)](#two-new-recommendations-to-deploy-the-log-analytics-agent-to-azure-arc-machines-preview)
- [Neue Richtlinien zum Erstellen von Konfigurationen für fortlaufenden Export und Workflowautomatisierung im großen Stil](#new-policies-to-create-continuous-export-and-workflow-automation-configurations-at-scale)
- [Neue Empfehlung zum Verwenden von NSGs zum Schützen von virtuellen Computern ohne Internetzugriff](#new-recommendation-for-using-nsgs-to-protect-non-internet-facing-virtual-machines)
- [Neue Richtlinien zum Aktivieren von Bedrohungsschutz und erweiterter Datensicherheit](#new-policies-for-enabling-threat-protection-and-advanced-data-security)



### <a name="secure-score-api-preview"></a>Sicherheitsbewertungs-API (Vorschau)

Sie können jetzt auf Ihre Bewertung über die [Sicherheitsbewertungs-API](/rest/api/securitycenter/securescores/) (derzeit in der Vorschau) zugreifen. Die API-Methoden bieten die Flexibilität, die Daten abzufragen und im Laufe der Zeit einen eigenen Berichtsmechanismus für Ihre Sicherheitsbewertungen zu erstellen. Beispielsweise können Sie die API **Sicherheitsbewertungen** verwenden, um die Bewertung für ein bestimmtes Abonnement zu erhalten. Darüber hinaus können Sie die API **Sicherheitsbewertungs-Steuerelemente** verwenden, um die Sicherheitssteuerelemente und die aktuelle Bewertung Ihrer Abonnements aufzulisten.

Beispiele für externe Tools, die mit der Sicherheitsbewertungs-API verwendet werden können, finden Sie im [Bereich zu den Sicherheitsbewertungen in unserer GitHub-Community](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score).

Weitere Informationen finden Sie unter [Erweiterter Secure Score (Vorschau) in Azure Security Center](secure-score-security-controls.md).



### <a name="advanced-data-security-for-sql-machines-azure-other-clouds-and-on-prem-preview"></a>Erweiterte Datensicherheit für SQL-Computer (Azure, andere Clouds und lokal) (Vorschau)

Advanced Data Security für SQL-Computer im Azure Security Center schützt jetzt in Azure, in anderen Cloudumgebungen und sogar auf lokalen Computern gehostete SQL-Server-Instanzen. Dies erweitert den Schutz für Ihre nativen Azure-SQL Server-Instanzen, um vollständige Unterstützung für Hybridumgebungen bereitzustellen.

Erweiterte Datensicherheit bietet eine Sicherheitsrisikobewertung und erweiterten Bedrohungsschutz für Ihre SQL-Computer, wo immer sie sich befinden.

Das Setup umfasst zwei Schritte:

1. Stellen Sie den Log Analytics-Agent auf dem Hostcomputer Ihrer SQL Server-Instanz bereit, um die Verbindung mit dem Azure-Konto herzustellen.

1. Aktivieren Sie das optionale Paket auf der Security Center-Seite „Preise und Einstellungen“.

Weitere Informationen finden Sie unter [Erweiterte Datensicherheit für SQL Server-Instanzen in Azure Virtual Machines (Vorschau)](defender-for-sql-usage.md).



### <a name="two-new-recommendations-to-deploy-the-log-analytics-agent-to-azure-arc-machines-preview"></a>Zwei neue Empfehlungen zum Bereitstellen des Log Analytics-Agent auf Azure Arc-Computern (Vorschau)

Es wurden zwei neue Empfehlungen hinzugefügt, die Ihnen helfen, den [Log Analytics-Agent](../azure-monitor/platform/log-analytics-agent.md) auf Ihren Azure Arc-Computern bereitzustellen und sicherzustellen, dass sie durch Azure Security Center geschützt sind:

- **Log Analytics-Agent muss auf Ihren Windows-basierten Azure Arc-Computern installiert sein (Vorschau)**
- **Log Analytics-Agent muss auf Ihren Linux-basierten Azure Arc-Computern installiert sein (Vorschau)**

Diese neuen Empfehlungen werden in denselben vier Sicherheitssteuerelementen angezeigt wie die vorhandene (verwandte) Empfehlung, **Monitoring Agent sollte auf ihren Computern installiert werden** : Sicherheitskonfigurationen reparieren, adaptive Anwendungssteuerung anwenden, Systemupdates anwenden und Endpunktschutz aktivieren.

Die Empfehlungen umfassen auch die Schnellkorrekturfunktion, um den Bereitstellungsprozess zu beschleunigen. 

Weitere Informationen zu diesen beiden neuen Empfehlungen finden Sie in der Tabelle [Compute- und App-Empfehlungen](recommendations-reference.md#recs-computeapp).

In [Was ist der Log Analytics-Agent?](faq-data-collection-agents.md#what-is-the-log-analytics-agent) erfahren Sie mehr darüber, wie Azure Security Center den Agent verwendet.

Erfahren Sie mehr über [Erweiterungen für Azure Arc-Computer](../azure-arc/servers/manage-vm-extensions.md).


### <a name="new-policies-to-create-continuous-export-and-workflow-automation-configurations-at-scale"></a>Neue Richtlinien zum Erstellen von Konfigurationen für fortlaufenden Export und Workflowautomatisierung im großen Stil

Die Automatisierung der Prozesse Ihrer Organisation zur Überwachung und Reaktion auf Vorfälle kann die Zeit, die zum Untersuchen von Sicherheitsvorfällen und zur Durchführung entsprechender Gegenmaßnahmen benötigt wird, erheblich verkürzen.

Zum Bereitstellen Ihrer Automatisierungskonfigurationen in Ihrer Organisation verwenden Sie diese integrierten „DeployIfdNotExist“-Azure-Richtlinien zum Erstellen und Konfigurieren von Prozeduren für [fortlaufenden Export](continuous-export.md) und [Workflowautomatisierung](workflow-automation.md):

Die Richtlinien finden Sie in der Azure-Richtlinie:


|Zielsetzung  |Richtlinie  |Richtlinien-ID  |
|---------|---------|---------|
|Fortlaufender Export zu einem Event Hub|[Bereitstellen eines Exports für Azure Security Center-Warnungen und -Empfehlungen in Event Hub](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fcdfcce10-4578-4ecd-9703-530938e4abcb)|cdfcce10-4578-4ecd-9703-530938e4abcb|
|Fortlaufender Export in einen Log Analytics-Arbeitsbereich|[Bereitstellen eines Exports für Azure Security Center-Warnungen und -Empfehlungen in Log Analytics-Arbeitsbereichen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fffb6f416-7bd2-4488-8828-56585fef2be9)|ffb6f416-7bd2-4488-8828-56585fef2be9|
|Workflowautomatisierung für Sicherheitswarnungen|[Bereitstellen der Workflowautomatisierung für Azure Security Center-Warnungen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2ff1525828-9a90-4fcf-be48-268cdd02361e)|f1525828-9a90-4fcf-be48-268cdd02361e|
|Workflowautomatisierung für Sicherheitsempfehlungen|[Bereitstellen der Workflowautomatisierung für Azure Security Center-Empfehlungen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f73d6ab6c-2475-4850-afd6-43795f3492ef)|73d6ab6c-2475-4850-afd6-43795f3492ef|
||||

Beginnen Sie mit [Vorlagen zur Workflowautomatisierung](https://github.com/Azure/Azure-Security-Center/tree/master/Workflow%20automation).

Weitere Informationen über die Verwendung der beiden Exportrichtlinien finden Sie unter [Konfigurieren der Workflowautomatisierung in großem Stile mit Hilfe der bereitgestellten Richtlinien](workflow-automation.md#configure-workflow-automation-at-scale-using-the-supplied-policies) und [Einrichten eines fortlaufenden Exports](continuous-export.md#set-up-a-continuous-export).


### <a name="new-recommendation-for-using-nsgs-to-protect-non-internet-facing-virtual-machines"></a>Neue Empfehlung zum Verwenden von NSGs zum Schützen von virtuellen Computern ohne Internetzugriff

Das Sicherheitssteuerelement „Bewährte Sicherheitsmethoden implementieren“ enthält jetzt die folgende neue Empfehlung:

- **Virtuelle Computer ohne Internetzugang sollten über Netzwerksicherheitsgruppen geschützt werden.**

Eine vorhandene Empfehlung **VMs mit Internetzugang sollten mithilfe von Netzwerksicherheitsgruppen geschützt werden** unterschied nicht zwischen virtuellen Computern mit und ohne Internetzugriff. Für beide wurde eine Empfehlung mit hohem Schweregrad generiert, wenn eine VM keiner Netzwerksicherheitsgruppe zugewiesen war. Diese neue Empfehlung separiert die Computer ohne Internetzugriff, um falsch positive Ergebnisse zu reduzieren und unnötige Warnungen mit hohem Schweregrad zu vermeiden.

Weitere Informationen finden Sie in der Tabelle [Netzwerkempfehlungen](recommendations-reference.md#recs-network).




### <a name="new-policies-for-enabling-threat-protection-and-advanced-data-security"></a>Neue Richtlinien zum Aktivieren von Bedrohungsschutz und erweiterter Datensicherheit

Die folgenden neuen Richtlinien wurden der ASC-Standardinitiative hinzugefügt und dienen zur Unterstützung beim Aktivieren von Bedrohungsschutz oder erweiterter Datensicherheit für die relevanten Ressourcentypen.

Die Richtlinien finden Sie in der Azure-Richtlinie:


| Richtlinie                                                                                                                                                                                                                                                                | Richtlinien-ID                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| [Für Azure SQL-Datenbank-Server muss Advanced Data Security aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f7fe3b40f-802b-4cdd-8bd4-fd799c948cc2)     | 7fe3b40f-802b-4cdd-8bd4-fd799c948cc2 |
| [Advanced Data Security muss für SQL Server-Instanzen auf Computern aktiviert sein.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f6581d072-105e-4418-827f-bd446d56421b) | 6581d072-105e-4418-827f-bd446d56421b |
| [Advanced Threat Protection muss für Azure Storage-Konten aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f308fbb08-4ab8-4e67-9b29-592e93fb94fa)           | 308fbb08-4ab8-4e67-9b29-592e93fb94fa |
| [Advanced Threat Protection muss in Azure Key Vault-Instanzen aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f0e6763cc-5078-4e64-889d-ff4d9a839047)           | 0e6763cc-5078-4e64-889d-ff4d9a839047 |
| [Advanced Threat Protection muss in Azure App Service-Plänen aktiviert sein.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f2913021d-f2fd-4f3d-b958-22354e2bdbcb)                | 2913021d-f2fd-4f3d-b958-22354e2bdbcb |
| [Advanced Threat Protection muss in Azure Container Registry-Instanzen aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fc25d9a16-bc35-4e15-a7e5-9db606bf9ed4)   | c25d9a16-bc35-4e15-a7e5-9db606bf9ed4 |
| [Advanced Threat Protection muss in Azure Kubernetes Service-Clustern aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f523b5cd1-3e23-492f-a539-13118b6d1e3a)   | 523b5cdq1-3 23-492b5a539-13118b6d1e3a |
| [Advanced Threat Protection muss für Virtual Machines aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f4da35fc9-c9e7-4960-aec9-797fe7d9051d)           | 4da35fc9-c9e7-4960-aec9-797fe7d9051d |
|                                                                                                                                                                                                                                                                       |                                      |

Erfahren Sie mehr über [Bedrohungsschutz in Azure Security Center](azure-defender.md).





## <a name="may-2020"></a>Mai 2020

Zu den Updates im Mai gehören:
- [Regeln zur Warnungsunterdrückung (Vorschau)](#alert-suppression-rules-preview)
- [Sicherheitsrisikobewertung für virtuelle Computer ist jetzt allgemein verfügbar](#virtual-machine-vulnerability-assessment-is-now-generally-available)
- [Änderungen am JIT-VM-Zugriff (Just-In-Time)](#changes-to-just-in-time-jit-virtual-machine-vm-access)
- [Benutzerdefinierte Empfehlungen wurden in ein separates Sicherheitssteuerelement verschoben](#custom-recommendations-have-been-moved-to-a-separate-security-control)
- [Umschalter zum Anzeigen von Empfehlungen in Steuerelementen oder als flache Liste hinzugefügt](#toggle-added-to-view-recommendations-in-controls-or-as-a-flat-list)
- [Erweitertes Sicherheitssteuerelement „Bewährte Sicherheitsmethoden implementieren“](#expanded-security-control-implement-security-best-practices)
- [Benutzerdefinierte Richtlinien mit benutzerdefinierten Metadaten sind nun allgemein verfügbar](#custom-policies-with-custom-metadata-are-now-generally-available)
- [Migration der Funktionen für die Absturzabbildanalyse zur Erkennung dateiloser Angriffe](#crash-dump-analysis-capabilities-migrating-to-fileless-attack-detection)


### <a name="alert-suppression-rules-preview"></a>Regeln zur Warnungsunterdrückung (Vorschau)

Dieses neue Feature (derzeit in der Vorschauphase) trägt zur Reduzierung der „Warnungsmüdigkeit“ bei. Verwenden Sie Regeln, damit Warnungen, die auf harmlose Fehler hinweisen oder sich auf normale Aktivitäten Ihrer Organisation beziehen, automatisch ausgeblendet werden. Auf diese Weise können Sie sich auf die relevantesten Bedrohungen konzentrieren. 

Warnungen, die den aktivierten Unterdrückungsregeln entsprechen, werden zwar weiterhin generiert, aber ihr Status lautet „Geschlossen“. Der Status wird im Azure-Portal bzw. an dem Ort angezeigt, über den Sie auf die Security Center-Sicherheitswarnungen zugreifen.

Mit Unterdrückungsregeln werden die Kriterien definiert, nach denen Warnungen automatisch geschlossen werden sollen. Normalerweise verwenden Sie in folgenden Fällen eine Unterdrückungsregel:

- Unterdrücken von Warnungen, die Sie als falsch positiv identifiziert haben

- Unterdrücken von Warnungen, die zu oft ausgelöst werden und daher nicht nützlich sind

Weitere Informationen finden Sie unter [Unterdrücken von Warnungen aus dem Threat Protection-Modul von Azure Security Center](alerts-suppression-rules.md).


### <a name="virtual-machine-vulnerability-assessment-is-now-generally-available"></a>Sicherheitsrisikobewertung für virtuelle Computer ist jetzt allgemein verfügbar

Der Standardtarif von Security Center enthält jetzt eine integrierte Sicherheitsrisikobewertung, für die keine zusätzlichen Gebühren anfallen. Diese Erweiterung basiert auf Qualys, aber die Ergebnisse werden direkt an Security Center gemeldet. Sie benötigen keine Qualys-Lizenz und auch kein Qualys-Konto – alles erfolgt nahtlos innerhalb von Security Center.

Mit der neuen Lösung können Ihre virtuellen Computer fortlaufend gescannt werden, um Sicherheitsrisiken zu ermitteln, und die Ergebnisse werden in Security Center bereitgestellt. 

Verwenden Sie zum Bereitstellen der Lösung die neue Sicherheitsempfehlung:

„Integrierte Lösung zur Sicherheitsrisikobewertung (unterstützt von Qualys) auf virtuellen Computern aktivieren“

Informieren Sie sich über die [integrierte Sicherheitsrisikobewertung für virtuelle Computer von Security Center](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner).



### <a name="changes-to-just-in-time-jit-virtual-machine-vm-access"></a>Änderungen am JIT-VM-Zugriff (Just-In-Time)

Security Center enthält ein optionales Feature zum Schützen der Verwaltungsports Ihrer virtuellen Computer. Dies ist eine Verteidigungsmaßnahme gegen die häufigste Form von Brute-Force-Angriffen.

Mit diesem Update werden die folgenden Änderungen an diesem Feature vorgenommen:

- Die Empfehlung, in der Ihnen zur JIT-Aktivierung auf einer VM geraten wird, wurde umbenannt. „Just-In-Time-Netzwerkzugriffssteuerung auf virtuelle Computer anwenden“ wurde geändert in: „Verwaltungsports virtueller Computer müssen mit der Just-In-Time-Netzwerkzugriffssteuerung geschützt werden“.

- Die Empfehlung wird nur ausgelöst, wenn offene Verwaltungsports vorhanden sind.

Erfahren Sie mehr zum [Feature für JIT-Zugriff](security-center-just-in-time.md).


### <a name="custom-recommendations-have-been-moved-to-a-separate-security-control"></a>Benutzerdefinierte Empfehlungen wurden in ein separates Sicherheitssteuerelement verschoben

Eine Sicherheitssteuerung, die im Rahmen der erweiterten Sicherheitsbewertung eingeführt wurde, ist „Best Practices für die Sicherheit implementieren“. Alle benutzerdefinierten Empfehlungen, die für Ihre Abonnements erstellt wurden, wurden automatisch in dieses Steuerelement eingefügt. 

Um Ihnen die Suche nach Ihren benutzerdefinierten Empfehlungen zu erleichtern, haben wir diese in das dedizierte Sicherheitssteuerelement „Benutzerdefinierte Empfehlungen“ verschoben. Dieses Steuerelement wirkt sich nicht auf Ihre Sicherheitsbewertung aus.

Weitere Informationen zu Sicherheitssteuerelementen finden Sie unter [Erweiterte Sicherheitsbewertung (Vorschau) in Azure Security Center](secure-score-security-controls.md).


### <a name="toggle-added-to-view-recommendations-in-controls-or-as-a-flat-list"></a>Umschalter zum Anzeigen von Empfehlungen in Steuerelementen oder als flache Liste hinzugefügt

Bei Sicherheitssteuerelementen handelt es sich um logische Gruppen mit zusammengehörigen Sicherheitsempfehlungen. Diese spiegeln Ihre anfälligen Angriffsflächen wider. Eine Sicherheitskontrolle umfasst eine Reihe von Sicherheitsempfehlungen mit Anweisungen, mit denen Sie diese Empfehlungen implementieren können.

Um sofort zu sehen, wie gut jede einzelne Angriffsfläche in Ihrer Organisation geschützt ist, sehen Sie sich die Bewertungen für die einzelnen Sicherheitskontrollen an.

Ihre Empfehlungen werden standardmäßig in den Sicherheitssteuerelementen angezeigt. Ab diesem Update können Sie sie auch als Liste anzeigen. Verwenden Sie den neuen Umschalter, mit dem Sie „Nach Steuerelementen gruppieren“ können, um sie als einfache Liste anzuzeigen, die nach dem Integritätsstatus der betroffenen Ressourcen sortiert ist. Der Umschalter befindet sich oberhalb der Liste im Portal.

Die Sicherheitssteuerelemente – und dieser Umschalter – sind Teil der neuen Benutzeroberfläche für die Sicherheitsbewertung. Denken Sie daran, uns über das Portal Ihr Feedback hierzu zu senden.

Weitere Informationen zu Sicherheitssteuerelementen finden Sie unter [Erweiterte Sicherheitsbewertung (Vorschau) in Azure Security Center](secure-score-security-controls.md).

:::image type="content" source="./media/secure-score-security-controls/recommendations-group-by-toggle.gif" alt-text="Die Empfehlung „Für Kubernetes-Dienste sollten autorisierte IP-Adressbereiche definiert werden“ mit einer Option zur schnellen Problembehebung":::

### <a name="expanded-security-control-implement-security-best-practices"></a>Erweitertes Sicherheitssteuerelement „Bewährte Sicherheitsmethoden implementieren“ 

Eine Sicherheitssteuerung, die im Rahmen der erweiterten Sicherheitsbewertung eingeführt wurde, ist „Best Practices für die Sicherheit implementieren“. Wenn eine Empfehlung in diesem Steuerelement angeordnet ist, wirkt sie sich nicht auf die Sicherheitsbewertung aus. 

Mit diesem Update wurden drei Empfehlungen aus den Steuerelementen, in denen sie ursprünglich angeordnet waren, in dieses Steuerelement für die bewährte Vorgehensweise verschoben. Wir haben dies durchgeführt, weil wir festgestellt haben, dass das Risiko bei diesen drei Empfehlungen niedriger als anfänglich gedacht ist.

Außerdem wurden zwei neue Empfehlungen eingeführt und diesem Steuerelement hinzugefügt.

Die folgenden drei Empfehlungen wurden verschoben:

- **Für Konten mit Leseberechtigungen für Ihr Abonnement muss MFA aktiviert sein** (ursprünglich im Steuerelement „MFA aktivieren“ enthalten)
- **Externe Konten mit Leseberechtigungen müssen aus Ihrem Abonnement entfernt werden** (ursprünglich im Steuerelement „Zugriff und Berechtigungen verwalten“ enthalten)
- **Für Ihr Abonnement dürfen maximal 3 Besitzer festgelegt werden** (ursprünglich im Steuerelement „Zugriff und Berechtigungen verwalten“ enthalten)

Die beiden folgenden neuen Empfehlungen wurden dem Steuerelement hinzugefügt:

- **Die Erweiterung Guest Configuration sollte auf Windows-VMs installiert sein (Vorschau)** : [Guest Configuration von Azure Policy](../governance/policy/concepts/guest-configuration.md) ermöglicht auf virtuellen Computern einen Einblick in Server- und Anwendungseinstellungen (nur Windows).

- **Windows Defender Exploit Guard sollte auf Ihren Computern aktiviert sein (Vorschau)** : Für Windows Defender Exploit Guard wird der Guest Configuration-Agent von Azure Policy genutzt. Exploit Guard verfügt über vier Komponenten für die Absicherung von Geräten gegen viele verschiedene Angriffsvektoren und Blockierungsverhalten, mit denen bei Angriffen mit Schadsoftware häufig zu rechnen ist. Darüber hinaus ermöglichen diese Komponenten es Unternehmen, die richtige Balance zwischen Sicherheitsrisiken und Produktivitätsanforderungen zu finden (nur Windows).

Weitere Informationen zu Windows Defender Exploit Guard finden Sie unter [Erstellen und Bereitstellen einer Exploit Guard-Richtlinie](/mem/configmgr/protect/deploy-use/create-deploy-exploit-guard-policy).

Weitere Informationen zu Sicherheitssteuerelementen finden Sie unter [Erweiterte Sicherheitsbewertung (Vorschau)](secure-score-security-controls.md).



### <a name="custom-policies-with-custom-metadata-are-now-generally-available"></a>Benutzerdefinierte Richtlinien mit benutzerdefinierten Metadaten sind nun allgemein verfügbar

Benutzerdefinierte Richtlinien sind nun Bestandteil der Security Center-Oberfläche mit den Empfehlungen, der Sicherheitsbewertung und des Dashboards mit den Standards zur Einhaltung gesetzlicher Bestimmungen. Dieses Feature ist jetzt allgemein verfügbar und ermöglicht es Ihnen, die Abdeckung in Bezug auf die Sicherheitsbewertung Ihrer Organisation in Security Center zu erweitern. 

Erstellen Sie in Azure Policy eine benutzerdefinierte Initiative, und fügen Sie ihr Richtlinien hinzu. Führen Sie anschließend das Onboarding in Azure Security Center und die Visualisierung in Form von Empfehlungen durch.

Wir haben jetzt auch die Option zum Bearbeiten der benutzerdefinierten Metadaten für die Empfehlungen hinzugefügt. Zu den Metadatenoptionen zählen Schweregrad, Problembehandlungsschritte, Bedrohungsinformationen und mehr.  

Informieren Sie sich über das [Verbessern der benutzerdefinierten Empfehlungen mit ausführlichen Informationen](custom-security-policies.md#enhance-your-custom-recommendations-with-detailed-information).



### <a name="crash-dump-analysis-capabilities-migrating-to-fileless-attack-detection"></a>Migration der Funktionen für die Absturzabbildanalyse zur Erkennung dateiloser Angriffe 

Wir integrieren die Erkennungsfunktionen der Windows-Absturzabbildanalyse mit der [Erkennung dateiloser Angriffe](defender-for-servers-introduction.md#what-are-the-benefits-of-azure-defender-for-servers). Die Analyse zur Erkennung dateiloser Angriffe verfügt über verbesserte Versionen der folgenden Sicherheitswarnungen für Windows-Computer: „Erkennung einer Codeinjektion“, „Erkennung eines Windows-Maskerademoduls“, „Erkennung von Shellcode“ und „Verdächtiges Codesegment erkannt“.

Diese Umstellung hat u. a. folgende Vorteile:

- **Proaktive und rechtzeitige Erkennung von Schadsoftware** : Bei der Absturzabbildanalyse wurde gewartet, bis es zu einem Absturz gekommen ist, und erst dann wurde die Analyse durchgeführt, um bösartige Artefakte zu ermitteln. Bei der Erkennung von dateilosen Angriffen werden In-Memory-Bedrohungen proaktiv während der Ausführung identifiziert. 

- **Erweiterte Warnungen** : Die Sicherheitswarnungen bei der Erkennung von dateilosen Angriffen verfügen über erweiterte Elemente, die bei der Absturzabbildanalyse nicht vorhanden sind, z. B. Informationen zu aktiven Netzwerkverbindungen. 

- **Warnungsaggregation** : Wenn bei der Absturzabbildanalyse in einem Absturzabbild mehrere Angriffsmuster erkannt wurden, wurden mehrere Sicherheitswarnungen ausgelöst. Bei der Erkennung von dateilosen Angriffen werden alle identifizierten Angriffsmuster eines Prozesses in einer Warnung zusammengefasst, damit nicht mehrere Warnungen korreliert werden müssen.

- **Verringerung der Anforderungen in Ihrem Log Analytics-Arbeitsbereich** : Absturzabbilder mit potenziell vertraulichen Daten werden nicht mehr in Ihren Log Analytics-Arbeitsbereich hochgeladen.