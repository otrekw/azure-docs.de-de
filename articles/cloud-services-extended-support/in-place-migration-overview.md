---
title: Migrieren von Azure Cloud Services (klassisch) zu Azure Cloud Services (erweiterter Support)
description: Übersicht über die Migration von Cloud Services (klassisch) zu Cloud Services (erweiterter Support)
ms.topic: how-to
ms.service: cloud-services-extended-support
ms.subservice: classic-to-arm-migration
author: tanmaygore
ms.author: tagore
ms.reviewer: mimckitt
ms.date: 2/08/2021
ms.custom: ''
ms.openlocfilehash: 96315899f80d0bd02ac3d2108b7cd76876025218
ms.sourcegitcommit: b8995b7dafe6ee4b8c3c2b0c759b874dff74d96f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106286737"
---
# <a name="migrate-azure-cloud-services-classic-to-azure-cloud-services-extended-support"></a>Migrieren von Azure Cloud Services (klassisch) zu Azure Cloud Services (erweiterter Support)

Dieser Artikel bietet eine Übersicht über das plattformgestützte Migrationstool und dessen Verwendung zum Migrieren von [Azure Cloud Services (klassisch)](../cloud-services/cloud-services-choose-me.md) zu [Azure Cloud Services (erweiterter Support)](overview.md).

Das Migrationstool verwendet dieselben APIs und bietet dieselbe Funktionalität wie die [Migration von Virtual Machines (klassisch)](https://docs.microsoft.com/azure/virtual-machines/migration-classic-resource-manager-overview). 

> [!IMPORTANT]
> Die Migration von Cloud Services (klassisch) zu Cloud Services (erweiterter Support) mithilfe des Migrationstools befindet sich derzeit in der öffentlichen Vorschau. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Wenn Sie bei der Migration Unterstützung benötigen, finden Sie weitere Informationen in den folgenden Ressourcen: 

- [Microsoft Q&A (Fragen und Antworten):](https://docs.microsoft.com/answers/topics/azure-cloud-services-extended-support.html) Microsoft- und Communityunterstützung für die Migration.
- [Azure-Migrationsunterstützung:](https://ms.portal.azure.com/#create/Microsoft.Support/Parameters/%7B%22pesId%22:%22e79dcabe-5f77-3326-2112-74487e1e5f78%22,%22supportTopicId%22:%22fca528d2-48bd-7c9f-5806-ce5d5b1d226f%22%7D) Spezielles Supportteam zur technischen Unterstützung während der Migration. Kunden ohne technischen Support können die [kostenlose Supportfunktion](https://aka.ms/cs-migration-errors) verwenden, die speziell für diese Migration bereitgestellt wird.
- Wenn Ihr Unternehmen oder Ihre Organisation eine Partnerschaft mit Microsoft eingegangen ist oder mit Microsoft-Vertretern wie z. B. Cloud Solution Architects (CSAs) oder Technical Account Managers (TAMs) zusammenarbeitet, fragen Sie diese nach zusätzlichen Ressourcen für die Migration.
- Nehmen Sie an [dieser Umfrage](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR--AgudUMwJKgRGMO84rHQtUQzZYNklWUk4xOTFXVFBPOFdGOE85RUIwVC4u) teil, um Feedback bereitzustellen oder das Produktteam von Cloud Services (erweiterter Support) auf Probleme aufmerksam zu machen. 

## <a name="migration-benefits"></a>Vorteile der Migration
Die plattformgestützte Migration bietet die folgenden wesentlichen Vorteile:
- Die Migration wird vollständig von der Plattform orchestriert, wobei die gesamte Bereitstellung und die zugehörigen Ressourcen in Azure Resource Manager verschoben werden.
- Die Migration verursacht keine Downtime.
- Im Vergleich zu anderen Migrationspfaden verläuft die Migration durch das Minimieren manueller Aufgaben einfach und schnell. 
- Cloud Services-IP-Adresse und DNS-Bezeichnung werden im Rahmen der Migration beibehalten. 

Weitere Vorteile und Gründe für die Migration finden Sie unter [Cloud Services (erweiterter Support)](overview.md) und [Azure Resource Manager](../azure-resource-manager/management/overview.md). 

## <a name="setup-access-for-migration"></a>Einrichten des Zugriffs für die Migration

Um diese Migration auszuführen, müssen Sie als Co-Administrator für das Abonnement hinzugefügt werden und die erforderlichen Anbieter registrieren. 

1. Melden Sie sich beim Azure-Portal an.
3. Wählen Sie im Menü Hub die Option Abonnement aus. Wählen Sie Alle Dienste aus, falls die Option nicht angezeigt wird.
3. Suchen Sie den entsprechenden Abonnementeintrag, und prüfen Sie dann das Feld MEINE ROLLE. Für einen Co-Administrator muss der Wert „Kontoadministrator“ lauten. Sollten Sie keinen Co-Administrator hinzufügen können, lassen Sie sich von einem Dienstadministrator oder Co-Administrator für das Abonnement hinzufügen.

4. Registrieren Sie Ihr Abonnement über das [Portal](../azure-resource-manager/management/resource-providers-and-types.md#azure-portal), [PowerShell](../azure-resource-manager/management/resource-providers-and-types.md#azure-powershell) oder die [CLI](../azure-resource-manager/management/resource-providers-and-types.md#azure-cli) für den Namespace „Microsoft.ClassicInfrastructureMigration“.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate 
    ```
 
5. Registrieren Sie Ihr Abonnement über das [Portal](../azure-resource-manager/management/resource-providers-and-types.md#azure-portal), [PowerShell](../azure-resource-manager/management/resource-providers-and-types.md#azure-powershell) oder die [CLI](../azure-resource-manager/management/resource-providers-and-types.md#azure-cli) für das Vorschaufeature der Cloud Services-Migration.

    ```powershell
    Register-AzProviderFeature -FeatureName CloudServices -ProviderNamespace Microsoft.Compute 
    ```

6. Überprüfen Sie den Status Ihrer Registrierung. Die Registrierung kann einige Minuten dauern. 

    ```powershell
    Get-AzProviderFeature -FeatureName CloudServices -ProviderNamespace Microsoft.Compute 
    ```

## <a name="how-is-migration-for-cloud-services-classic-different-from-virtual-machines-classic"></a>Migrationsunterschiede zwischen Cloud Services (klassisch) und Virtual Machines (klassisch)
Azure Service Manager unterstützt zwei verschiedene Computeprodukte: [Azure Virtual Machines (klassisch)](https://docs.microsoft.com/previous-versions/azure/virtual-machines/windows/classic/tutorial-classic) und [Azure Cloud Services (klassisch)](../cloud-services/cloud-services-choose-me.md), auch als Web-/Workerrollen bezeichnet. Die beiden Produkte unterscheiden sich in Bezug auf den Bereitstellungstyp innerhalb des Clouddiensts. Azure Cloud Services (klassisch) verwendet einen Clouddienst, der Bereitstellungen mit Web-/Workerrollen enthält. Azure Virtual Machines (klassisch) verwendet einen Clouddienst, der Bereitstellungen mit IaaS-VMs enthält.

Die Liste unterstützter Szenarien unterscheidet sich zwischen Cloud Services (klassisch) und Virtual Machines (klassisch) aufgrund von Unterschieden bei den Bereitstellungstypen.

## <a name="migration-steps"></a>Schritte bei der Migration
 
Kunden können ihre Bereitstellungen von Cloud Services (klassisch) mithilfe derselben vier Vorgänge migrieren, die auch für die Migration von Virtual Machines (klassisch) verwendet werden. 

1. **Migration überprüfen**: Stellt sicher, dass die Migration nicht durch gängige, nicht unterstützte Szenarien verhindert wird.
2. **Migration vorbereiten**: Dupliziert die Ressourcenmetadaten in Azure Resource Manager. Alle Ressourcen werden für Erstellungs-, Aktualisierungs- und Löschvorgänge gesperrt, um sicherzustellen, dass die Ressourcenmetadaten zwischen Azure Service Manager und Azure Resource Manager synchronisiert sind. Alle Lesevorgänge funktionieren sowohl mit der API für Cloud Services (klassisch) als auch mit der API für Cloud Services (erweiterter Support).
3. **Migration abbrechen**: Entfernt Ressourcenmetadaten aus Azure Resource Manager. Entsperrt alle Ressourcen für Erstellungs-, Aktualisierungs- und Löschvorgänge.
4. **Commit für Migration ausführen**: Entfernt Ressourcenmetadaten aus Azure Service Manager. Entsperrt die Ressourcen für Erstellungs-, Aktualisierungs- und Löschvorgänge. Nach einem Commitversuch ist ein Abbruch nicht mehr zulässig.

>[!NOTE]
> Vorbereitung, Abbruch und Commit sind idempotent. Wenn Fehler auftreten, sollte sich das Problem durch einen Wiederholungsversuch beheben lassen.

:::image type="content" source="media/in-place-migration-1.png" alt-text="Abbildung mit dem Diagramm der bei der Migration durchzuführenden Schritte.":::

Weitere Informationen finden Sie unter [Übersicht über die plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](../virtual-machines/migration-classic-resource-manager-overview.md).

## <a name="supported-resources-and-features-available-for-migration-associated-with-cloud-services-classic"></a>Für die Migration von Cloud Services (klassisch) verfügbare unterstützte Ressourcen und Features
-   Speicherkonten
-   Virtuelle Netzwerke
-   Netzwerksicherheitsgruppen
-   Reservierte öffentliche IP-Adressen
-   Endpunkt-Zugriffssteuerungslisten
-   Benutzerdefinierte Routen
-   Interner Lastenausgleich
-   Zertifikatmigration zu Key Vault
-   Plug-Ins und Erweiterung (XML- und JSON-basiert)
-   Aufgaben beim Start/beim Beenden
-   Bereitstellungen mit beschleunigtem Netzwerkbetrieb
-   Bereitstellungen mit einzelnen oder mehreren Rollen
-   Load Balancer Basic
-   Eingabe, Instanzeingabe, interne Endpunkte
-   Dynamische öffentliche IP-Adressen
-   DNS-Name 
-   Regeln für den Netzwerkdatenverkehr
-   Hypernet-VNet

## <a name="supported-configurations--migration-scenarios"></a>Unterstützte Konfigurationen/Migrationsszenarien
Dies sind die wichtigsten Szenarien für Kombinationen aus Ressourcen, Features und Cloud Services. Diese Liste ist nicht vollständig.

| Dienst | Konfiguration | Kommentare | 
|---|---|---|
| [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/migrate-from-classic-vnet) | Virtuelle Netzwerke, die Azure Active Directory Domain Services enthalten | Ein virtuelles Netzwerk mit Clouddienstbereitstellung und Azure AD Domain Services wird unterstützt. Der Kunde muss Azure AD Domain Services zunächst separat migrieren und dann eine Migration des übrigen virtuellen Netzwerks nur mit der Clouddienstbereitstellung durchführen. |
| Clouddienst | Clouddienst mit einer Bereitstellung nur in einem einzigen Slot | Clouddienste mit einer Produktions- oder Stagingslotbereitstellung können migriert werden. |
| Clouddienst | Bereitstellung nicht in einem öffentlich sichtbaren virtuellen Netzwerk (VNet-Standardbereitstellung) | Ein Clouddienst kann sich in einem öffentlich sichtbaren virtuellen Netzwerk, in einem verborgenen virtuellen Netzwerk oder in gar keinem virtuellen Netzwerk befinden.  Clouddienste in einem verborgenen virtuellen Netzwerk und Clouddienste in einem öffentlich sichtbaren virtuellen Netzwerk werden für die Migration unterstützt. Der Kunde kann anhand der Überprüfungs-API ermitteln, ob eine Bereitstellung innerhalb eines Standard-VNet erfolgt, und somit feststellen, ob die Migration möglich ist. |
|Clouddienst | XML-Erweiterungen (BGInfo, Visual Studio Debugger, Web Deploy und Remotedebuggen) | Alle XML-Erweiterungen werden für die Migration unterstützt. 
| Virtual Network | Virtuelles Netzwerk mit mehreren Clouddiensten | Ein virtuelles Netzwerk mit mehreren Clouddiensten wird für die Migration unterstützt. Das virtuelle Netzwerk und alle darin enthaltenen Clouddienste werden zu Azure Resource Manager migriert. |
| Virtual Network | Migration virtueller Netzwerke, die über das Portal erstellt wurden (erfordert die Verwendung von „Group Resource-group-name VNet-Name“ in der CSCFG-Datei)  | Im Rahmen der Migration wird der VNet-Name in der CSCFG-Datei in die Azure Resource Manager-ID des virtuellen Netzwerks geändert. (subscription/subscription-id/resource-group/resource-group-name/resource/vnet-name) <br><br>Um die Bereitstellung nach der Migration zu verwalten, aktualisieren Sie die lokale Kopie der CSCFG-Datei so, dass sie die Azure Resource Manager-ID anstelle des VNet-Namens verwendet. <br><br>Eine CSCFG-Datei, die das alte Benennungsschema verwendet, führt bei der Überprüfung zu einem Fehler. 
| Virtual Network | Migration einer Bereitstellung mit Rollen in einem anderen Subnetz | Ein Clouddienst mit unterschiedlichen Rollen in verschiedenen Subnetzen wird für die Migration unterstützt. |
    

## <a name="resources-and-features-not-available-for-migration"></a>Für die Migration nicht verfügbare Ressourcen und Features
Dies sind die wichtigsten Szenarien für Kombinationen aus Ressourcen, Features und Cloud Services. Diese Liste ist nicht vollständig. 

| Resource | Nächste Schritte/Problemumgehung | 
|---|---|
| Regeln zur automatischen Skalierung | Die Migration wird durchlaufen, aber Regeln werden gelöscht. [Erstellen Sie die Regeln nach der Migration zu Cloud Services (erweiterter Support) neu](https://docs.microsoft.com/azure/cloud-services-extended-support/configure-scaling). | 
| Alerts | Die Migration wird durchlaufen, aber Warnungen werden gelöscht. [Erstellen Sie die Regeln nach der Migration zu Cloud Services (erweiterter Support) neu](https://docs.microsoft.com/azure/cloud-services-extended-support/enable-alerts). | 
| VPN Gateway | Entfernen Sie die VPN Gateway-Instanz, bevor Sie mit der Migration beginnen, und erstellen Sie sie nach Abschluss der Migration neu. | 
| ExpressRoute-Gateway (nur im gleichen Abonnement wie Virtual Network) | Entfernen Sie das ExpressRoute-Gateway, bevor Sie mit der Migration beginnen, und erstellen Sie es nach Abschluss der Migration neu. | 
| Kontingent  | Kontingente werden nicht migriert. Fordern Sie vor der Migration ein [neues Kontingent](https://docs.microsoft.com/azure/azure-resource-manager/templates/error-resource-quota#solution) für Azure Resource Manager an, damit die Überprüfung erfolgreich ist. | 
| Affinitätsgruppen | Wird nicht unterstützt. Entfernen Sie alle Affinitätsgruppen vor der Migration.  | 
| Virtuelle Netzwerke mit [VNet-Peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)| Bevor Sie ein virtuelles Netzwerk migrieren, das mittels Peering mit einem anderen virtuellen Netzwerk verbunden ist, löschen Sie das Peering, migrieren Sie das virtuelle Netzwerk zu Resource Manager, und erstellen Sie das Peering neu. Je nach Architektur kann dies zu Downtime führen. | 
| Virtuelle Netzwerke, die App Service-Umgebungen enthalten | Nicht unterstützt | 
| Virtuelle Netzwerke, die HDInsight-Dienste enthalten | Wird nicht unterstützt. 
| Virtuelle Netzwerke, die Azure API Management-Bereitstellungen enthalten | Wird nicht unterstützt. <br><br> Um das virtuelle Netzwerk zu migrieren, ändern Sie das virtuelle Netzwerk der API Management-Bereitstellung. Dies ist ein Vorgang ohne Downtime. | 
| Klassische ExpressRoute-Verbindungen | Wird nicht unterstützt. <br><br>Diese Verbindungen müssen vor Beginn der PaaS-Migration zu Azure Resource Manager migriert werden. Weitere Informationen finden Sie unter [Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell](../expressroute/expressroute-howto-move-arm.md). |  
| Rollenbasierte Access Control | Nach der Migration ändert sich der URI der Ressource von `Microsoft.ClassicCompute` in `Microsoft.Compute`. RBAC-Richtlinien müssen nach der Migration aktualisiert werden. | 
| Application Gateway | Nicht unterstützt. <br><br> Entfernen Sie die Application Gateway-Instanz, bevor Sie mit der Migration beginnen, und erstellen Sie sie nach Abschluss der Migration zu Azure Resource Manager neu. | 

## <a name="unsupported-configurations--migration-scenarios"></a>Nicht unterstützte Konfigurationen/Migrationsszenarien

| Konfiguration/Szenario  | Nächste Schritte/Problemumgehung | 
|---|---|
| Migration einiger älterer Bereitstellungen, die sich nicht in einem virtuellen Netzwerk befinden | Für einige Cloud Services-Bereitstellungen, die nicht in einem virtuellen Netzwerk enthalten sind, wird die Migration nicht unterstützt. <br><br> 1. Überprüfen Sie mithilfe der Überprüfungs-API, ob die Bereitstellung für die Migration geeignet ist. <br> 2. Wenn sie geeignet ist, werden die Bereitstellungen in Azure Resource Manager unter ein virtuelles Netzwerk mit dem Präfix „DefaultRdfeVnet“ verschoben. | 
| Migration von Bereitstellungen, die sowohl die Produktions- als auch die Stagingslotbereitstellung über dynamische IP-Adressen beinhalten | Bei der Migration eines Clouddiensts mit zwei Slots muss der Stagingslot gelöscht werden. Anschließend migrieren Sie den Produktionsslot als unabhängige Instanz von Cloud Services (erweiterter Support) zu Azure Resource Manager. Stellen Sie dann die Stagingumgebung als neue Instanz von Cloud Services (erweiterter Support) erneut bereit, und konfigurieren Sie sie als mit der ersten Instanz austauschbar. | 
| Migration von Bereitstellungen, die sowohl die Produktions- als auch die Stagingslotbereitstellung über reservierte IP-Adressen beinhalten | Wird nicht unterstützt. | 
| Migration einer Produktions- und Stagingbereitstellung in einem anderen virtuellen Netzwerk|Bei der Migration eines Clouddiensts mit zwei Slots muss der Stagingslot gelöscht werden. Anschließend migrieren Sie den Produktionsslot als unabhängige Instanz von Cloud Services (erweiterter Support) zu Azure Resource Manager. Eine neue Bereitstellung von Cloud Services (erweiterter Support) kann dann mit der migrierten Bereitstellung verknüpft werden. Die Eigenschaft „Austauschbar“ muss aktiviert sein. Bereitstellungsdateien der alten Stagingslotbereitstellung können wiederverwendet werden, um diese neue austauschbare Bereitstellung zu erstellen. | 
| Migration eines leeren Clouddiensts (Clouddienst ohne Bereitstellung) | Wird nicht unterstützt. | 
| Migration einer Bereitstellung mit dem Remotedesktop-Plug-In und den Remotedesktoperweiterungen | Option 1: Entfernen Sie das Remotedesktop-Plug-In vor der Migration. Dazu sind Änderungen an den Bereitstellungsdateien erforderlich. Die Migration wird dann erfolgreich durchlaufen. <br><br> Option 2: Entfernen Sie die Remotedesktoperweiterung, und migrieren Sie die Bereitstellung. Entfernen Sie nach der Migration das Plug-In, und installieren Sie die Erweiterung. Dazu sind Änderungen an den Bereitstellungsdateien erforderlich. <br><br> Entfernen Sie das Plug-In und die Erweiterung vor der Migration. [Plug-Ins werden für die Verwendung in Cloud Services (erweiterter Support) nicht empfohlen](https://docs.microsoft.com/azure/cloud-services-extended-support/deploy-prerequisite#required-service-definition-file-csdef-updates).| 
| Virtuelle Netzwerke mit PaaS- und IaaS-Bereitstellung |Nicht unterstützt <br><br> Verschieben Sie entweder die PaaS- oder die IaaS-Bereitstellungen in ein anderes virtuelles Netzwerk. Dies führt zu Downtime. | 
Clouddienstbereitstellungen mit älteren Rollengrößen (z. B. „Small“ oder „ExtraLarge“) | Die Migration wird ausgeführt, aber die Rollengrößen werden auf moderne Rollengrößen aktualisiert. Die Kosten oder SKU-Eigenschaften bleiben unverändert, und die VM wird für diese Änderung nicht neu gestartet. Aktualisieren Sie alle Bereitstellungsartefakte, damit sie auf diese neuen modernen Rollengrößen verweisen. Weitere Informationen finden Sie unter [Verfügbare VM-Größen](available-sizes.md).|
| Migration des Clouddiensts zu einem anderen virtuellen Netzwerk | Nicht unterstützt <br><br> 1. Verschieben Sie die Bereitstellung vor der Migration in ein anderes klassisches virtuelles Netzwerk. Dies führt zu Downtime. <br> 2. Migrieren Sie das neue virtuelle Netzwerk zu Azure Resource Manager. <br><br> oder <br><br> 1. Migrieren Sie das virtuelle Netzwerk zu Azure Resource Manager. <br>2. Verschieben Sie den Clouddienst in ein neues virtuelles Netzwerk. Dies führt zu Downtime. | 
| Clouddienst in einem virtuellen Netzwerk, aber ohne Zuweisung eines expliziten Subnetzes | Wird nicht unterstützt. Zur Entschärfung muss die Rolle in ein Subnetz verschoben werden. Hierfür ist ein Rollenneustart erforderlich (Downtime). | 


## <a name="post-migration-changes"></a>Änderungen nach der Migration
Die Bereitstellung von Cloud Services (klassisch) wird in eine Bereitstellung von Cloud Services (erweiterter Support) konvertiert. Weitere Informationen finden Sie in der [Dokumentation zu Cloud Services (erweiterter Support)](deploy-prerequisite.md).  

### <a name="changes-to-deployment-files"></a>Änderungen an Bereitstellungsdateien 

An der CSDEF- und der CSCFG-Datei des Kunden werden kleinere Änderungen vorgenommen, damit die Bereitstellungsdateien den Anforderungen von Azure Resource Manager und Cloud Services (erweiterter Support) entsprechen. Nach der Migration werden die neuen Bereitstellungsdateien abgerufen oder die vorhandenen Dateien aktualisiert. Dies ist für Aktualisierungs-/Löschvorgänge erforderlich.  

- Virtual Network verwendet die vollständige Azure Resource Manager-Ressourcen-ID anstelle des Ressourcennamens im Abschnitt „NetworkConfiguration“ der CSCFG-Datei. Beispielsweise `/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.Network/virtualNetworks/vnet-name`. Bei virtuellen Netzwerken, die derselben Ressourcengruppe wie der Clouddienst angehören, können Sie die CSCFG-Datei wieder auf die Verwendung des VNet-Namen zurücksetzen.  

- Klassische Größen wie „Small“, „Large“ und „ExtraLarge“ werden durch ihre neuen Größennamen ersetzt: Standard_A*. Die Größennamen müssen in der CSDEF-Datei in ihre neuen Namen geändert werden. Weitere Informationen finden Sie unter [Voraussetzungen für die Bereitstellung von Azure Cloud Services (erweiterter Support)](deploy-prerequisite.md#required-service-definition-file-csdef-updates).

- Verwenden Sie die Abruf-API, um die neueste Kopie der Bereitstellungsdateien zu erhalten. 
    - Rufen Sie die Vorlage über das [Portal](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal), [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-powershell#export-resource-groups-to-templates), die [CLI](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-cli#export-resource-groups-to-templates) oder die [REST-API](https://docs.microsoft.com/rest/api/resources/resourcegroups/exporttemplate) ab. 
    - Rufen Sie die CSDEF-Datei über [PowerShell](https://docs.microsoft.com/powershell/module/az.cloudservice/?view=azps-5.4.0#cloudservice&preserve-view=true) oder die [REST-API](https://docs.microsoft.com/rest/api/compute/cloudservices/rest-get-package) ab. 
    - Rufen Sie die CSCFG-Datei über [PowerShell](https://docs.microsoft.com/powershell/module/az.cloudservice/?view=azps-5.4.0#cloudservice&preserve-view=true) oder die [REST-API](https://docs.microsoft.com/rest/api/compute/cloudservices/rest-get-package) ab. 
    
 

### <a name="changes-to-customers-automation-cicd-pipeline-custom-scripts-custom-dashboards-custom-tooling-etc"></a>Änderungen z. B. an Automatisierung, CI/CD-Pipeline, benutzerdefinierten Skripts, benutzerdefinierten Dashboards oder benutzerdefinierten Tools von Kunden  

Kunden müssen ihre Tools und Automatisierungsfunktionen aktualisieren, um mit der Verwendung der neuen APIs/Befehle zum Verwalten ihrer Bereitstellung zu beginnen. Ein Kunde kann im Rahmen dieser Änderung problemlos neue Features und Funktionen von Azure Resource Manager/Cloud Services (erweiterter Support) übernehmen. 

- Änderungen an Ressourcen- und Ressourcengruppennamen nach der Migration
    - Im Rahmen der Migration werden beispielsweise die Namen einiger Ressourcen wie des Clouddiensts oder öffentliche IP-Adressen geändert. Diese Änderungen müssen möglicherweise vor der Aktualisierung des Clouddiensts in den Bereitstellungsdateien berücksichtigt werden. [Erfahren Sie mehr über die Änderung von Ressourcennamen](in-place-migration-technical-details.md#translation-of-resources-and-naming-convention-post-migration).  

- Erneutes Erstellen von Regeln und Richtlinien zum Verwalten und Skalieren von Clouddiensten 
    - [Regeln für die automatische Skalierung](configure-scaling.md) werden nicht migriert. Erstellen Sie die Regeln für die automatische Skalierung nach der Migration neu.  
    - [Warnungen](enable-alerts.md) werden nicht migriert. Erstellen Sie die Warnungen nach der Migration neu.
    - Der Schlüsseltresor wird ohne Zugriffsrichtlinien erstellt. [Erstellen Sie geeignete Richtlinien](../key-vault/general/assign-access-policy-portal.md) für den Schlüsseltresor, um Ihre Zertifikate anzuzeigen oder zu verwalten. Zertifikate werden unter den Einstellungen auf der Registerkarte „Geheimnisse“ angezeigt.

## <a name="next-steps"></a>Nächste Schritte
- [Übersicht über die plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](../virtual-machines/migration-classic-resource-manager-overview.md)
- Migrieren zu Cloud Services (erweiterter Support) über das [Azure-Portal](in-place-migration-portal.md)
- Migrieren zu Cloud Services (erweiterter Support) über [PowerShell](in-place-migration-powershell.md)
