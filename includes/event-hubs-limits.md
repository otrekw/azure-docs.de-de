---
title: include file
description: include file
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 09/10/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 31031462d9904e3554c19f47bc82f87746443693
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91371724"
---
In der folgenden Tabelle finden Sie die Kontingente und Grenzwerte, die für [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) gelten. Informationen zu den Preisen von Event Hubs finden Sie unter [Event Hubs – Preise](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="common-limits-for-all-tiers"></a>Allgemeine Grenzwerte für alle Ebenen
Die folgenden Grenzwerte sind gängig für alle Ebenen: 

| Begrenzung |  Notizen | Wert |
| --- |  --- | --- |
| Anzahl von Event Hubs-Namespaces pro Abonnement |- |100 |
| Anzahl von Event Hubs pro Namespace | Nachfolgende Anforderungen für die Erstellung eines neuen Event Hub werden zurückgewiesen. |10 |
| Anzahl von Partitionen pro Event Hub |- |32 |
| Größe eines Event Hub-Namens |- | 256 Zeichen |
| Größe eines Consumergruppennamens |- | 256 Zeichen |
| Anzahl nicht epochenbezogener Empfänger pro Consumergruppe |- |5 |
| Anzahl von Autorisierungsregeln pro Namespace | Nachfolgende Anforderungen zur Erstellung von Autorisierungsregeln werden abgelehnt.|12 |
| Die Anzahl der Aufrufe der GetRuntimeInformation-Methode |  - | 50 pro Sekunde | 
| Anzahl von Regeln für virtuelle Netzwerke (VNET) und IP-Konfigurationen | - | 128 | 


### <a name="basic-and-standard-tiers"></a>Basic- und Standard-Tarif
Die folgende Tabelle enthält die Grenzwerte, die sich für die Ebenen „Basic“ und „Standard“ unter Umständen unterscheiden: 

| Begrenzung | Notizen | Basic | Standard |
| --- |  --- | -- | --- |
| Maximale Größe des Event Hubs-Ereignisses| &nbsp; | 256 KB | 1 MB |
| Anzahl von Consumergruppen pro Event Hub | &nbsp; |1 |20 |
| Anzahl von AMQP-Verbindungen pro Namespace | Nachfolgende Anforderungen für zusätzliche Verbindungen werden abgelehnt, und der aufrufende Code empfängt eine Ausnahme. |100 |5\.000|
| Maximale Aufbewahrungsdauer von Ereignisdaten | &nbsp; |1 Tag |1–7 Tage |
| Maximale Durchsatzeinheiten |Bei einer Überschreitung des Grenzwerts für Durchsatzeinheiten werden Ihre Daten gedrosselt, und es wird eine [ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) ausgelöst. Um eine höhere Anzahl von Durchsatzeinheiten für den Tarif „Standard“ anzufordern, erstellen Sie eine [Supportanfrage](/azure/azure-portal/supportability/how-to-create-azure-support-request). [Zusätzliche Durchsatzeinheiten](../articles/event-hubs/event-hubs-auto-inflate.md) sind für einen festgelegten Kaufpreis in 20er-Blöcken verfügbar. |20 | 20 | 

### <a name="dedicated-tier"></a>Dedizierte Ebene
Das Event Hubs Dedicated-Angebot wird zu einem festen Monatspreis mit einem Minimum von vier Stunden Nutzung in Rechnung gestellt. Der Dedicated-Tarif umfasst die Funktionen des Standard-Tarifs, jedoch mit Kapazitäten und Limits auf Unternehmensniveau für Kunden mit anspruchsvollen Workloads. 

| Funktion | Einschränkungen |
| --- | ---|
| Bandbreite |  20 CUs |
| Namespaces | 50 pro CU |
| Event Hubs |  1000 pro Namespace |
| Nachrichtengröße | 1 MB |
| Partitionen | 2000 pro CU |
| Verbrauchergruppen | Kein Limit pro CU, 1000 pro Event Hub |
| Brokerverbindungen | 100.000 enthalten |
| Nachrichtenaufbewahrungsdauer | 90 Tage, 10 TB enthalten pro CU |
| Eingangsereignisse | Enthalten |
| Erfassung | Enthalten |


### <a name="schema-registry-limitations"></a>Einschränkungen der Schemaregistrierung

#### <a name="limits-that-are-the-same-for-standard-and-dedicated-tiers"></a>Grenzwerte, die für die Ebenen **Standard** und **Dedicated** identisch sind 
| Funktion | Begrenzung | 
| --- |  --- | -- |
| Maximale Länge eines Schemagruppennamens | 50 |  
| Maximale Länge eines Schemanamens | 100 |    
| Größe in Bytes pro Schema | 1 MB |   
| Anzahl von Eigenschaften pro Schemagruppe | 1024 |
| Größe in Bytes pro Gruppeneigenschaftenschlüssel | 256 | 
| Größe in Bytes pro Gruppeneigenschaftenschlüssel | 1024 | 


#### <a name="limits-that-are-different-for-standard-and-dedicated-tiers"></a>Grenzwerte, die sich für die Ebenen **Standard** und **Dedicated** unterscheiden 

| Begrenzung | Standard | Dediziert | 
| --- |  --- | -- | --- |
| Größe der Schemaregistrierung (Namespace) in Megabytes | 25 |  1024 |
| Anzahl von Schemagruppen in einer Schemaregistrierung (Namespace)| 1 (ohne Standardgruppe) | 1000 |
| Anzahl von Schemaversionen in allen Schemagruppen | 25 | 10000 |





