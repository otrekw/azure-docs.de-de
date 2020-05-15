---
title: Azure Service Bus-Themenfilter | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Abonnenten definieren können, welche Nachrichten von einem Thema empfangen werden sollen, indem Filter angegeben werden.
services: service-bus-messaging
documentationcenter: ''
author: spelluru
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/16/2020
ms.author: spelluru
ms.openlocfilehash: 6e780268d4b8c1a512ce82b1ca10a2f6b7b894b7
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83125756"
---
# <a name="topic-filters-and-actions"></a>Themenfilter und -aktionen

Abonnenten können definieren, welche Nachrichten von einem Thema empfangen werden sollen. Diese Nachrichten werden in Form von benannten Abonnementregeln angegeben. Jede Regel besteht aus einer Bedingung, die bestimmte Nachrichten auswählt, und einer Aktion, die die ausgewählte Nachricht kommentiert. Für jede übereinstimmende Regelbedingung erzeugt das Abonnement eine Kopie der Nachricht, die für jede übereinstimmende Regel anders kommentiert werden kann.

Jedes neu erstellte Themenabonnement weist eine anfängliche Standardabonnementregel auf. Wenn Sie nicht explizit eine Filterbedingung für die Regel angeben, wird der Filter **true** angewendet, mit dem alle Nachrichten in das Abonnement gewählt werden können. Der Standardregel ist keine Anmerkungsaktion zugeordnet.

Service Bus unterstützt drei Filterbedingungen:

-   *Boolesche Filter* – **TrueFilter** und **FalseFilter** sorgen dafür, dass entweder alle eingehenden Nachrichten (**true**) oder keine der eingehenden Nachrichten (**false**) für das Abonnement ausgewählt werden.

-   *SQL-Filter* – ein **SqlFilter** enthält einen SQL-ähnlichen bedingten Ausdruck, der im Broker für die benutzerdefinierten Eigenschaften und Systemeigenschaften des eingehenden Nachrichtenstroms ausgewertet wird. Allen Systemeigenschaften muss das Präfix `sys.` im bedingten Ausdruck vorangestellt werden. Die [SQL-Sprachteilmenge für Filterbedingungen](service-bus-messaging-sql-filter.md) ermöglicht das Überprüfen des Vorhandenseins von Eigenschaften (`EXISTS`), Nullwerten (`IS NULL`), logischen Beziehungen (NOT/AND/OR), einfacher numerischer Arithmetik und einfachen Textmustervergleichen mit `LIKE`.

-   *Korrelationsfilter* – ein **CorrelationFilter** enthält eine Reihe von Bedingungen, die für Benutzer- und Systemeigenschaften einer eingehenden Nachricht überprüft werden. Er wird häufig zum Vergleichen der Eigenschaft **CorrelationId** verwendet. Die Anwendung kann aber auch die folgenden Eigenschaften vergleichen:

    - **ContentType**
     - **Label**
     - **MessageId**
     - **ReplyTo**
     - **ReplyToSessionId**
     - **SessionId** 
     - **An**
     - beliebige benutzerdefinierte Eigenschaften. 
     
     Eine Übereinstimmung liegt vor, wenn der Wert einer Eigenschaft einer eingehenden Nachricht gleich dem im Korrelationsfilter angegebenen Wert ist. Für Zeichenfolgenausdrücke wird beim Vergleich die Groß-/Kleinschreibung beachtet. Wenn mehrere Übereinstimmungseigenschaften angegeben werden, kombiniert der Filter sie in einer logischen AND-Bedingung, d.h., der Filter stimmt dann überein, wenn alle Bedingungen erfüllt sind.

Alle Filter werten Nachrichteneigenschaften aus. Der Nachrichtentext kann von Filtern nicht ausgewertet werden.

Komplexe Filterregeln erfordern Verarbeitungskapazitäten. Insbesondere bewirkt die Verwendung von SQL-Filterregeln einen geringeren Gesamtnachrichtendurchsatz auf Ebene des Abonnements, des Themas und des Namespace. Nach Möglichkeit sollten Anwendungen eher Korrelationsfilter als SQL-ähnliche Filter verwenden, weil sie erheblich effizienter verarbeitet werden und geringere Auswirkungen auf den Durchsatz haben.

## <a name="actions"></a>Aktionen

Mit SQL-Filterbedingungen können Sie eine Aktion definieren, die die Nachricht durch Hinzufügen, Entfernen oder Ersetzen von Eigenschaften und deren Werten kommentieren kann. Die Aktion [verwendet einen SQL-ähnlichen Ausdruck](service-bus-messaging-sql-filter.md), der grob an die Syntax der SQL UPDATE-Anweisung angelehnt ist. Die Aktion wird für die Nachricht angewendet, nachdem eine Übereinstimmung dafür gefunden wurde und bevor die Nachricht für das Abonnement ausgewählt wird. Die Änderungen an den Nachrichteneigenschaften sind innerhalb der in das Abonnement kopierten Nachricht privat.

## <a name="usage-patterns"></a>Verwendungsmuster

Das einfachste Verwendungsszenario für ein Thema ist, dass jedes Abonnement eine Kopie aller an ein Thema gesendeten Nachrichten erhält, sodass ein Broadcastmuster ermöglicht wird.

Filter und Aktionen ermöglichen zwei weitere Gruppen von Mustern: Partitionierung und Weiterleitung.

Bei der Partitionierung werden Nachrichten mithilfe von Filtern auf vorhersagbare Weise und unter Ausschluss von Doppelungen an mehrere vorhandene Themenabonnements verteilt. Das Partitionierungsmuster wird verwendet, wenn ein System viele unterschiedliche Kontexte in funktionell identischen Abteilungen bearbeitet, wobei in jeder Abteilung eine Teilmenge der gesamten Daten vorhanden ist, z.B. Kundenprofilinformationen. Mit Partitionierung übermittelt ein Herausgeber die Nachricht an ein Thema, ohne dass Kenntnisse des Partitionierungsmodells erforderlich sind. Die Nachricht wird dann in das richtige Abonnement verschoben, aus dem es anschließend vom Meldungshandler der Partition abgerufen werden kann.

Bei der Weiterleitung werden Nachrichten mit Filtern auf vorhersehbare Weise auf Themenabonnements verteilt. Dabei müssen die Nachrichten jedoch nicht ausschließlich in einem Abonnement vorhanden sein. In Verbindung mit dem Feature für die [automatische Weiterleitung](service-bus-auto-forwarding.md) können mit Themenfiltern komplexer Weiterleitungsgraphen innerhalb eines Service Bus-Namespace erstellt werden, über die die Nachrichtenverteilung in einer Azure-Region erfolgt. Mit Azure Functions oder Azure Logic Apps als Brücke zwischen Azure Service Bus-Namespaces können Sie komplexe globale Topologien mit direkter Integration in Branchenanwendungen erstellen.


> [!NOTE]
> Zurzeit können Sie im Azure-Portal keine Filterregeln für Abonnements angeben. Sie können alle unterstützten SDKs oder Azure Resource Manager Vorlagen verwenden, um Abonnementregeln zu definieren. 

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die folgenden Beispiele an: 

- [.NET – Grundlegendes Tutorial zum Senden und Empfangen mit Filtern](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/BasicSendReceiveTutorialwithFilters/BasicSendReceiveTutorialWithFilters)
- [.NET – Themenfilter](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TopicFilters)
- [Azure Resource Manager-Vorlage](https://docs.microsoft.com/azure/templates/microsoft.servicebus/2017-04-01/namespaces/topics/subscriptions/rules)


