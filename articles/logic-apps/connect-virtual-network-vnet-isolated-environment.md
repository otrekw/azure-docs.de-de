---
title: Herstellen einer Verbindung mit virtuellen Azure-Netzwerken mithilfe einer Integrationsdienstumgebung
description: Erstellen einer Integrationsdienstumgebung (ISE), die auf virtuelle Azure-Netzwerke (VNETs) von Azure Logic Apps zugreifen kann
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: 6683c1b78b0e7ecba162026708c83843e2c08180
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478888"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Herstellen einer Verbindung mit virtuellen Azure-Netzwerken in Azure Logic Apps mithilfe einer Integrationsdienstumgebung

Für Szenarios, in denen Ihre Logik-Apps und Integrationskonten Zugriff auf ein [virtuelles Azure-Netzwerk](../virtual-network/virtual-networks-overview.md) benötigen, erstellen Sie eine [*Integrationsdienstumgebung* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Eine Integrationsdienstumgebung ist eine isolierte Umgebung, die dedizierten Speicher und andere Ressourcen verwendet, die vom „globalen“ mehrinstanzfähigen Logic Apps-Dienst getrennt bleiben. Diese Trennung trägt auch dazu bei, jegliche Auswirkungen anderer Azure-Mandanten auf die Leistung Ihrer Apps zu verringern. Eine ISE stellt Ihnen außerdem Ihre eigenen statischen IP-Adressen bereit. Diese IP-Adressen sind gesondert von den statischen IP-Adressen, die von den Logik-Apps im öffentlichen, mehrinstanzenfähigen Dienst gemeinsam verwendet werden.

Bei der Erstellung einer ISE wird diese ISE von Azure in Ihr virtuelles Azure-Netzwerk *injiziert*, sodass dann der Logic Apps-Dienst in Ihrem virtuellen Netzwerk bereitgestellt wird. Wählen Sie Ihre Integrationsdienstumgebung beim Erstellen einer Logik-App oder eines Integrationskontos als Speicherort aus. Ihre Logik-App bzw. Ihr Integrationskonto kann dann direkt auf Ressourcen wie virtuelle Computer (VMs), Server, Systeme und Dienste in Ihrem virtuellen Netzwerk zugreifen.

![Auswählen der Integrationsdienstumgebung](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

> [!IMPORTANT]
> Damit Logik-Apps und Integrationskonten in einer ISE zusammenarbeiten können, muss jeweils *dieselbe ISE* als Speicherort genutzt werden.

Eine ISE weist auch höhere Grenzwerte für Laufzeiten, Speicheraufbewahrung, Durchsatz, Zeitlimits für HTTP-Anforderungen und -Antworten, Nachrichtengrößen und benutzerdefinierte Connectoranforderungen auf. Weitere Informationen finden Sie unter [Grenzwerte und Konfiguration für Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md). Weitere Informationen zu ISEs finden Sie unter [Zugreifen auf Ressourcen virtueller Azure-Netzwerke über Azure Logic Apps mit Integrationsdienstumgebungen (ISEs)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

In diesem Artikel wird gezeigt, wie die folgenden Aufgaben mithilfe des Azure-Portals ausgeführt werden:

* Aktivieren Sie den Zugriff für Ihre ISE.
* Erstellen Sie Ihre ISE.
* Fügen Sie Ihrer ISE zusätzliche Kapazität hinzu.

Sie können eine ISE auch erstellen, indem Sie das [Beispiel für die Azure Resource Manager-Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-integration-service-environment) verwenden, oder indem Sie die Logic Apps-REST-API verwenden, einschließlich der Einrichtung kundenseitig verwalteter Schlüssel:

* [Erstellen einer Integrationsdienstumgebung (Integration Service Environment, ISE) mithilfe der Logic Apps-REST-API](../logic-apps/create-integration-service-environment-rest-api.md)
* [Einrichten von kundenseitig verwalteten Schlüsseln zum Verschlüsseln von ruhenden Daten für Integrationsdienstumgebungen (Integration Service Environment, ISE) in Azure Logic Apps](../logic-apps/customer-managed-keys-integration-service-environment.md)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

  > [!IMPORTANT]
  > Für Logik-Apps, integrierte Trigger, integrierte Aktionen und Connectors, die in Ihrer ISE ausgeführt werden, gilt ein anderer als der nutzungsbasierte Tarif. Weitere Informationen zur Preisgestaltung und Abrechnung für ISEs finden Sie unter [Feststehendes Preismodell](../logic-apps/logic-apps-pricing.md#fixed-pricing). Eine Preisübersicht finden Sie unter [Logic Apps – Preise](../logic-apps/logic-apps-pricing.md).

* Ein [virtuelles Azure-Netzwerk](../virtual-network/virtual-networks-overview.md). Wenn Sie kein virtuelles Netzwerk besitzen, erfahren Sie, wie Sie [ein virtuelles Azure-Netzwerk erstellen](../virtual-network/quick-create-portal.md).

  * Ihr virtuelles Netzwerk muss vier *leere* Subnetze aufweisen, um Ressourcen in Ihrer ISE erstellen und bereitstellen zu können. Jedes Subnetz unterstützt eine andere Logic Apps-Komponente, die in Ihrer ISE verwendet wird. Sie können diese Subnetze im Voraus erstellen oder warten, bis Sie Ihre ISE erstellt haben, in der Sie Subnetze parallel erstellen können. Erfahren Sie mehr über die [Voraussetzungen für Subnetze](#create-subnet).

  * Subnetznamen müssen entweder mit einem alphabetischen Zeichen oder einem Unterstrich beginnen und können keins der folgenden Zeichen verwenden: `<`, `>`, `%`, `&`, `\\`, `?`, `/`. 
  
  * Wenn Sie die ISE über eine Azure Resource Manager-Vorlage bereitstellen möchten, stellen Sie zunächst sicher, dass Sie ein leeres Subnetz an „Microsoft.Logic/integrationServiceEnvironment“ delegieren. Sie müssen diese Delegierung nicht durchführen, wenn die Bereitstellung über das Azure-Portal erfolgt.

  * Stellen Sie sicher, dass Ihr virtuelles Netzwerk [den Zugriff für Ihre ISE ermöglicht](#enable-access), damit Ihre ISE ordnungsgemäß funktioniert und zugänglich ist.

  * Wenn Sie [ExpressRoute](../expressroute/expressroute-introduction.md) verwenden, das eine private Verbindung mit Microsoft Cloud Services bereitstellt, die vom Konnektivitätsanbieter genutzt wird, müssen Sie [eine Routentabelle](../virtual-network/manage-route-table.md) erstellen, die die folgende Route enthält, und diese Tabelle mit jedem von Ihrer ISE verwendeten Subnetz verknüpfen:

    **Name**: <*Routenname*><br>
    **Adresspräfix**: 0.0.0.0/0<br>
    **Nächster Hop:** Internet

* Wenn Sie benutzerdefinierte DNS-Server für Ihr virtuelles Azure-Netzwerk verwenden möchten, [richten Sie diese Server gemäß dieser Schritte ein](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md), bevor Sie Ihre ISE in Ihrem virtuellen Netzwerk bereitstellen. Weitere Informationen zum Verwalten von DNS-Servereinstellungen finden Sie unter [Erstellen, Ändern oder Löschen eines virtuellen Netzwerks](../virtual-network/manage-virtual-network.md#change-dns-servers).

  > [!NOTE]
  > Wenn Sie Ihren DNS-Server oder die DNS-Servereinstellungen ändern, müssen Sie die ISE neu starten, damit die ISE diese Änderungen übernehmen kann. Weitere Informationen finden Sie unter [Neustarten der ISE](../logic-apps/ise-manage-integration-service-environment.md#restart-ISE).

<a name="enable-access"></a>

## <a name="enable-access-for-ise"></a>Aktivieren des Zugriffs für die ISE

Wenn Sie eine ISE mit einem virtuellen Azure-Netzwerk verwenden, besteht ein häufiges Einrichtungsproblem darin, dass ein oder mehrere Ports blockiert sind. Die Connectors, die Sie zum Herstellen von Verbindungen zwischen Ihrer ISE und den Zielsystemen verwenden, können ebenfalls eigene Portanforderungen aufweisen. Wenn Sie z. B. über den FTP-Connector mit einem FTP-System kommunizieren, muss der Port, den Sie auf Ihrem FTP-System verwenden, verfügbar sein, z. B. Port 21 zum Senden von Befehlen.

Um sicherzustellen, dass Ihre ISE zugänglich ist und dass die Logik-Apps in dieser ISE über jedes Subnetz in Ihrem virtuellen Netzwerk kommunizieren können, [öffnen Sie die in dieser Tabelle für jedes Subnetz beschriebenen Ports](#network-ports-for-ise). Wenn erforderliche Ports nicht verfügbar sind, funktioniert Ihre ISE nicht ordnungsgemäß.

* Wenn Sie über mehrere ISE-Instanzen verfügen, die Zugriff auf andere Endpunkte mit IP-Einschränkungen benötigen, stellen Sie eine [Azure Firewall-Instanz](../firewall/overview.md) oder ein [virtuelles Netzwerkgerät](../virtual-network/virtual-networks-overview.md#filter-network-traffic) in Ihrem virtuellen Netzwerk bereit, und leiten Sie ausgehenden Datenverkehr über die Firewall oder das virtuelle Netzwerkgerät weiter. Sie können dann [eine einzelne, ausgehende, öffentliche, statische und vorhersagbare IP-Adresse einrichten](connect-virtual-network-vnet-set-up-single-ip-address.md), die alle ISE-Instanzen in Ihrem virtuellen Netzwerk verwenden können, um mit Zielsystemen zu kommunizieren. Auf diese Weise müssen Sie nicht für jede ISE an den Zielsystemen zusätzliche Firewallzugänge einrichten.

   > [!NOTE]
   > Sie können diesen Ansatz auch für eine einzelne ISE verwenden, wenn in Ihrem Szenario die Anzahl der IP-Adressen eingeschränkt werden muss, die Zugriff benötigen. Wägen Sie ab, ob die zusätzlichen Kosten für die Firewall oder das virtuelle Netzwerkgerät für Ihr Szenario sinnvoll sind. Erfahren Sie mehr über [Azure Firewall-Preise](https://azure.microsoft.com/pricing/details/azure-firewall/).

* Wenn Sie ein neues virtuelles Azure-Netzwerk und Subnetze ohne Einschränkungen erstellt haben, müssen Sie keine [Netzwerksicherheitsgruppen (NSG)](../virtual-network/security-overview.md#network-security-groups) in Ihrem virtuellen Netzwerk einrichten, um den Datenverkehr über Subnetze zu steuern.

* In einem vorhandenen virtuellen Netzwerk können Sie Netzwerksicherheitsgruppen *optional* einrichten, indem Sie den [Netzwerkdatenverkehr über Subnetze filtern](../virtual-network/tutorial-filter-network-traffic.md). Wenn Sie diese Route verwenden möchten, oder wenn Sie bereits NSGs verwenden, stellen Sie sicher, dass Sie die [in dieser Tabelle angegebenen Ports in dem virtuellen Netzwerk öffnen](#network-ports-for-ise), in dem Sie NSGs besitzen oder einrichten möchten.

  > [!NOTE]
  > Wenn Sie [NSG-Sicherheitsregeln](../virtual-network/security-overview.md#security-rules) verwenden, müssen Sie *sowohl* TCP- als auch UDP-Protokolle verwenden. NSG-Sicherheitsregeln beschreiben die Ports, die Sie für die IP-Adressen öffnen müssen, die Zugriff auf diese Ports benötigen. Stellen Sie sicher, dass alle Firewalls, Router oder andere Elemente, die zwischen diesen Endpunkten vorhanden sind, diese Ports auch für diese IP-Adressen zugänglich halten.

<a name="network-ports-for-ise"></a>

### <a name="network-ports-used-by-your-ise"></a>Von ihrer ISE verwendete Netzwerkports

In dieser Tabelle werden die Ports in Ihrem virtuellen Azure-Netzwerk beschrieben, die Ihre Integrationsdienstumgebung verwendet, und es wird angegeben wo diese Ports verwendet werden. Um die Komplexität beim Erstellen von Sicherheitsregeln zu reduzieren, stellen die [Diensttags](../virtual-network/service-tags-overview.md) in der Tabelle Gruppen von IP-Adresspräfixen für einen bestimmten Azure-Dienst dar.

> [!IMPORTANT]
> Quellports sind kurzlebig. Legen Sie sie also für alle Regeln auf `*` fest. Wo vermerkt, verweisen die interne ISE und die externe ISE auf den [Endpunkt, der bei der ISE-Erstellung ausgewählt ist](connect-virtual-network-vnet-isolated-environment.md#create-environment). Weitere Informationen finden Sie unter [Endpunktzugriff](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access). 

| Zweck | Direction | Zielports | Quelldiensttag | Zieldiensttag | Notizen |
|---------|-----------|-------------------|--------------------|-------------------------|-------|
| Kommunikation zwischen Subnetzen innerhalb Ihres virtuellen Netzwerks | Ein- und ausgehend | * | Der Adressraum für das virtuelle Netzwerk, in dem sich die Subnetze Ihrer ISE befinden. | Der Adressraum für das virtuelle Netzwerk, in dem sich die Subnetze Ihrer ISE befinden. | Erforderlich für die Übertragung von Datenverkehr *zwischen* den Subnetzen in Ihrem virtuellen Netzwerk. <p><p>**Wichtig**: Damit Datenverkehr zwischen den *Komponenten* in den einzelnen Subnetzen übertragen werden kann, müssen alle Ports innerhalb der einzelnen Subnetze geöffnet werden. |
| Kommunikation mit Ihrer Logik-App | Eingehend | 443 | Internes ISE: <br>VirtualNetwork <p><p>Externes ISE: <br>Internet <br>(siehe die Spalte **Hinweise**) | VirtualNetwork | Statt des Diensttags **Internet** können Sie die Quell-IP-Adresse des Computers oder Diensts angeben, der einen Anforderungstrigger oder Webhook in Ihrer Logik-App aufruft. <p><p>**Wichtig**: Wenn Sie diesen Port schließen oder blockieren, werden HTTP-Aufrufe für Logik-Apps mit Anforderungstriggern verhindert. |
| Ausführungsverlauf einer Logik-App | Eingehend | 443 | Internes ISE: <br>VirtualNetwork <p><p>Externes ISE: <br>Internet <br>(siehe die Spalte **Hinweise**) | VirtualNetwork | Statt des Diensttags **Internet** können Sie die Quell-IP-Adresse des Computers oder Diensts angeben, von dem aus Sie den Ausführungsverlauf Ihrer Logik-App anzeigen möchten. <p><p>**Wichtig**: Wenn Sie diesen Port schließen oder blockieren, können Sie den Ausführungsverlauf zwar anzeigen, aber Sie können weder Ein- noch Ausgaben für die Schritte im Ausführungsverlauf anzeigen. |
| Logic Apps-Designer: dynamische Eigenschaften | Eingehend | 454 | LogicAppsManagement | VirtualNetwork | Anforderungen werden von den [eingehenden](../logic-apps/logic-apps-limits-and-config.md#inbound) IP-Adressen des Logic Apps-Zugriffsendpunkts für diese Region gesendet. |
| Bereitstellen von Connectors | Eingehend | 454 | AzureConnectors | VirtualNetwork | Zum Bereitstellen und Aktualisieren von Connectors erforderlich. Wenn Sie diesen Port schließen oder blockieren, führt dies zu Fehlern bei ISE-Bereitstellungen, und es können keine Connectorupdates oder -fixes durchgeführt werden. |
| Überprüfen der Netzwerkintegrität | Eingehend | 454 | LogicApps | VirtualNetwork | Anforderungen werden von den [eingehenden](../logic-apps/logic-apps-limits-and-config.md#inbound) und [ausgehenden](../logic-apps/logic-apps-limits-and-config.md#outbound) IP-Adressen des Logic Apps-Zugriffsendpunkts für diese Region gesendet. |
| Abhängigkeit von der App Service-Verwaltung | Eingehend | 454, 455 | AppServiceManagement | VirtualNetwork | |
| Kommunikation über Azure Traffic Manager | Eingehend | Internes ISE: 454 <p><p>Externes ISE: 443 | AzureTrafficManager | VirtualNetwork | |
| API Management: Verwaltungsendpunkt | Eingehend | 3443 | APIManagement | VirtualNetwork | |
| Connectorrichtlinienbereitstellung | Eingehend | 3443 | APIManagement | VirtualNetwork | Zum Bereitstellen und Aktualisieren von Connectors erforderlich. Wenn Sie diesen Port schließen oder blockieren, führt dies zu Fehlern bei ISE-Bereitstellungen, und es können keine Connectorupdates oder -fixes durchgeführt werden. |
| Kommunikation von Ihrer Logik-App | Ausgehend | 80, 443 | VirtualNetwork | Variiert abhängig vom Ziel | Die Endpunkte für den externen Dienst, mit dem Ihre Logik-App kommunizieren muss. |
| Azure Active Directory | Ausgehend | 80, 443 | VirtualNetwork | AzureActiveDirectory | |
| Verbindungsverwaltung | Ausgehend | 443 | VirtualNetwork  | AppService | |
| Veröffentlichen von Diagnoseprotokollen und Metriken | Ausgehend | 443 | VirtualNetwork  | AzureMonitor | |
| Azure Storage-Abhängigkeit | Ausgehend | 80, 443, 445 | VirtualNetwork | Storage | |
| Azure SQL-Abhängigkeiten | Ausgehend | 1433 | VirtualNetwork | SQL | |
| Azure Resource Health | Ausgehend | 1886 | VirtualNetwork | AzureMonitor | Zum Veröffentlichen des Integritätsstatus in Resource Health erforderlich |
| Abhängigkeit von Richtlinie zum Anmelden bei Event Hub und Überwachungs-Agent | Ausgehend | 5672 | VirtualNetwork | EventHub | |
| Zugriff auf Azure Cache for Redis-Instanzen zwischen Rolleninstanzen | Eingehend <br>Ausgehend | 6379–6383 | VirtualNetwork | VirtualNetwork | Damit die ISE mit Azure Cache for Redis funktioniert, müssen Sie die [Ports für ausgehenden und eingehenden Datenverkehr öffnen, die in den häufig gestellten Fragen zu Azure Cache for Redis beschrieben werden](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Erstellen Ihrer Integrationsdienstumgebung

1. Geben Sie im Hauptsuchfeld von Azure im [Azure-Portal](https://portal.azure.com) `integration service environments` als Filter ein, und wählen Sie **Integrationsdienstumgebungen** aus.

   ![Suchen und Auswählen von „Integrationsdienstumgebungen“](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Wählen Sie im Bereich **Integrationsdienstumgebungen** den Befehl **Hinzufügen** aus.

   ![Suchen und Auswählen von „Integrationsdienstumgebungen“](./media/connect-virtual-network-vnet-isolated-environment/add-integration-service-environment.png)

1. Geben Sie diese Details für Ihre Umgebung an, und wählen Sie dann **Überprüfen und erstellen** aus, z. B.:

   ![Angeben von Umgebungsdetails](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Eigenschaft | Erforderlich | Wert | BESCHREIBUNG |
   |----------|----------|-------|-------------|
   | **Abonnement** | Ja | <*Name des Azure-Abonnements*> | Das für Ihre Umgebung zu verwendende Azure-Abonnement |
   | **Ressourcengruppe** | Ja | <*Name der Azure-Ressourcengruppe*> | Eine neue oder vorhandene Azure-Ressourcengruppe, in der Sie Ihre Umgebung erstellen möchten. |
   | **Name der Integrationsdienstumgebung** | Ja | <*Umgebungsname*> | Ihr ISE-Name, der nur Buchstaben, Ziffern, Bindestriche (`-`), Unterstriche (`_`) und Punkte (`.`) enthalten kann. |
   | **Location** | Ja | <*Azure-Datencenterregion*> | Die Azure-Datencenterregion, in der Sie Ihre Umgebung bereitstellen. |
   | **SKU** | Ja | **Premium** oder **Developer (keine SLA)** | Die ISE-SKU, die erstellt und verwendet werden soll. Informationen zu den Unterschieden zwischen diesen SKUs finden Sie unter [ISE-SKUs](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level). <p><p>**Wichtig**: Diese Option ist nur bei der ISE-Erstellung verfügbar und kann später nicht mehr geändert werden. |
   | **Zusätzliche Kapazität** | Premium: <br>Ja <p><p>Developer: <br>Nicht verfügbar | Premium: <br>0 bis 10 <p><p>Developer: <br>Nicht verfügbar | Die Anzahl der für diese ISE-Ressource zu verwendenden zusätzlichen Verarbeitungseinheiten. Weitere Informationen zum Hinzufügen von Kapazität nach dem Erstellen finden Sie im Abschnitt [Hinzufügen von ISE-Kapazität](../logic-apps/ise-manage-integration-service-environment.md#add-capacity). |
   | **Zugriffsendpunkt** | Ja | **Intern** oder **Extern** | Der für Ihre ISE zu verwendende Typ von Zugriffsendpunkten. Diese Endpunkte bestimmen, ob Anforderungs- oder Webhooktrigger für Logik-Apps in Ihrer ISE Aufrufe von außerhalb Ihres virtuellen Netzwerks empfangen können. <p><p>Ihre Auswahl hat auch Einfluss auf die Methode, mit der Sie Eingaben und Ausgaben im Ausführungsverlauf Ihrer Logik-App anzeigen und darauf zugreifen können. Weitere Informationen finden Sie unter [ISE-Endpunktzugriff](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access). <p><p>**Wichtig**: Diese Option ist nur bei der ISE-Erstellung verfügbar und kann später nicht mehr geändert werden. |
   | **Virtuelles Netzwerk** | Ja | <*Azure-virtual-network-name*> | Das virtuelle Azure-Netzwerk, in das Sie Ihre Umgebung einfügen möchten, damit Logik-Apps in dieser Umgebung auf Ihr virtuelles Netzwerk zugreifen können. Wenn Sie nicht über ein Netzwerk verfügen, [erstellen Sie zunächst ein virtuelles Azure-Netzwerk](../virtual-network/quick-create-portal.md). <p><p>**Wichtig**: Sie können diese Einfügung *nur* einmalig durchführen, wenn Sie Ihre ISE erstellen. |
   | **Subnetze** | Ja | <*subnet-resource-list*> | Für eine ISE sind vier *leere* Subnetze zum Erstellen und Bereitstellen von Ressourcen in Ihrer Umgebung erforderlich. Um jedes Subnetz zu erstellen, [führen Sie die Schritte unter dieser Tabelle aus](#create-subnet). |
   |||||

   <a name="create-subnet"></a>

   **Erstellen eines Subnetzes**

   Für Ihre ISE sind vier *leere* Subnetze erforderlich, die an keinen Dienst delegiert wurden, um eine Ressource in Ihrer Umgebung zu erstellen und bereitzustellen. Jedes Subnetz unterstützt eine andere Logic Apps-Komponente, die in Ihrer ISE verwendet wird. Sie können diese Subnetzadressen *nicht mehr ändern*, nachdem Sie Ihre Umgebung erstellt haben. Jedes Subnetz muss folgende Anforderungen erfüllen:

   * Der Name muss mit einem alphabetischen Zeichen oder mit einem Unterstrich beginnen (keine Zahlen) und darf keines der folgenden Zeichen enthalten: `<`, `>`, `%`, `&`, `\\`, `?`, `/`.

   * Das Format [Classless Inter-Domain Routing (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) und einen Class B-Adressraum

   * Es muss mindestens `/27` im Adressbereich verwenden, da jedes Subnetz *mindestens* 32 Adressen benötigt. Beispiel:

     * `10.0.0.0/28` hat nur 16 Adressen und ist somit zu klein, da 2<sup>(32 – 28)</sup> 2<sup>4</sup> oder 16 ist.

     * `10.0.0.0/27` hat 32 Adressen, da 2<sup>(32 – 27)</sup> 2<sup>5</sup> oder 32 ist.

     * `10.0.0.0/24` hat 256 Adressen, da 2<sup>(32 – 24)</sup> 2<sup>8</sup> oder 256 ist. Zusätzliche Adressen haben jedoch keine weiteren Vorteile.

     Weitere Informationen zum Berechnen der Adressen finden Sie unter [Übersicht für IPv4](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * Wenn Sie [ExpressRoute](../expressroute/expressroute-introduction.md) verwenden, müssen Sie [eine Routentabelle](../virtual-network/manage-route-table.md) erstellen, die die folgende Route enthält, und diese Tabelle mit jedem von Ihrer ISE verwendeten Subnetz verknüpfen:

     **Name**: <*Routenname*><br>
     **Adresspräfix**: 0.0.0.0/0<br>
     **Nächster Hop:** Internet

   1. Wählen Sie unter der Liste **Subnetze** den Eintrag **Subnetzkonfiguration verwalten** aus.

      ![Verwalten der Subnetzkonfiguration](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet-configuration.png)

   1. Wählen Sie im Bereich **Subnetze** den Eintrag **Subnetz** aus.

      ![Hinzufügen von vier leeren Subnetzen](./media/connect-virtual-network-vnet-isolated-environment/add-empty-subnets.png)

   1. Geben Sie im Bereich **Subnetz hinzufügen** diese Informationen an.

      * **Name**: Der Name für Ihr Subnetz.
      * **Adressbereich (CIDR-Block)** : Der Bereich Ihres Subnetzes in Ihrem virtuellen Netzwerk und im CIDR-Format

      ![Hinzufügen von Subnetzdetails](./media/connect-virtual-network-vnet-isolated-environment/provide-subnet-details.png)

   1. Wenn Sie fertig sind, wählen Sie **OK**.

   1. Wiederholen Sie diese Schritte für drei weitere Subnetze.

      > [!NOTE]
      > Wenn die Subnetze, die Sie zu erstellen versuchen, nicht gültig sind, wird im Azure-Portal eine Meldung angezeigt, der Fortschritt aber nicht behindert.

   Weitere Informationen zum Erstellen von Subnetzen finden Sie unter [Hinzufügen von Subnetzen virtueller Netzwerke](../virtual-network/virtual-network-manage-subnet.md).

1. Nachdem Azure Ihre ISE-Informationen erfolgreich überprüft hat, wählen Sie **Erstellen** aus, z. B.:

   ![Wählen Sie nach der erfolgreicher Überprüfung „Erstellen“ aus.](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure beginnt mit der Bereitstellung Ihrer Umgebung, die in der Regel innerhalb von zwei Stunden abgeschlossen wird. Gelegentlich kann die Bereitstellung bis zu vier Stunden dauern. Um den Bereitstellungsstatus zu überprüfen, wählen Sie in Ihrer Azure-Symbolleiste das Symbol „Benachrichtigungen“ aus, wodurch der Benachrichtigungsbereich geöffnet wird.

   ![Überprüfen des Bereitstellungsstatus](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Wenn die Bereitstellung erfolgreich abgeschlossen wurde, zeigt Azure diese Benachrichtigung an:

   ![Bereitstellung erfolgreich](./media/connect-virtual-network-vnet-isolated-environment/deployment-success-message.png)

   Beachten Sie andernfalls die Anweisungen auf dem Azure-Portal zur Problembehandlung bei der Bereitstellung.

   > [!NOTE]
   > Wenn die Bereitstellung fehlschlägt oder Sie Ihre ISE löschen, kann es bis zu einer Stunde dauern, bis Azure Ihre Subnetze freigibt. Daher müssen Sie möglicherweise warten, bevor Sie diese Subnetze in einer anderen ISE wiederverwenden können.
   >
   > Wenn Sie Ihr virtuelles Netzwerk löschen, dauert es in der Regel bis zu zwei Stunden, bis Azure Ihre Subnetze freigibt, dieser Vorgang kann aber auch länger dauern. 
   > Stellen Sie beim Löschen virtueller Netzwerke sicher, dass keine Ressourcen mehr verbunden sind. 
   > Beachten Sie hierzu die Anleitung [Löschen eines virtuellen Netzwerks](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

1. Um Ihre Umgebung anzuzeigen, wählen Sie **Zu Ressource wechseln** aus, wenn Azure nach Abschluss der Bereitstellung nicht automatisch zu Ihrer Umgebung wechselt.

1. Informationen zum Überprüfen der Netzwerkintegrität für Ihre ISE finden Sie unter [Verwalten Ihrer Integrationsdienstumgebung](../logic-apps/ise-manage-integration-service-environment.md#check-network-health).

1. Informationen zum Erstellen von Logik-Apps und anderen Artefakten in Ihrer ISE finden Sie unter [Hinzufügen von Ressourcen zu Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps](../logic-apps/add-artifacts-integration-service-environment-ise.md).

   > [!IMPORTANT]
   > Verwaltete ISE-Connectors, die nach dem Erstellen der ISE verfügbar sind, werden nicht automatisch in der Connectorauswahl im Logik-App-Designer aufgeführt. Bevor Sie diese ISE-Connectors verwenden können, müssen Sie sie [manuell Ihrer ISE hinzufügen und bereitstellen](../logic-apps/add-artifacts-integration-service-environment-ise.md#add-ise-connectors-environment), damit sie im Logik-App-Designer angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

* [Hinzufügen von Ressourcen zu Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [Verwalten von Integrationsdienstumgebungen](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)
* Erfahren Sie mehr über [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).
* Erfahren Sie mehr über die [Integration virtueller Netzwerke für Azure-Dienste](../virtual-network/virtual-network-for-azure-services.md).
