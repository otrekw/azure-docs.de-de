---
title: 'Azure Service Bus-Messaging: Erweiterte Features'
description: Dieser Artikel enthält eine allgemeine Übersicht über erweiterte Features in Azure Service Bus.
ms.topic: overview
ms.date: 06/11/2021
ms.openlocfilehash: 80fab956c182b6f934a767f2dffc09ca2d0c0214
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112034818"
---
# <a name="azure-service-bus---advanced-features"></a>Azure Service Bus: Erweiterte Features
Service Bus enthält auch erweiterte Features für komplexere Messagingszenarien. In diesem Artikel werden einige dieser Features beschrieben.

## <a name="message-sessions"></a>Nachrichtensitzungen
Mithilfe von Sitzungen können Sie in Service Bus eine FIFO-Garantie (First In, First Out) erstellen. Nachrichtensitzungen ermöglichen die exklusive geordnete Verarbeitung von unbegrenzten Sequenzen verwandter Nachrichten. Um die Verarbeitung von Sitzungen in Systemen mit hoher Skalierung und Hochverfügbarkeit zu ermöglichen, kann mit dem Feature auch der Sitzungszustand gespeichert werden, damit Sitzungen auf sichere Weise zwischen Handlern verschoben werden können. Weitere Informationen finden Sie unter [Nachrichtensitzungen: FIFO (First In, First Out)](message-sessions.md).

## <a name="autoforwarding"></a>Automatische Weiterleitung
Bei der automatischen Weiterleitung wird eine Warteschlange oder ein Abonnement mit einer anderen Warteschlange oder einem anderen Thema im selben Namespace verkettet. Bei Verwendung dieses Features werden Nachrichten von Service Bus automatisch aus einer Warteschlange oder einem Abonnement in eine Zielwarteschlange bzw. ein Zielthema verschoben. Alle Verschiebungen dieser Art werden als Transaktionen durchgeführt. Weitere Informationen finden Sie unter [Verketten von Service Bus-Entitäten mit automatischer Weiterleitung](service-bus-auto-forwarding.md).

## <a name="dead-letter-queue"></a>Warteschlange für unzustellbare Nachrichten
Alle Service Bus-Warteschlangen und Themenabonnements verfügen über eine Warteschlange für unzustellbare Nachrichten (Dead-Letter Queue, DLQ). Eine Warteschlange dieser Art enthält Nachrichten, die die folgenden Kriterien erfüllen: 

- Die erfolgreiche Zustellung an einen Empfänger ist nicht möglich.
- Es ist ein Timeout aufgetreten.
- Sie werden von der empfangenden Anwendung explizit zurückgestellt. 

Für Nachrichten in der Warteschlange für unzustellbare Nachrichten wird der Grund angegeben, warum sie darin platziert wurden. Die Warteschlange für unzustellbare Nachrichten verfügt über einen speziellen Endpunkt, verhält sich aber sonst wie eine reguläre Warteschlange. Eine Anwendung oder ein Tool kann eine Warteschlange für unzustellbare Nachrichten durchsuchen oder die Entfernung daraus durchführen. Sie können auch automatische Weiterleitungen aus einer Warteschlange für unzustellbare Nachrichten durchführen. Weitere Informationen finden Sie unter [Übersicht über Service Bus-Warteschlangen für unzustellbare Nachrichten](service-bus-dead-letter-queues.md).

## <a name="scheduled-delivery"></a>Zeitgesteuerte Zustellung
Sie können Nachrichten zur verzögerten Verarbeitung an eine Warteschlange oder ein Thema übermitteln und einen Zeitpunkt festlegen, zu dem die Nachricht für die Nutzung verfügbar wird. Es ist auch möglich, geplante Nachrichten abzubrechen. Weitere Informationen finden Sie unter [Geplante Nachrichten](message-sequencing.md#scheduled-messages).

## <a name="message-deferral"></a>Nachrichtenverzögerung
Ein Warteschlangen- oder Abonnementclient kann das Abrufen einer empfangenen Nachricht auf einen späteren Zeitpunkt verschieben. Unter Umständen wurde die Nachricht nicht in der erwarteten Reihenfolge veröffentlicht, und der Client möchte warten, bis eine andere Nachricht empfangen wird. Verzögerte Nachrichten verbleiben in der Warteschlange bzw. im Abonnement und müssen explizit erneut aktiviert werden, indem die vom Dienst zugewiesene Sequenznummer verwendet wird. Weitere Informationen finden Sie unter [Nachrichtenverzögerung](message-deferral.md).

## <a name="batching"></a>Batching
Beim clientseitigen Batching kann ein Warteschlangen- oder Themenclient mehrere Nachrichten zusammenfassen und dann gemeinsam übertragen. Dies wird häufig genutzt, um Bandbreite zu sparen oder den Durchsatz zu erhöhen. Weitere Informationen finden Sie unter [Clientseitige Batchverarbeitung](service-bus-performance-improvements.md#client-side-batching).

## <a name="transactions"></a>Transaktionen
Eine Transaktion gruppiert zwei oder mehr Vorgänge in einem Ausführungsbereich. Service Bus ermöglicht Ihnen das Gruppieren von Vorgängen für mehrere Nachrichtenentitäten innerhalb einer Transaktion. Eine Nachrichtenentität kann eine Warteschlange, ein Thema oder ein Abonnement sein. Weitere Informationen finden Sie unter [Übersicht über die Service Bus-Transaktionsverarbeitung](service-bus-transactions.md).

## <a name="autodelete-on-idle"></a>Automatisches Löschen nach Leerlauf
Automatisches Löschen nach Leerlauf ermöglicht die Angabe eines Leerlaufintervalls, nach dessen Ablauf ein Warteschlangen- oder Themenabonnement automatisch gelöscht wird. Die Mindestdauer ist fünf Minuten. 

## <a name="duplicate-detection"></a>Duplikaterkennung
Mit dem Feature „Duplikaterkennung“ kann der Absender die gleiche Nachricht erneut senden, und der Broker kann ein potenzielles Duplikat verwerfen. Weitere Informationen finden Sie unter [Duplikaterkennung](duplicate-detection.md).

## <a name="geo-disaster-recovery"></a>Georedundante Notfallwiederherstellung
Wenn es für eine Azure-Region zu Downtime kommt, ermöglicht das Feature „Notfallwiederherstellung“ die Nachrichtenverarbeitung, um den Betrieb in einer anderen Region oder einem anderen Rechenzentrum fortsetzen zu können. Bei diesem Feature wird eine strukturelle Spiegelung eines Namespace in der sekundären Region vorgehalten, sodass für die Namespaceidentität zum sekundären Namespace gewechselt werden kann. Bereits gepostete Nachrichten verbleiben im ersten primären Namespace, damit nach der Behebung des Verfügbarkeitsproblems die Wiederherstellung durchgeführt werden kann. Weitere Informationen finden Sie unter [Georedundante Notfallwiederherstellung in Azure Service Bus](service-bus-geo-dr.md).

## <a name="security"></a>Sicherheit
Für Service Bus werden die Standardprotokolle [AMQP 1.0](service-bus-amqp-overview.md) und [HTTP oder REST](/rest/api/servicebus/) sowie die zugehörigen Sicherheitsfunktionen unterstützt, z. B. Transport Layer Security (TLS). Für Clients kann der Zugriff autorisiert werden, indem die rollenbasierte Sicherheit per [Shared Access Signature](service-bus-sas.md) oder [Azure Active Directory](service-bus-authentication-and-authorization.md) verwendet wird. 

Als Schutz vor unerwünschtem Datenverkehr verfügt Service Bus über [Sicherheitsfunktionen](network-security.md), z. B. IP-Firewall und Integration in virtuelle Netzwerke. 

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich [Service Bus-Messagingbeispiele](service-bus-samples.md) an, die die Verwendung dieser Service Bus-Features zeigen.
