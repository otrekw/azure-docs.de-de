---
title: 'Azure Event Hubs: Resource Manager-Ausnahmen | Microsoft-Dokumentation'
description: Enthält eine Liste mit den Azure Event Hubs-Ausnahmen, die von Azure Resource Manager angezeigt werden, und vorgeschlagene Aktionen.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: cec24696d0d49ba408860f6562c34dd14876c311
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91334207"
---
# <a name="azure-event-hubs---resource-manager-exceptions"></a>Azure Event Hubs – Resource Manager-Ausnahmen
In diesem Artikel werden die Ausnahmen aufgelistet, die bei der Interaktion mit Azure Event Hubs mithilfe von Azure Resource Manager (über Vorlagen oder direkte Aufrufe) generiert werden.

> [!IMPORTANT]
> Dieses Dokument wird häufig aktualisiert. Überprüfen Sie zu einem späteren Zeitpunkt auf dieser Seite, ob neue Informationen vorliegen.

Die folgenden Abschnitte enthalten verschiedene Ausnahmen/Fehler, die von Azure Resource Manager angezeigt werden.

## <a name="error-code-conflict"></a>Fehlercode: Konflikt:

| Fehlercode | Untergeordneter Fehlercode | Fehlermeldung | BESCHREIBUNG | Empfehlung |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| Konflikt: | 40300 | Die maximale Anzahl von Ressourcen vom Typ „EventHub“ wurde erreicht oder überschritten. Ist-Wert: #, Maximal zulässig: # | Für den Namespace wurde das [Kontingent](event-hubs-quotas.md) für die Anzahl zulässiger Event Hubs erreicht. | Löschen Sie alle nicht verwendeten oder fehlerhaften Event Hubs aus dem Namespace, oder erwägen Sie ein Upgrade auf einen [dedizierten Cluster](event-hubs-dedicated-overview.md). |
| Konflikt: | none | Die Konfiguration der Notfallwiederherstellung kann nicht gelöscht werden, weil gerade eine Replikation durchgeführt wird. Führen Sie ein Failover aus, oder lösen Sie die Kopplung auf, bevor Sie versuchen, die Konfiguration der Notfallwiederherstellung zu löschen. | Da gerade eine [Notfallwiederherstellung mit Georeplikation (GeoDR)](event-hubs-geo-dr.md) durchgeführt wird, kann die Konfiguration derzeit nicht gelöscht werden. | Warten Sie, bis die Replikation abgeschlossen ist, lösen Sie ein Failover aus, oder lösen Sie die Kopplung der Notfallwiederherstellung mit Georeplikation auf, um das Löschen der Konfiguration zu ermöglichen. |
| Konflikt: | none | Beim Update des Namespace ist ein Fehler aufgetreten, weil ein Konflikt mit dem Back-End besteht. | Für diesen Namespace wird derzeit ein anderer Vorgang ausgeführt. | Warten Sie, bis der aktuelle Vorgang abgeschlossen ist, und versuchen Sie es anschließend noch einmal. |

## <a name="error-code-429"></a>Fehlercode: 429

| Fehlercode | Untergeordneter Fehlercode | Fehlermeldung | BESCHREIBUNG | Empfehlung |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| 429 | none | Namespacebereitstellung im Übergang | Für diesen Namespace wird derzeit ein anderer Vorgang ausgeführt. | Warten Sie, bis der aktuelle Vorgang abgeschlossen ist, und versuchen Sie es anschließend noch einmal. |
| 429 | none | Der Vorgang für die Notfallwiederherstellung wird gerade durchgeführt. | Für diesen Namespace bzw. diese Kopplung wird gerade ein Vorgang für die [Notfallwiederherstellung mit Georeplikation](event-hubs-geo-dr.md) durchgeführt. | Warten Sie, bis der aktuelle Vorgang für die Notfallwiederherstellung mit Georeplikation abgeschlossen ist, und versuchen Sie es anschließend noch einmal. |

## <a name="error-code-badrequest"></a>Fehlercode: BadRequest

| Fehlercode | Untergeordneter Fehlercode | Fehlermeldung | BESCHREIBUNG | Empfehlung |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| BadRequest | 40.000 | PartitionCount kann für einen Event Hub nicht geändert werden. | Für den Tarif „Basic“ oder „Standard“ von Azure Event Hubs wird das Ändern von Partitionen nicht unterstützt. | Erstellen Sie in Ihrem Namespace im Tarif „Basic“ oder „Standard“ einen neuen Event Hub mit der gewünschten Anzahl von Partitionen. Das horizontale Hochskalieren von Partitionen wird für [dedizierte Cluster](event-hubs-dedicated-overview.md) unterstützt. |
| BadRequest | 40.000 | Der Wert „#“ für „MessageRetentionInDays“ ist für den Tarif „Basic“ nicht gültig. Der Wert darf nicht höher als „1“ Tag sein. | Für Event Hubs-Namespaces im Tarif „Basic“ wird für die Nachrichtenaufbewahrung nur ein Zeitraum von maximal einem Tag unterstützt. | [Erstellen Sie einen Event Hubs-Standardnamespace](event-hubs-create.md), falls Sie Nachrichten länger als einen Tag lang aufbewahren möchten. | 
| BadRequest | none | Der angegebene Name ist nicht verfügbar. | Namespacenamen müssen eindeutig sein, und der angegebene Name wird bereits verwendet. | Wenn Sie der Besitzer des vorhandenen Namespace mit dem angegebenen Namen sind, können Sie ihn löschen. Dies führt zu Datenverlust. Versuchen Sie es anschließend erneut mit demselben Namen. Wenn das Löschen des Namespace nicht sicher ist (oder Sie nicht der Besitzer sind), sollten Sie einen anderen Namespacenamen auswählen. |
| BadRequest | none | Für das angegebene Abonnement wurde das Namespacekontingent erreicht. | Für Ihr Abonnement wurde das zulässige [Kontingent](event-hubs-quotas.md) für die Anzahl von Namespaces erreicht. | Erwägen Sie, nicht verwendete Namespaces unter diesem Abonnement zu löschen, erstellen Sie ein anderes Abonnement, oder führen Sie ein Upgrade auf einen [dedizierten Cluster](event-hubs-dedicated-overview.md) durch. |
| BadRequest | none | Sekundärer Namespace kann nicht aktualisiert werden | Der Namespace kann nicht aktualisiert werden, weil es sich um den sekundären Namespace der [Kopplung einer Notfallwiederherstellung mit Georeplikation](event-hubs-geo-dr.md) handelt. | Nehmen Sie die Änderung stattdessen am primären Namespace der Kopplung vor, falls dies möglich ist. Lösen Sie andernfalls die Kopplung einer Notfallwiederherstellung mit Georeplikation auf, um die Änderung vorzunehmen. |
| BadRequest | none | Automatische Vergrößerung kann in Basic-SKU nicht festgelegt werden | Die automatische Vergrößerung kann für Event Hubs-Namespaces im Tarif „Basic“ nicht aktiviert werden. | Sie müssen den Tarif „Standard“ verwenden, um für einen Namespace die [automatische Vergrößerung zu aktivieren](event-hubs-auto-inflate.md). |
| BadRequest | none | Es ist nicht genügend Kapazität vorhanden, um den Namespace zu erstellen. Wenden Sie sich an Ihren Event Hubs-Administrator. | Für die ausgewählte Region ist keine Kapazität mehr vorhanden, und es können keine weiteren Namespaces erstellt werden. | Wählen Sie eine andere Region für Ihren Namespace aus. |
| BadRequest | none | Der Vorgang kann nicht für den Entitätstyp „ConsumerGroup“ durchgeführt werden, weil für den Namespace „Namespacename“ der Tarif „Basic“ verwendet wird.  | Event Hubs-Namespaces im Tarif „Basic“ verfügen über ein Kontingent (event-hubs-quotas.md) von einer Consumergruppe (Standardeinstellung). Die Erstellung von weiteren Consumergruppen wird nicht unterstützt. | Verwenden Sie die Standardconsumergruppe ($Default) weiter, oder erwägen Sie stattdessen die Verwendung eines Event Hubs-Namespace im Tarif „Standard“, falls Sie weitere Consumergruppen benötigen. | 
| BadRequest | none | Der Namespace „Namespacename“ ist nicht vorhanden. | Der angegebene Namespace wurde nicht gefunden. | Überprüfen Sie, ob der Namespacename stimmt und in Ihrem Abonnement auffindbar ist. Wenn nicht, sollten Sie einen [Event Hubs-Namespace erstellen](event-hubs-create.md). | 
| BadRequest | none | Die Standorteigenschaft der Ressource stimmt nicht mit dem Namespace überein, in dem diese enthalten ist. | Beim Erstellen eines Event Hubs in einer bestimmten Region ist ein Fehler aufgetreten, weil diese nicht mit der Region des Namespace übereinstimmt. | Versuchen Sie, den Event Hub in derselben Region wie den Namespace zu erstellen. | 

## <a name="error-code-internal-server-error"></a>Fehlercode: Interner Serverfehler

| Fehlercode | Untergeordneter Fehlercode | Fehlermeldung | BESCHREIBUNG | Empfehlung |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| Interner Serverfehler | none | Interner Serverfehler. | Für den Event Hubs-Dienst ist ein interner Fehler aufgetreten. | Wiederholen Sie den Vorgang, für den der Fehler aufgetreten ist. Wenden Sie sich an den Support, falls der Fehler für den Vorgang weiterhin auftritt. |