---
title: Konfigurieren der Datensammlung für den Azure Monitor-Agent (Vorschau)
description: In diesem Artikel wird beschrieben, wie Sie eine Datensammlungsregel erstellen, um mit dem Azure Monitor-Agent Daten von VMs zu sammeln.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/16/2021
ms.openlocfilehash: 884f048ac099cfc6b799fe266172a0eecef3db6f
ms.sourcegitcommit: afb79a35e687a91270973990ff111ef90634f142
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107478368"
---
# <a name="configure-data-collection-for-the-azure-monitor-agent-preview"></a>Konfigurieren der Datensammlung für den Azure Monitor-Agent (Vorschau)

Mit Datensammlungsregeln werden die in Azure Monitor eingehenden Daten definiert, und es wird angegeben, wohin sie gesendet werden sollen. In diesem Artikel wird beschrieben, wie Sie eine Datensammlungsregel erstellen, um mit dem Azure Monitor-Agent Daten von virtuellen Computern zu sammeln.

Eine vollständige Beschreibung der Datensammlungsregeln finden Sie unter [Datensammlungsregeln in Azure Monitor (Vorschau)](data-collection-rule-overview.md).

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie Daten für virtuelle Computer mit dem Azure Monitor-Agent konfigurieren, der sich derzeit in der Vorschauphase befindet. Eine Beschreibung der allgemein verfügbaren Agents und ihrer Nutzung zum Sammeln von Daten finden Sie unter [Übersicht über Azure Monitor-Agents](agents-overview.md).

## <a name="data-collection-rule-associations"></a>Zuordnungen zu Datensammlungsregeln

Zum Anwenden einer Datensammlungsregel auf einen virtuellen Computer erstellen Sie eine Zuordnung für den Computer. Ein virtueller Computer kann über eine Zuordnung zu mehreren Datensammlungsregeln verfügen, und einer Datensammlungsregel können mehrere virtuelle Computer zugeordnet sein. Auf diese Weise können Sie eine Gruppe mit Datensammlungsregeln definieren, die jeweils für eine bestimmte Anforderung gelten, und sie dann nur auf die virtuellen Computer anwenden, für die sie gelten. 

Angenommen, in einer Umgebung mit einer Gruppe von virtuellen Computern wird auf einigen eine Branchenanwendung und auf den anderen SQL Server ausgeführt. Unter Umständen verfügen Sie über eine Standard-Datensammlungsregel, die für alle virtuellen Computer gilt, und über separate Datensammlungsregeln, mit denen Daten speziell für die Branchenanwendung und für SQL Server gesammelt werden. Die Zuordnungen der virtuellen Computer zu den Datensammlungsregeln sieht dann in etwa wie im folgenden Diagramm aus.

![Diagramm mit virtuellen Computern, auf denen eine Branchenanwendung und SQL Server gehostet werden. Dazu gehören die Datensammlungsregeln „central-i t-default“ und „lob-app“ für die Branchenanwendung sowie „central-i t-default“ und „s q l“ für SQL Server.](media/data-collection-rule-azure-monitor-agent/associations.png)



## <a name="create-rule-and-association-in-azure-portal"></a>Erstellen einer Regel und einer Zuordnung im Azure-Portal

Sie können das Azure-Portal verwenden, um eine Datensammlungsregel zu erstellen und dieser Regel unter Ihrem Abonnement virtuelle Computer zuzuordnen. Der Azure Monitor-Agent wird automatisch installiert, und für alle virtuellen Computer wird eine verwaltete Identität erstellt (falls noch nicht geschehen).

> [!IMPORTANT]
> Derzeit besteht folgendes bekanntes Problem: Wenn die Datensammlungsregel eine verwaltete Identität auf einem virtuellen Computer erstellt, der bereits über eine benutzerseitig zugewiesene verwaltete Identität verfügt, wird die benutzerseitig zugewiesene Identität deaktiviert.

Wählen Sie im Portal im Menü **Azure Monitor** unter **Einstellungen** die Option **Datensammlungsregeln** aus. Klicken Sie auf **Hinzufügen**, um eine neue Datensammlungsregel und eine Zuweisung hinzuzufügen.

[![Datensammlungsregeln](media/data-collection-rule-azure-monitor-agent/data-collection-rules.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rules.png#lightbox)

Klicken Sie auf **Hinzufügen**, um eine neue Regel und eine Gruppe von Zuordnungen zu erstellen. Geben Sie einen **Regelnamen** und dann ein **Abonnement** und eine **Ressourcengruppe** an. Hiermit wird angegeben, wo die Datensammlungsregel erstellt wird. Die virtuellen Computer und ihre Zuordnungen können unter einem beliebigen Abonnement bzw. einer Ressourcengruppe auf dem Mandanten angeordnet sein.

[![Grundlagen von Datensammlungsregeln](media/data-collection-rule-azure-monitor-agent/data-collection-rule-basics.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-basics.png#lightbox)

Fügen Sie auf der Registerkarte **Virtuelle Computer** virtuelle Computer hinzu, für die die Datensammlungsregel angewendet werden sollte. In Ihrer Umgebung sollten sowohl Azure-VMs als auch Server mit Azure Arc-Unterstützung aufgeführt sein. Der Azure Monitor-Agent wird auf virtuellen Computern installiert, auf denen dies noch nicht durchgeführt wurde.

[![Datensammlungsregeln für virtuelle Computer](media/data-collection-rule-azure-monitor-agent/data-collection-rule-virtual-machines.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-virtual-machines.png#lightbox)

Klicken Sie auf der Registerkarte **Sammeln und übermitteln** auf **Datenquelle hinzufügen**, um eine Datenquelle und einen Zielsatz hinzuzufügen. Wählen Sie einen **Datenquellentyp** aus. Die entsprechenden Details für die Auswahl werden angezeigt. Für Leistungsindikatoren können Sie eine Auswahl aus einer Gruppe mit Objekten mit der zugehörigen Stichprobenhäufigkeit treffen. Für Ereignisse können Sie bestimmte Protokolle bzw. entsprechende Funktionen und den Schweregrad auswählen. 

[![Einfache Datenquelle](media/data-collection-rule-azure-monitor-agent/data-collection-rule-data-source-basic.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-data-source-basic.png#lightbox)


Um andere Protokolle und Leistungsindikatoren aus den [derzeit unterstützten Datenquellen](azure-monitor-agent-overview.md#data-sources-and-destinations) anzugeben oder um Ereignisse mithilfe von XPath-Abfragen zu filtern, wählen Sie **Benutzerdefiniert** aus. Anschließend können Sie einen [XPath](https://www.w3schools.com/xml/xpath_syntax.asp) für bestimmte Werte angeben, die gesammelt werden sollen. Beispiele hierzu finden Sie unter [Beispiel für eine Datensammlungsregel](data-collection-rule-overview.md#sample-data-collection-rule).

[![Datenquelle: Benutzerdefiniert](media/data-collection-rule-azure-monitor-agent/data-collection-rule-data-source-custom.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-data-source-custom.png#lightbox)

Fügen Sie auf der Registerkarte **Ziel** mindestens ein Ziel für die Datenquelle hinzu. Für Windows-Ereignis- und Syslog-Datenquellen ist nur das Senden an Azure Monitor-Protokolle möglich. Bei Leistungsindikatoren ist sowohl das Senden an Azure Monitor-Metriken als auch an Azure Monitor-Protokolle möglich.

[![Destination](media/data-collection-rule-azure-monitor-agent/data-collection-rule-destination.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-destination.png#lightbox)

Klicken Sie auf **Datenquelle hinzufügen** und dann auf **Bewerten + erstellen**, um die Details der Datensammlungsregel und die Zuordnung zur Gruppe mit den VMs zu überprüfen. Klicken Sie auf **Erstellen**, um die Erstellung durchzuführen.

> [!NOTE]
> Nachdem die Datensammlungsregel und die Zuordnungen erstellt wurden, kann es bis zu fünf Minuten dauern, bis Daten an die Ziele gesendet werden.

## <a name="limit-data-collection-with-custom-xpath-queries"></a>Begrenzen der Datensammlung mit benutzerdefinierten XPath-Abfragen
Da Ihnen alle in einem Log Analytics-Arbeitsbereich gesammelten Daten in Rechnung gestellt werden, sollten Sie nur die Daten sammeln, die Sie benötigen. Mit der Standardkonfiguration im Azure-Portal haben Sie nur eingeschränkte Möglichkeiten zur Filterung der Ereignisse, die gesammelt werden sollen. Bei Anwendungs- und Systemprotokollen sind dies alle Protokolle mit einem bestimmten Schweregrad. Bei Sicherheitsprotokollen sind es alle Protokolle von Überwachungserfolgen oder Überwachungsfehlern.

Zur Angabe von zusätzlichen Filtern müssen Sie mithilfe der benutzerdefinierten Konfiguration eine XPath-Abfrage angeben, mit der die Ereignisse herausgefiltert werden. XPath-Einträge haben folgende Form: `LogName!XPathQuery`. Angenommen, Sie möchten aus dem Anwendungsereignisprotokoll nur Ereignisse mit der Ereignis-ID 1035 abrufen. In diesem Fall würde die XPathQuery-Abfrage für diese Ereignisse `*[System[EventID=1035]]` lauten. Da Sie die Ereignisse aus dem Anwendungsereignisprotokoll abrufen möchten, würde die XPath-Abfrage `Application!*[System[EventID=1035]]` lauten.

Eine Liste der Einschränkungen in XPath, die vom Windows-Ereignisprotokoll unterstützt werden, finden Sie unter [Einschränkungen für XPath 1.0](/windows/win32/wes/consuming-events#xpath-10-limitations).

> [!TIP]
> Verwenden Sie zum Testen der Gültigkeit einer XPathQuery-Abfrage das PowerShell-Cmdlet `Get-WinEvent` mit dem Parameter `FilterXPath`. Das folgende Skript ist ein Beispiel hierfür:
> 
> ```powershell
> $XPath = '*[System[EventID=1035]]'
> Get-WinEvent -LogName 'Application' -FilterXPath $XPath
> ```
>
> - Werden Ereignisse zurückgegeben, ist die Abfrage gültig.
> - Wenn Sie die Meldung *Es wurden keine Ereignisse gefunden, die mit den angegebenen Auswahlkriterien übereinstimmen* erhalten, ist die Abfrage möglicherweise gültig, aber es sind keine übereinstimmenden Ereignisse auf dem lokalen Computer vorhanden.
> - Wenn Sie die Meldung *Die angegebene Abfrage ist ungültig* erhalten, ist die Abfragesyntax ungültig. 

Die folgende Tabelle enthält Beispiele für das Filtern von Ereignissen mithilfe einer benutzerdefinierten XPath-Abfrage:

| Beschreibung |  XPath |
|:---|:---|
| Es werden nur Systemereignisse mit der Ereignis-ID 4648 gesammelt. |  `System!*[System[EventID=4648]]`
| Es werden nur Systemereignisse mit der Ereignis-ID 4648 und dem Prozessnamen „consent.exe“ gesammelt. | `Security!*[System[(EventID=4648)]] and *[EventData[Data[@Name='ProcessName']='C:\Windows\System32\consent.exe']]` |
| Es werden alle kritischen, Fehler-, Warnungs- und Informationsereignisse aus dem Systemereignisprotokoll mit Ausnahme der Ereignis-ID 6 (Treiber geladen) gesammelt. |  `System!*[System[(Level=1 or Level=2 or Level=3) and (EventID != 6)]]` |
| Es werden alle erfolgreichen und fehlerhaften Sicherheitsereignisse für die Ereignis-ID 4624 (Erfolgreiche Anmeldung) gesammelt. |  `Security!*[System[(band(Keywords,13510798882111488)) and (EventID != 4624)]]` |


## <a name="create-rule-and-association-using-rest-api"></a>Erstellen einer Regel und einer Zuordnung mithilfe der REST-API

Führen Sie die folgenden Schritte aus, um mithilfe der REST-API eine Datensammlungsregel und Zuordnungen zu erstellen.

1. Erstellen Sie die DCR-Datei manuell im JSON-Format, das im [Beispiel für eine Datensammlungsregel](data-collection-rule-overview.md#sample-data-collection-rule) gezeigt wird.

2. Erstellen Sie die Regel mithilfe der [REST-API](/rest/api/monitor/datacollectionrules/create#examples).

3. Erstellen Sie mithilfe der [REST-API](/rest/api/monitor/datacollectionruleassociations/create#examples) für jede VM eine Zuordnung zur Datensammlungsregel.


## <a name="create-association-using-resource-manager-template"></a>Erstellen einer Zuordnung mithilfe einer Resource Manager-Vorlage

Sie können eine Zuordnung zu einer Azure-VM oder einem Server mit Azure Arc-Unterstützung mithilfe einer Resource Manager-Vorlage erstellen. Beispielvorlagen finden Sie unter [Beispiele für Resource Manager-Vorlagen für Datensammlungsregeln in Azure Monitor](./resource-manager-data-collection-rules.md).



## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über den [Azure Monitor-Agent](azure-monitor-agent-overview.md).
- Informieren Sie sich über die [Datensammlungsregeln](data-collection-rule-overview.md).
