---
title: Erkennung doppelter Nachrichten von Azure Service Bus | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie Duplikate in Azure Service Bus-Nachrichten erkennen können. Die doppelte Nachricht kann ignoriert und gelöscht werden.
ms.topic: article
ms.date: 04/19/2021
ms.openlocfilehash: 0b4c90885c9da817e4f78eea883e2d39e65b2480
ms.sourcegitcommit: aba63ab15a1a10f6456c16cd382952df4fd7c3ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107989286"
---
# <a name="duplicate-detection"></a>Duplikaterkennung

Tritt bei einer Anwendung unmittelbar nach dem Senden einer Nachricht ein schwerwiegender Fehler auf und geht die neu gestartete Anwendungsinstanz fälschlicherweise davon aus, dass die vorherige Nachrichtenübermittlung nicht stattgefunden hat, ist die Nachricht nach einem weiteren Sendevorgang zweimal im System enthalten.

Es ist auch möglich, dass kurz zuvor ein Fehler auf Client- oder Netzwerkebene aufgetreten ist, sodass eine gesendete Nachricht in die Warteschlange eingereiht wird, ohne dass die Bestätigung erfolgreich an den Client zurückgesendet werden kann. In diesem Fall weiß der Client nicht sicher, welches Ergebnis der Sendevorgang hatte.

Durch die Erkennung von Duplikaten werden diese Situationen aufgelöst, indem der Absender die gleiche Nachricht erneut senden kann, während die Warteschlange oder das Thema mögliche Duplikate verwirft.

> [!NOTE]
> Der Basic-Tarif von Service Bus unterstützt keine Duplikaterkennung. Die Standard- und Premium-Tarife unterstützen Duplikaterkennung. Informationen zu den Unterschieden zwischen diesen Tarifen finden Sie unter [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="how-it-works"></a>So funktioniert's 
Das Aktivieren der Erkennung von Duplikaten unterstützt das Nachverfolgen der anwendungsgesteuerten *MessageId* aller in einem angegebenen Zeitfenster an eine Warteschlange oder ein Thema gesendeten Nachrichten. Wenn eine neue Nachricht mit einer *MessageId* gesendet wird, die während des Zeitfensters erfasst wurde, wird die Nachricht als akzeptiert gemeldet (der Sendevorgang war erfolgreich). Die neu gesendete Nachricht wird jedoch sofort ignoriert und verworfen. Es werden keine anderen Teile der Nachricht außer der *MessageId* ausgewertet.

Die Anwendungssteuerung der ID ist wichtig, da nur diese es der Anwendung erlaubt, die *MessageId* einem Geschäftsprozesskontext zuzuordnen, aus dem sie bei einem Fehler vorhersagbar rekonstruiert werden kann.

Für einen Geschäftsprozess, bei dem für die Behandlung von Anwendungskontext mehrere Nachrichten gesendet werden, setzt sich die *MessageId* möglicherweise aus dem Kontextbezeichner auf Anwendungsebene (z.B. einer Bestellnummer) und dem Betreff der Nachricht (z.B. **12345.2017/Bezahlung**) zusammen.

Die *MessageId* kann immer auch eine GUID sein, aber das Verankern des Bezeichners in den Geschäftsprozess sorgt für eine vorhersagbare Wiederholbarkeit, die für die effektive Verwendung der Erkennung von Duplikaten gewünscht wird.

> [!IMPORTANT]
>- Wenn **Partitionierung** **aktiviert** ist, wird `MessageId+PartitionKey` verwendet, um die Eindeutigkeit zu bestimmen. Wenn Sitzungen aktiviert sind, müssen der Partitionsschlüssel und die Sitzungs-ID identisch sein. 
>- Wenn **Partitionierung** **deaktiviert** ist (Standardeinstellung), wird nur `MessageId` verwendet, um die Eindeutigkeit zu bestimmen.
>- Weitere Informationen zu SessionId, PartitionKey und MessageId finden Sie unter [Verwenden von Partitionsschlüsseln](service-bus-partitioning.md#use-of-partition-keys).
>- Die [Premier-Ebene](service-bus-premium-messaging.md) unterstützt keine Partitionierung. Daher wird empfohlen, eindeutige Nachrichten-IDs in Ihren Anwendungen zu verwenden und sich für die Duplikaterkennung nicht auf Partitionsschlüssel zu verlassen. 


## <a name="duplicate-detection-window-size"></a>Fenstergröße für Duplikaterkennung

Neben der Aktivierung der Duplikaterkennung können Sie auch die Größe des Zeitfensters für den Duplikaterkennungsverlauf konfigurieren, in dem Nachrichten-IDs beibehalten werden.
Der Wert beträgt für Warteschlangen und Themen standardmäßig 10 Minuten. Der Mindestwert beträgt 20 Sekunden, der Höchstwert beträgt 7 Tage.

Das Aktivieren der Duplikaterkennung und die Größe des Zeitfensters haben direkte Auswirkungen auf den Durchsatz von Warteschlangen (und Themen), da alle aufgezeichneten Nachrichten-IDs mit den neu übermittelten Nachrichten-IDs verglichen werden müssen.

Ein kürzeres Zeitfenster bedeutet, dass weniger Nachrichten-IDs beibehalten und verglichen werden müssen, sodass der Durchsatz weniger beeinträchtigt wird. Für Entitäten mit einem hohen Durchsatz, die eine Duplikaterkennung erfordern, sollten Sie das Zeitfenster so klein wie möglich halten.

## <a name="next-steps"></a>Nächste Schritte
Sie können die Duplikaterkennung mit dem Azure-Portal, PowerShell, der CLI, einer Resource Manager-Vorlage, .NET, Java, Python und JavaScript aktivieren. Weitere Informationen finden Sie unter [Aktivieren der Duplikaterkennung](enable-duplicate-detection.md). 

In Szenarien, in denen der Clientcode eine Nachricht mit der gleichen *MessageId* wie zuvor nicht erneut übermitteln kann, ist es wichtig, Nachrichten zu entwerfen, die sicher erneut verarbeitet werden können. In diesem [Blogbeitrag zu Idempotenz](https://particular.net/blog/what-does-idempotent-mean) werden verschiedene Verfahren für diese Vorgehensweise beschrieben.

Sehen Sie sich die Beispiele in der Sprache Ihrer Wahl an, um Azure Service Bus-Features zu untersuchen. 

- [Azure Service Bus-Clientbibliothekbeispiele für .NET (neueste Version)](/samples/azure/azure-sdk-for-net/azuremessagingservicebus-samples/) 
- [Azure Service Bus-Clientbibliothekbeispiele für Java (neueste Version)](/samples/azure/azure-sdk-for-java/servicebus-samples/)
- [Azure Service Bus-Clientbibliothekbeispiele für Python](/samples/azure/azure-sdk-for-python/servicebus-samples/)
- [Azure Service Bus-Clientbibliothekbeispiele für JavaScript](/samples/azure/azure-sdk-for-js/service-bus-javascript/)
- [Azure Service Bus-Clientbibliothekbeispiele für TypeScript](/samples/azure/azure-sdk-for-js/service-bus-typescript/)

Hier finden Sie Beispiele für die älteren .NET- und Java-Clientbibliotheken:
- [Azure Service Bus-Clientbibliothekbeispiele für .NET (Legacy)](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/)
- [Azure Service Bus-Clientbibliothekbeispiele für Java (Legacy)](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/azure-servicebus)

