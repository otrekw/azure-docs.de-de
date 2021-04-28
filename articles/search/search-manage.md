---
title: Dienstverwaltung im Portal
titleSuffix: Azure Cognitive Search
description: Verwalten eines Azure Cognitive Search-Diensts (gehosteter Cloudsuchdienst in Microsoft Azure) mit dem Azure-Portal.
manager: nitinme
author: HeidiSteen
ms.author: heidist
tags: azure-portal
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/06/2021
ms.openlocfilehash: 98e0137c8e48696737cd5d8d1fd4d3de925b9f7f
ms.sourcegitcommit: d63f15674f74d908f4017176f8eddf0283f3fac8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106579791"
---
# <a name="service-administration-for-azure-cognitive-search-in-the-azure-portal"></a>Dienstverwaltung für Azure Cognitive Search im Azure-Portal

> [!div class="op_single_selector"]
>
> * [PowerShell](search-manage-powershell.md)
> * [Azure-Befehlszeilenschnittstelle](search-manage-azure-cli.md)
> * [REST-API](/rest/api/searchmanagement/)
> * [.NET SDK](/dotnet/api/microsoft.azure.management.search)
> * [Portal](search-manage.md)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Azure Cognitive Search ist ein vollständig verwalteter cloudbasierter Suchdienst zum Erstellen einer umfassenden Suchoberfläche für benutzerdefinierte Apps. In diesem Artikel werden die Verwaltungsaufgaben beschrieben, die Sie im [Azure-Portal](https://portal.azure.com) für einen zuvor erstellten Suchdienst ausführen können. Über das Portal können Sie alle [Verwaltungsaufgaben](#management-tasks) im Zusammenhang mit dem Dienst sowie die Verwaltung und Erkundung von Inhalten durchführen. Daher bietet das Portal einen umfassenden Zugriff auf alle Aspekte von Suchdienstvorgängen.

Jeder Suchdienst wird als eigenständige Ressource verwaltet. Die folgende Abbildung zeigt die Portalseiten für einen einzelnen kostenlosen Suchdienst namens „demo-search-svc“. Obwohl Sie es vielleicht gewohnt sind, Azure PowerShell oder die Azure CLI für die Dienstverwaltung zu verwenden, ist es sinnvoll, sich mit den auf den Portalseiten bereitgestellten Tools und Funktionen vertraut zu machen. Einige Aufgaben können im Portal einfacher und schneller ausgeführt werden als über Code. 

## <a name="overview-home-page"></a>Übersichtsseite (Startseite)

Die Übersichtsseite ist sozusagen die Startseite jedes Diensts. Unten sind in den rot eingerahmten Bereichen auf dem Bildschirm Aufgaben, Tools und Kacheln zu sehen, die Sie möglicherweise häufig verwenden, insbesondere, wenn Sie noch nicht mit dem Dienst arbeiten.

:::image type="content" source="media/search-manage/search-portal-overview-page.png" alt-text="Portalseiten für einen Suchdienst" border="true":::

| Bereich | BESCHREIBUNG |
|------|-------------|
| 1  | Im Abschnitt **Essentials** (Zusammenfassung) werden Diensteigenschaften angezeigt, einschließlich des Endpunkts, der beim Einrichten von Verbindungen verwendet wird. Außerdem sind die Anzahl der Ebenen, Replikate und Partitionen auf einen Blick zu sehen. |
| 2 | Oben auf der Seite finden Sie eine Reihe von Befehlen zum Aufrufen interaktiver Tools oder zum Verwalten des Diensts. Sowohl [Daten importieren](search-get-started-portal.md) auch der [Suchexplorer](search-explorer.md) werden häufig für die Prototyperstellung und Erkundung verwendet. |
| 3 | Unterhalb des Abschnitts **Essentials** (Zusammenfassung) finden Sie mehrere Registerkarten zu Unterseiten für den schnellen Zugriff auf Nutzungsstatistiken, Dienstintegritätsmetriken und den Zugriff auf alle vorhandenen Indizes, Indexer, Datenquellen und Skillsets in Ihrem Dienst. Wenn Sie einen Index oder ein anderes Objekt auswählen, werden zusätzliche Seiten verfügbar, um Objektzusammenstellung, Einstellungen und Status (falls zutreffend) anzuzeigen. |
| 4 | Auf der linken Seite können Sie auf Links zugreifen, die zusätzliche Seiten für den Zugriff auf die für Verbindungen verwendeten API-Schlüssel, die Konfiguration des Diensts, die Konfiguration der Sicherheitsfunktionen, die Überwachung von Vorgängen, die Automatisierung von Aufgaben und die Anforderung von Support öffnen. |

### <a name="read-only-service-properties"></a>Schreibgeschützte Diensteigenschaften

Einige Aspekte eines Suchdiensts werden beim Bereitstellen des Diensts festgelegt und können nicht mehr geändert werden:

* Dienstname (Sie können einen Suchdienst nicht umbenennen.)
* Dienstspeicherort (Sie können einen intakten Suchdienst nicht einfach in eine andere Region verschieben) Obwohl es eine Vorlage gibt, ist das Verschieben des Inhalts ein manueller Prozess.)
* Dienstebene (Sie können z. B. nicht von S1 zu S2 wechseln, ohne einen neuen Dienst zu erstellen.)

## <a name="management-tasks"></a>Verwaltungsaufgaben

Die Dienstverwaltung ist von Grund auf einfach konzipiert und wird häufig durch die Aufgaben definiert, die Sie bezogen auf den Dienst selbst ausführen können:

* [Anpassen der Kapazität](search-capacity-planning.md) durch Hinzufügen oder Entfernen von Replikaten und Partitionen
* [Verwalten von API-Schlüsseln](search-security-api-keys.md) für Administrator- und Abfragevorgänge
* [Steuern des Zugriffs auf Administratorvorgänge](search-security-rbac.md) über rollenbasierte Sicherheit
* [Konfigurieren von IP-Firewallregeln](service-configure-firewall.md) zum Einschränken des Zugriffs nach IP-Adresse
* [Konfigurieren eines privaten Endpunkts](service-create-private-endpoint.md) mit Azure Private Link und einem privaten virtuellen Netzwerk
* [Überwachen der Dienstintegrität](search-monitor-usage.md): Speicher, Abfragevolumes und Latenz

Sie können auch alle Objekte aufzählen, die für den Dienst erstellt wurden: Indizes, Indexer, Datenquellen, Skillsets usw. Auf der Übersichtsseite des Portals werden alle Inhalte angezeigt, die in Ihrem Dienst vorhanden sind. Die meisten Vorgänge für einen Suchdienst sind inhaltsbezogen.

Die gleichen Verwaltungsaufgaben, die im Portal ausgeführt werden, können auch programmgesteuert über die [Verwaltungs-REST-APIs](/rest/api/searchmanagement/), das [Az.Search-PowerShell-Modul,](search-manage-powershell.md) [az search-Azure CLI-Modul](search-manage-azure-cli.md) und die Azure SDKs für .NET, Python, Java und JavaScript verarbeitet werden. Verwaltungsaufgaben werden vollständig über das Portal und alle programmgesteuerten Schnittstellen dargestellt. Es gibt keine bestimmte Verwaltungsaufgabe, die nur in einer Modalität verfügbar ist.

Cognitive Search nutzt andere Azure-Dienste zur detaillierteren Überwachung und Verwaltung. An sich sind die einzigen in einem Suchdienst gespeicherten Daten Objektinhalte (Indizes, Indexer und Datenquellendefinitionen sowie andere Objekte). Metriken, die auf Portalseiten gemeldet werden, werden alle 30 Tage aus internen Protokollen abgerufen. Für die benutzergesteuerte Protokollaufbewahrung und zusätzliche Ereignisse benötigen Sie [Azure Monitor](../azure-monitor/index.yml). Weitere Informationen zum Einrichten der Diagnoseprotokollierung für einen Suchdienst finden Sie unter [Sammeln und Analysieren von Protokolldaten](search-monitor-logs.md).

## <a name="administrator-permissions"></a>Administratorberechtigungen

Wenn Sie die Übersichtsseite des Suchdiensts öffnen, bestimmen die Ihrem Konto zugewiesenen Berechtigungen, welche Seiten ihnen zur Verfügung stehen. Die Übersichtsseite am Anfang des Artikels zeigt die Portalseiten an, die einem Administrator oder Mitwirkenden angezeigt werden.

In einer Azure-Ressource werden Administratorrechte über Rollenzuweisungen gewährt. Im Kontext von Azure Cognitive Search bestimmen die [Rollenzuweisungen](search-security-rbac.md), wer Replikate und Partitionen zuweisen oder API-Schlüssel verwalten kann, unabhängig davon, ob Sie das Portal, [PowerShell](search-manage-powershell.md), die [Azure CLI](search-manage-azure-cli.md) oder die [Verwaltungs-REST-APIs](/rest/api/searchmanagement/search-howto-management-rest-api) verwenden:

> [!TIP]
> Die Bereitstellung oder Außerbetriebnahme des eigentlichen Diensts kann von einem Administrator oder Co-Administrator des Azure-Abonnements durchgeführt werden. Mithilfe von Azure-weit gültigen Mechanismen können Sie ein Abonnement oder eine Ressource sperren, um die versehentliche oder nicht autorisierte Löschung Ihres Suchdiensts durch Benutzer mit Administratorrechten zu verhindern. Weitere Informationen finden Sie unter [Sperren von Ressourcen zum Verhindern unerwarteter Löschungen](../azure-resource-manager/management/lock-resources.md).

## <a name="next-steps"></a>Nächste Schritte

* Überprüfen der im Portal verfügbaren [Überwachungsfunktionen](search-monitor-usage.md)
* Automatisieren mit [PowerShell](search-manage-powershell.md) bzw. der [Azure CLI](search-manage-azure-cli.md)
* Informieren über [Sicherheitsfeatures](search-security-overview.md) zum Schützen von Inhalt und Vorgängen
* Aktivieren der [Diagnoseprotokollierung](search-monitor-logs.md) zum Überwachen von Abfrage- und Indizierungsworkloads