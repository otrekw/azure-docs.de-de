---
title: Abrechnungs- und Preismodelle
description: Übersicht über die Funktionsweise von Preis- und Abrechnungsmodellen in Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, azla
ms.topic: conceptual
ms.date: 06/10/2021
ms.openlocfilehash: 3b627abdf27907a5c0739e6dd7920932a3035ee7
ms.sourcegitcommit: e39ad7e8db27c97c8fb0d6afa322d4d135fd2066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111981695"
---
# <a name="pricing-and-billing-models-for-azure-logic-apps"></a>Preis- und Abrechnungsmodelle für Azure Logic Apps

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) unterstützt Sie dabei, automatisierte Integrationsworkflows zu erstellen und auszuführen, die in der Cloud abskaliert werden können. In diesem Artikel werden die Abrechnungs- und Preismodelle für Azure Logic Apps und die zugehörigen Ressourcen beschrieben. Eine Preisübersicht finden Sie unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps). Informationen zum Planen, Verwalten und Überwachen von Kosten finden Sie unter [Planen und Verwalten von Kosten für Azure Logic Apps](plan-manage-costs.md).

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-multi-tenant"></a>Nutzungspreise (mehrere Mandanten)

Für Logik-Apps, die in der öffentlichen, „globalen“, mehrinstanzenfähigen Azure Logic Apps-Umgebung ausgeführt werden, gilt ein nutzungsabhängiges Preismodell. Sie haben mehrere Optionen für die Erstellung dieser Logik-Apps, z. B.:

* Ressourcentyp „Logik-App (Verbrauch)“ im Azure-Portal
* Erweiterung „Azure Logic Apps (Verbrauch)“ in Visual Studio Code
* Erweiterung „Azure Logic Apps-Tools“ in Visual Studio
* ARM-Vorlage (Azure Resource Manager) mit dem Ressourcentyp `Microsoft.Logic`
* Azure CLI für Azure Logic Apps mit den `az logic`-Befehlen
* Azure PowerShell für Azure Logic Apps mit dem Modul `Az.LogicApp`
* REST-API für Azure Logic Apps
* [Automatisierungsaufgaben](create-automation-tasks-azure-resources.md) im Azure-Portal

Die Messung und die Abrechnung basieren auf den Trigger- und Aktionsausführungen in einem Logik-App-Workflow. Diese Ausführungen werden unabhängig davon gemessen und abgerechnet, ob der Workflow erfolgreich ausgeführt oder überhaupt instanziiert wird. Gehen wir beispielsweise davon aus, dass Ihre Automatisierungsaufgabe einen Abruftrigger verwendet, der regelmäßig ausgehende Aufrufe an einen Endpunkt sendet. Diese ausgehenden Anforderungen werden unabhängig davon gemessen und als Ausführung abgerechnet, ob der Trigger ausgelöst oder übersprungen wurde. Dies wirkt sich darauf aus, ob eine Workflowinstanz erstellt wird.

| Elemente | Beschreibung |
|-------|-------------|
| [Integrierte](../connectors/built-in.md) Trigger und Aktionen | Werden im Dienst Azure Logic Apps nativ ausgeführt und mit dem Preis von [**Aktionen** gemessen](https://azure.microsoft.com/pricing/details/logic-apps/). <p><p>Beispielsweise sind die Trigger „HTTP“ und „Anforderung“ integrierte Trigger, während die Aktionen „HTTP“ und „Antwort“ integrierte Aktionen sind. Daten-, Batch- und Variablenvorgänge sowie [Aktionen zum Steuern von Workflows](../connectors/built-in.md) wie Schleifen, Bedingungen, Schalter, parallele Verzweigungen usw. sind ebenfalls integrierte Aktionen. <p><p>**Hinweis**: Als monatlichen Bonus enthält der Verbrauchsplan mehrere Tausend integrierte Ausführungen, die kostenlos sind. |
| Trigger und Aktionen des Typs [Standardconnector](../connectors/managed.md) <p><p>Trigger und Aktionen des Typs [Benutzerdefinierter Connector](../connectors/apis-list.md#custom-apis-and-connectors) | Gemessen mit dem Preis für [Standardconnector](https://azure.microsoft.com/pricing/details/logic-apps/). |
| Trigger und Aktionen des Typs [Unternehmensconnector](../connectors/managed.md) | Gemessen mit dem Preis für [Unternehmensconnector](https://azure.microsoft.com/pricing/details/logic-apps/). In der Connectorvorschau werden Unternehmensconnectors jedoch mit dem Preis für [*Standardconnectors*](https://azure.microsoft.com/pricing/details/logic-apps/) abgerechnet. |
| Aktionen innerhalb von [Schleifen](logic-apps-control-flow-loops.md) | Jede Aktion, die in einer Schleife erfolgt, wird für jeden ausgeführten Schleifenzyklus gemessen. <p><p>Angenommen, Sie haben eine FOR EACH-Schleife mit Aktionen zum Verarbeiten einer Liste. Der Dienst Azure Logic Apps erfasst jede Aktion in dieser Schleife, indem die Anzahl der Listenelemente mit der Anzahl der Aktionen in der Schleife multipliziert wird. Anschließend wird die Aktion zum Starten der Schleife hinzugefügt. Daher lautet die Berechnung für eine Liste mit zehn Elementen (10 * 1) + 1, sodass sich 11 Aktionsausführungen ergeben. |
| Wiederholungsversuche | Um die grundlegendsten Ausnahmen und Fehler zu behandeln, können Sie, sofern unterstützt, eine [Wiederholungsrichtlinie](logic-apps-exception-handling.md#retry-policies) für Trigger und Aktionen einrichten. Diese Wiederholungen werden zusammen mit der ursprünglichen Anforderung zu Tarifen in Rechnung gestellt, die darauf basieren, ob der Trigger oder die Aktion den Typ „Integriert“, „Standard“ oder „Unternehmen“ hat. Beispielsweise werden für eine Aktion, die mit zwei Wiederholungen ausgeführt wird, drei Aktionsausführungen berechnet. |
| [Datenaufbewahrung und Speichernutzung](#data-retention) | Wird mit dem Preis für Datenaufbewahrung gemessen, den Sie auf der [Preisseite für Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/) unter der Tabelle **Preisdetails** finden. |
|||

Weitere Informationen finden Sie in der folgenden Dokumentation:

* [Anzeigen von Metriken für Ausführungen und Speichernutzung](plan-manage-costs.md#monitor-billing-metrics)
* [Grenzwerte in Azure Logic Apps](logic-apps-limits-and-config.md)

### <a name="not-metered"></a>Nicht gemessen

* Aktionen, die aufgrund nicht erfüllter Bedingungen übersprungen werden
* Aktionen, die nicht ausgeführt werden, weil die Logik-App vor Abschluss beendet wurde
* [Deaktivierte Logik-Apps](#disabled-apps)

### <a name="other-related-resources"></a>Andere zugehörige Ressourcen

Logik-Apps arbeiten mit anderen zugehörigen Ressourcen zusammen, z. B. mit Integrationskonten, lokalen Datengateways und Integrationsdienstumgebungen (ISEs). Weitere Informationen zu den Preisen dieser Ressourcen finden Sie in diesen Abschnitten weiter unten in diesem Thema:

* [Lokales Datengateway](#data-gateway)
* [Preismodell für Integrationskonten](#integration-accounts)
* [ISE-Preismodell](#fixed-pricing)

### <a name="tips-for-estimating-consumption-costs"></a>Tipps zum Schätzen der Nutzungskosten

Diese Tipps helfen Ihnen beim Schätzen genauerer Nutzungskosten:

* Berücksichtigen Sie die mögliche Anzahl von Nachrichten oder Ereignissen, die an einem beliebigen Tag eingehen, statt Ihre Berechnungen nur auf das Abrufintervall zu stützen.

* Wenn ein Ereignis oder eine Nachricht die Auslösekriterien erfüllt, versuchen viele Trigger sofort, alle anderen wartenden Ereignisse oder Nachrichten, die die Kriterien erfüllen, zu lesen. Dieses Verhalten bedeutet, dass auch bei Auswahl eines längeren Abrufintervalls der Trigger aufgrund der Anzahl der wartenden Ereignisse oder Nachrichten, die für das Starten von Workflows qualifiziert sind, ausgelöst wird. Zu den Triggern, die diesem Verhalten folgen, zählen Azure Service Bus und Azure Event Hub.

  Angenommen, Sie haben einen Trigger eingerichtet, der täglich einen Endpunkt überprüft. Wenn der Trigger den Endpunkt überprüft und 15 Ereignisse findet, die die Kriterien erfüllen, wird der Trigger ausgelöst und führt den entsprechenden Workflow 15 mal aus. Der Dienst Azure Logic Apps misst alle Aktionen, die diese 15 Workflows ausführen, einschließlich der Triggeranforderungen.

<a name="standard-pricing"></a>

## <a name="standard-pricing-single-tenant"></a>Standardpreise (einzelner Mandant)

Ein Preismodell, das auf einem Hostingplan und einem Tarif basiert, gilt für Logik-Apps, die in der Azure Logic Apps-Umgebung mit nur einem Mandanten ausgeführt werden. Diese Preise gelten für den Ressourcentyp **Logik-App (Standard)** im Azure-Portal oder für Logik-Apps, an denen Sie mit der Erweiterung **Azure Logic Apps (Standard)** für Visual Studio Code arbeiten. Beim Erstellen oder Bereitstellen einer Logik-App dieser Art müssen Sie einen Hostingplan und einen Tarif auswählen, um die Preise zu ermitteln, die beim Ausführen Ihrer Workflows für die Messung und Abrechnung genutzt werden sollen.

> [!NOTE]
> Für neue Logik-Apps, die Sie mit dem Ressourcentyp **Logik-App (Standard)** erstellen, müssen Sie den Hostingplan **Workflow (Standard)** verwenden. Der App Service-Plan und die App Service-Umgebung sind für neue Logik-Apps nicht verfügbar.

<a name="hosting-plans"></a>

### <a name="pricing-tiers-and-billing-rates"></a>Tarife und Abrechnungssätze

Jeder Tarif in einem Hostingplan umfasst eine bestimmte Menge an Compute-, Arbeitsspeicher- und Speicherressourcen. Informationen zu den Stundensätzen pro Ressource und Region finden Sie auf der [Seite mit den Preisen für Azure Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/).

Um Ihnen das Verständnis der Preise zu erleichtern, enthält dieses Beispiel Schätzwerte für die *Region „USA, Osten 2“* .

* Wählen Sie auf der [Seite mit den Preisen für Azure Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/) die Region **USA, Osten 2** aus, um die Stundensätze anzuzeigen, oder sehen Sie sich die folgende Tabelle an:

  | Resource | USD pro Stunde (USA, Osten 2) |
  |----------|------------------------|
  | **Virtuelle CPU (vCPU)** | 0,192 USD |
  | **Arbeitsspeicher** | 0,0137 USD pro GB |
  |||

* Basierend auf den obigen Informationen werden in dieser Tabelle die geschätzten Monatssätze für die einzelnen Tarife und die darin enthaltenen Ressourcen angezeigt:

  | Tarif | USD pro Monat (USA, Osten 2) | Virtuelle CPU (vCPU) | Arbeitsspeicher (GB) | Speicher (GB) |
  |--------------|-------------------------|--------------------|-------------|--------------|
  | **WS1** | 175,20 USD | 1 | 3,5 | 250 |
  | **WS2** | 350,40 USD | 2 | 7 | 250 |
  | **WS3** | 700,80 USD | 4 | 14 | 250 |
  ||||||

* Basierend auf den obigen Informationen werden in dieser Tabelle die einzelnen Ressourcen und der geschätzte Monatssatz angezeigt, wenn Sie den Tarif **WS1** auswählen:

  | Resource | Amount (Betrag) | USD pro Monat (USA, Osten 2) |
  |----------|--------|-------------------------|
  | **Virtuelle CPU (vCPU)** | 1 | 140,16 USD |
  | **Arbeitsspeicher** | 3,5 GB | 35,04 USD |
  ||||

<a name="storage-transactions"></a>

### <a name="storage-transactions"></a>Speichertransaktionen

Azure Logic Apps verwendet [Azure Storage](../storage/index.yml) für alle Speichervorgänge. In Azure Logic Apps für mehrere Mandanten werden die gesamte Speichernutzung und alle Speicherkosten an die Logik-App gebunden. Mit Azure Logic Apps mit einem einzelnen Mandanten können Sie Ihr eigenes Azure-[Speicherkonto](../azure-functions/storage-considerations.md#storage-account-requirements) verwenden. Diese Funktion bietet Ihnen mehr Kontrolle und Flexibilität für Ihre Logic Apps-Daten.

Wenn *zustandsbehaftete* Workflows ihre Vorgänge ausführen, führt die Azure Logic Apps-Runtime Speichertransaktionen durch. Beispielsweise werden Warteschlangen für die Zeitplanung verwendet, während Tabellen und Blobs zum Speichern von Workflowzuständen verwendet werden. Die Speicherkosten ändern sich basierend auf dem Inhalt Ihres Workflows. Verschiedene Auslöser, Vorgänge und Nutzdaten führen zu unterschiedlichen Speichervorgängen und Anforderungen. Speichertransaktionen folgen dem [Azure Storage-Preismodell](https://azure.microsoft.com/pricing/details/storage/). Speicherkosten werden in Ihrer Azure-Rechnung separat aufgeführt.

### <a name="tips-for-estimating-storage-needs-and-costs"></a>Tipps zum Schätzen von Speicheranforderungen und -kosten

Um eine Vorstellung von der Anzahl der Speichervorgänge, die ein Workflow ausführen könnte, und deren Kosten zu erhalten, verwenden Sie den [Logic Apps-Speicherrechner](https://logicapps.azure.com/calculator). Sie können entweder einen Beispielworkflow auswählen oder eine vorhandene Workflowdefinition verwenden. Die erste Berechnung schätzt die Anzahl der Speichervorgänge in Ihrem Workflow. Sie können diese Zahlen dann verwenden, um mögliche Kosten mithilfe des [Azure-Preisrechners](https://azure.microsoft.com/pricing/calculator/) zu schätzen.

Weitere Informationen finden Sie in der folgenden Dokumentation:

* [Abschätzen der Speicheranforderungen und -kosten für Workflows in Azure Logic Apps für nur einen Mandanten](estimate-storage-costs.md)
* [Azure Storage – Preisdetails](https://azure.microsoft.com/pricing/details/storage/)

<a name="fixed-pricing"></a>

## <a name="ise-pricing-dedicated"></a>ISE-Preise (dediziert)

Für Logik-Apps, die in der dedizierten [*Integrationsdienstumgebung* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) ausgeführt werden, gilt ein Festpreismodell. Eine ISE wird mit dem [Preis für Integrationsdienstumgebungen](https://azure.microsoft.com/pricing/details/logic-apps) abgerechnet, der von der von Ihnen erstellten [ISE-Stufe oder *SKU*](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) abhängt. Diese Preise unterscheiden sich von den Preisen für mehrere Mandanten, da Sie für reservierte Kapazität und dedizierte Ressourcen unabhängig davon zahlen, ob Sie diese nutzen oder nicht.

| ISE SKU | BESCHREIBUNG |
|---------|-------------|
| **Premium** | Die Basiseinheit bietet eine [feste Kapazität](logic-apps-limits-and-config.md#integration-service-environment-ise) und wird [für die SKU „Premium“ mit einem Stundensatz abgerechnet](https://azure.microsoft.com/pricing/details/logic-apps). Falls Sie mehr Durchsatz benötigen, können Sie bei der Erstellung Ihrer ISE oder nachträglich [weitere Skalierungseinheiten hinzufügen](../logic-apps/ise-manage-integration-service-environment.md#add-capacity). Jede Skalierungseinheit wird mit einem [Stundensatz abgerechnet, der etwa der Hälfte des Satzes der Basiseinheit entspricht](https://azure.microsoft.com/pricing/details/logic-apps). <p><p>Informationen zu Grenzwerten und Kapazitäten finden Sie unter [ISE-Grenzwerte in Azure Logic Apps](logic-apps-limits-and-config.md#integration-service-environment-ise). |
| **Developer** | Die Basiseinheit bietet eine [feste Kapazität](logic-apps-limits-and-config.md#integration-service-environment-ise) und wird [für die SKU „Developer“ mit einem Stundensatz abgerechnet](https://azure.microsoft.com/pricing/details/logic-apps). Diese SKU umfasst keine Vereinbarung zum Servicelevel (Service-Level Agreement, SLA), Funktion zum zentralen Hochskalieren der Kapazität oder Redundanz während der Wiederverwendung. Dies bedeutet, dass Verzögerungen oder Ausfallzeiten auftreten können. Der Dienst kann vorübergehend von Back-End-Updates unterbrochen werden. <p><p>**Wichtig**: Stellen Sie sicher, dass Sie diese SKU nur für Erkundung, Experimente, Entwicklung und Tests verwenden, nicht jedoch für die Produktion oder für Leistungstests. <p><p>Informationen zu Grenzwerten und Kapazitäten finden Sie unter [ISE-Grenzwerte in Azure Logic Apps](logic-apps-limits-and-config.md#integration-service-environment-ise). |
|||

### <a name="included-at-no-extra-cost"></a>Ohne zusätzliche Kosten inbegriffen

| Elemente | Beschreibung |
|-------|-------------|
| [Integrierte](../connectors/built-in.md) Trigger und Aktionen | Werden mit der Beschriftung **Core** angezeigt und in derselben ISE ausgeführt wie Ihre Logik-Apps. |
| [Standardconnectors](../connectors/managed.md) <p><p>[Unternehmensconnector](../connectors/managed.md#enterprise-connectors) | Verwaltete Connectors, die mit der Beschriftung **ISE** versehen sind, wurden speziell für den Betrieb ohne das lokale Datengateways entwickelt und werden in derselben ISE wie Ihre Logik-Apps ausgeführt. Die ISE-Preise decken so viele Unternehmensverbindungen wie gewünscht ab. <p><p>Connectors ohne die Beschriftung „ISE“ werden im Azure Logic Apps-Dienst für einen einzelnen Mandanten ausgeführt. Die ISE-Preise decken diese Ausführungen jedoch für in einer ISE ausgeführte Logik-Apps ab. |
| Aktionen innerhalb von [Schleifen](logic-apps-control-flow-loops.md) | Die ISE-Preise decken jede in einer Schleife ausgeführte Aktion für jeden ausgeführten Schleifenzyklus ab. <p><p>Angenommen, Sie haben eine FOR EACH-Schleife mit Aktionen zum Verarbeiten einer Liste. Zur Ermittlung der Gesamtanzahl der Ausführungen von Aktionen multiplizieren Sie die Anzahl der Listenelemente mit der Anzahl der Aktionen in der Schleife und addieren die Aktion zum Starten der Schleife. Daher lautet die Berechnung für eine Liste mit zehn Elementen (10 * 1) + 1, sodass sich 11 Aktionsausführungen ergeben. |
| Wiederholungsversuche | Um die grundlegendsten Ausnahmen und Fehler zu behandeln, können Sie, sofern unterstützt, eine [Wiederholungsrichtlinie](logic-apps-exception-handling.md#retry-policies) für Trigger und Aktionen einrichten. Die ISE-Preise berücksichtigen Wiederholungen ebenso wie die ursprüngliche Anforderung. |
| [Datenaufbewahrung und Speichernutzung](#data-retention) | Für Logik-Apps in einer ISE fallen keine Gebühren für die Datenaufbewahrung und Speichernutzung an. |
| [Integrationskonten](#integration-accounts) | Schließt die Nutzung eines Einzeltarifs für Integrationskonten auf Basis der ISE SKU ohne zusätzliche Kosten ein. |
|||

Informationen zu Grenzwerten finden Sie unter [ISE-Grenzwerte in Azure Logic Apps](logic-apps-limits-and-config.md#integration-service-environment-ise).

<a name="integration-accounts"></a>

## <a name="integration-accounts"></a>Integrationskonten

Ein [Integrationskonto](../logic-apps/logic-apps-pricing.md#integration-accounts) ist eine gesonderte Ressource, die Sie erstellen und mit einer Logik-App verknüpfen. Es dient zum Erkunden, Erstellen und Testen von B2B-Integrationslösungen, in denen Funktionen für [EDI](logic-apps-enterprise-integration-b2b.md) und die [XML-Verarbeitung](logic-apps-enterprise-integration-xml.md) genutzt werden.

Für Azure Logic Apps werden diese Integrationskontoebenen bzw. -tarife angeboten, die sich in Bezug auf die [Preise](https://azure.microsoft.com/pricing/details/logic-apps/) und das [Abrechnungsmodell](logic-apps-pricing.md#integration-accounts) unterscheiden können. Dies richtet sich danach, ob Ihre Logik-App unter Azure Logic Apps (mit mehreren oder nur einem Mandanten) oder in einer ISE ausgeführt wird.

| Tarif | BESCHREIBUNG |
|------|-------------|
| **Basic** | Für Szenarien, in denen Sie nur die Verarbeitung von Nachrichten nutzen oder als Partner eines kleines Unternehmens fungieren möchten, das eine Handelspartnerbeziehung mit einem größeren Unternehmen eingegangen ist. <p><p>Ist von der SLA für Logic Apps abgedeckt. |
| **Standard** | Für Szenarien mit komplexeren B2B-Beziehungen und einer größeren Anzahl zu verwaltender Entitäten. <p><p>Ist von der SLA für Logic Apps abgedeckt. |
| **Free** | Nur für Erkundungs- und nicht für Produktionsszenarien. Bei diesem Tarif gelten Einschränkungen für Region, Durchsatz und Nutzung. Der Free-Tarif ist beispielsweise nur für öffentliche Regionen in Azure verfügbar, z. B. USA, Westen oder Asien, Südosten, aber nicht für [Azure China 21ViaNet](/azure/china/overview-operations) oder [Azure Government](../azure-government/documentation-government-welcome.md). <p><p>**Hinweis**: Ist von der SLA für Logic Apps nicht abgedeckt. |
|||

Informationen zu den Grenzwerten für Integrationskonten finden Sie unter [Grenzwerte und Konfiguration für Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits) wie z. B.:

* [Grenzwerte für Integrationskonten pro Azure-Abonnement](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits)

* [Grenzwerte für verschiedene Artefakte pro Integrationskonto](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits). Artefakte umfassen Handelspartner, Vereinbarungen, Zuordnungen, Schemas, Assemblys, Zertifikate, Batchkonfigurationen usw.

### <a name="multi-tenant-or-single-tenant-based-logic-apps"></a>Logik-Apps mit mehreren oder nur einem Mandanten

Integrationskonten werden mit einem [Festpreis für Integrationskonten](https://azure.microsoft.com/pricing/details/logic-apps/) abgerechnet, der sich nach dem von Ihnen genutzten Kontotarif richtet.

### <a name="ise-based-logic-apps"></a>ISE-basierte Logik-Apps

Ohne Mehrkosten gehört zu Ihrer ISE ein einzelnes Integrationskonto, das auf Ihrer ISE SKU basiert. Gegen Mehrkosten können Sie weitere Integrationskonten für Ihre ISE anlegen, die Sie bis zur [ISE-Obergrenze](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits) nutzen können. Weitere Informationen zum [ISE-Preismodell](#fixed-pricing) finden Sie weiter oben in diesem Thema.

| ISE SKU | Integrationskonto inbegriffen | Zusätzliche Kosten |
|---------|------------------------------|-----------------|
| **Premium** | Einzelnes Integrationskonto im Tarif [Standard](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) | Bis zu 19 weitere Standard-Konten. Es sind keine Free- oder Basic-Konten zulässig. |
| **Developer** | Einzelnes Integrationskonto im Tarif [Free](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) | Bis zu 19 weitere Standard-Konten, wenn Sie bereits über ein Free-Konto verfügen, oder 20 Standardkonten, wenn Sie kein Free-Konto besitzen. Es sind keine Basic-Konten zulässig. |
||||

<a name="data-retention"></a>

## <a name="data-retention-and-storage-usage"></a>Datenaufbewahrung und Speichernutzung

Azure Logic Apps verwendet [Azure Storage](../storage/index.yml) für alle Speichervorgänge. Die Ein- und Ausgaben aus dem Ausführungsverlauf Ihres Workflows werden basierend auf dem [Aufbewahrungslimit des Ausführungsverlaufs](logic-apps-limits-and-config.md#run-duration-retention-limits), das für Ihre Logik-App gilt, gespeichert und gemessen. Informationen zur Überwachung der Speichernutzung finden Sie unter [Anzeigen von Metriken zu Ausführungen und Speichernutzung](plan-manage-costs.md#monitor-billing-metrics).

| Umgebung | Hinweise |
|-------------|-------|
| **Mehrinstanzenfähig** | Die Kosten für die Speichernutzung und die Aufbewahrung werden anhand eines Festpreises berechnet. Informationen hierzu finden Sie auf der [Seite mit den Preisen für Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps) in der Tabelle mit den **Preisdetails**. |
| **Einzelmandant** | Die Speichernutzung und die Aufbewahrung werden anhand des [Azure Storage-Preismodells](https://azure.microsoft.com/pricing/details/storage/) berechnet. Speicherkosten werden in Ihrer Azure-Rechnung separat aufgeführt. Weitere Informationen finden Sie unter [Speichertransaktionen (bei nur einem Mandanten)](#storage-transactions). |
| **ISE** | Für die Speichernutzung und die Aufbewahrung fallen keine Gebühren an. |
|||

<a name="data-gateway"></a>

## <a name="on-premises-data-gateway"></a>Lokales Datengateway

Ein [lokales Datengateway](../logic-apps/logic-apps-gateway-install.md) ist eine gesonderte Ressource, die Sie erstellen, damit Ihre Logik-App-Workflows über bestimmte vom Gateway unterstützte Connectors auf lokale Daten zugreifen können. Für über das Gateway erfolgende Connectorvorgänge fallen Gebühren an, für das Gateway selbst jedoch nicht.

<a name="disabled-apps"></a>

## <a name="disabled-logic-apps-or-workflows"></a>Deaktivierte Logik-Apps oder Workflows

Für deaktivierte Logik-Apps (mehrere Mandanten) oder Workflows (einzelner Mandant) fallen keine Gebühren an, weil darüber während der Deaktivierung keine neuen Instanzen erstellt werden können.

## <a name="next-steps"></a>Nächste Schritte

* [Planen und Verwalten von Kosten für Azure Logic Apps](plan-manage-costs.md)