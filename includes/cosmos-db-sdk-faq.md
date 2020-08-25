---
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 8/12/2020
ms.author: sngun
ms.openlocfilehash: 5c5ed4e1bbe54c642202c19e1beb61de94b4f751
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515211"
---
**1. Wie werden Kunden über die Deaktivierung eines SDK benachrichtigt?**

Um einen reibungslosen Übergang zu einem unterstützten SDK zu ermöglichen, informiert Microsoft die Kunden 12 Monate im Voraus über das Ende des Supports des einzustellenden SDK. Darüber hinaus werden Kunden über verschiedene Kommunikationskanäle benachrichtigt: Azure-Portal, Azure-Updates und direkte Kommunikation mit den entsprechenden Dienstadministratoren.

**2. Können Kunden mit einem Azure Cosmos DB SDK, das eingestellt werden soll, während des 12-monatigen Zeitraums Anwendungen erstellen?** 

Ja, mit einem solchen Azure Cosmos DB SDK können Kunden während der 12-monatigen Frist Anwendungen bei vollem Zugriff erstellen, bereitstellen und ändern. Während der 12-monatigen Frist wird Kunden empfohlen, ggf. zu einer neueren unterstützten Version des Azure Cosmos DB SDK zu migrieren. 

**3. Was geschieht nach dem Deaktivierungsdatum mit Anwendungen, die das nicht unterstützte Azure Cosmos DB SDK verwenden?** 

Nach dem Deaktivierungsdatum werden von Azure Cosmos DB für die eingestellten SDK-Versionen keine Fehlerbehebungen mehr durchgeführt und keine neuen Funktionen hinzugefügt, und es wird auch keine Unterstützung mehr dafür bereitgestellt. Wenn Sie kein Upgrade durchführen möchten, werden von den eingestellten Versionen des SDK gesendete Anforderungen weiterhin vom Azure Cosmos DB-Dienst bedient. 

**4. Welche SDK-Versionen werden über die neuesten Funktionen und Updates verfügen?**

Neue Funktionen und Updates werden nur der aktuellen Nebenversion der neuesten unterstützten SDK-Hauptversion hinzugefügt. Es wird empfohlen, immer die aktuelle Version zu verwenden, um die Vorteile von neuen Funktionen, Leistungsverbesserungen und Fehlerbehebungen nutzen zu können. Wenn Sie eine alte, noch aktivierte Version des SDK verwenden, funktionieren Ihre Anforderungen an Azure Cosmos DB weiterhin, aber Sie haben keinen Zugriff auf neue Funktionen.  

**5. Was muss ich tun, wenn ich meine Anwendung nicht vor einem Stichtag aktualisieren kann?**

Es wird empfohlen, so früh wie möglich auf das neueste SDK zu aktualisieren. Nachdem ein SDK für die Einstellung markiert wurde, bleiben Ihnen 12 Monate zur Aktualisierung Ihrer Anwendung. Wenn Sie die Aktualisierung nicht bis zum Deaktivierungsdatum vornehmen können, werden von den eingestellten Versionen des SDK gesendete Anforderungen weiterhin von Azure Cosmos DB bedient, sodass Ihre ausgeführten Anwendungen weiterhin funktionieren. Für die eingestellten SDK-Versionen werden jedoch von Azure Cosmos DB keine Fehlerbehebungen mehr durchgeführt und keine neuen Funktionen hinzugefügt, und es wird auch keine Unterstützung mehr dafür bereitgestellt. 

Wenn Sie über einen Supportplan verfügen und technischen Support benötigen, [kontaktieren Sie uns](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview), indem Sie ein Supportticket erstellen.
    


