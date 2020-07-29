---
title: Abfragesprache
titleSuffix: Azure Digital Twins
description: In diesem Artikel werden Ihnen die Grundlagen der Azure Digital Twins-Abfragespeichersprache vermittelt.
author: baanders
ms.author: baanders
ms.date: 3/26/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: b3ed5d6605097b31dfaa58a2d37e71d3a6702bee
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537491"
---
# <a name="about-the-query-language-for-azure-digital-twins"></a>Abfragesprache für Azure Digital Twins

Beachten Sie, dass im Mittelpunkt von Azure Digital Twins der [**Zwillingsgraph**](concepts-twins-graph.md) steht, der sich aus **digitalen Zwillingen** und **Beziehungen** zusammensetzt. Dieser Graph kann abgefragt werden, um Informationen zu den darin enthaltenen digitalen Zwillingen und Beziehungen abzurufen. Diese Abfragen werden in einer benutzerdefinierten SQL-ähnlichen Abfragesprache namens **Azure Digital Twins-Abfragespeichersprache** geschrieben.

Wenn Sie eine Abfrage aus einer Client-App an den Dienst senden möchten, verwenden Sie die Azure Digital Twins-[**Abfrage-API**](https://docs.microsoft.com/dotnet/api/azure.digitaltwins.core.digitaltwinsclient.query?view=azure-dotnet-preview). Damit können Entwickler Abfragen schreiben und Filter anwenden, um Gruppen von digitalen Zwillingen im Zwillingsgraphen sowie andere Informationen zum Azure Digital Twins-Szenario zu finden.

## <a name="query-language-features"></a>Funktionen der Abfragesprache

Azure Digital Twins bietet umfassende Abfragefunktionen für den Zwillingsgraphen. Abfragen werden mithilfe einer SQL-ähnlichen Syntax in einer Abfragesprache beschrieben, die der [IoT Hub-Abfragesprache](../iot-hub/iot-hub-devguide-query-language.md) mit vielen vergleichbaren Features ähnelt.

> [!NOTE]
> Bei allen Azure Digital Twins-Abfragevorgängen muss die Groß-/Kleinschreibung beachtet werden.

Diese Vorgänge sind in der Azure Digital Twins-Abfragespeichersprache verfügbar:
* Sie können Zwillinge anhand von Digital Twins-Eigenschaften abrufen.
* Sie können Zwillinge anhand von Digital Twins-Schnittstellen abrufen.
* Sie können Zwillinge anhand von Beziehungseigenschaften abrufen.
* Sie können Zwillinge über mehrere Beziehungstypen (`JOIN`-Abfragen) abrufen. Es gelten Einschränkungen hinsichtlich der zulässigen Anzahl von `JOIN`s (eine Ebene für die öffentliche Vorschauversion).
* Verwenden Sie die benutzerdefinierte Funktion `IS_OF_MODEL(twinCollection, twinTypeName)`, die das Filtern anhand des [Modells](concepts-models.md) des Zwillings ermöglicht. Die Vererbung wird unterstützt.
* Verwenden Sie Skalarfunktionen: `IS_BOOL`, `IS_DEFINED`, `IS_NULL`, `IS_NUMBER`, `IS_OBJECT`, `IS_PRIMITIVE`, `IS_STRING`, `STARTS_WITH`, `ENDS_WITH`.
* Verwenden Sie Vergleichsoperatoren für Abfragen: `IN`/`NIN`, `=`, `!=`, `<`, `>`, `<=`, `>=`.
* Sie können eine beliebige Kombination (Operatoren `AND`, `OR`, `NOT`) der obigen Vorgänge ausführen.
* Verwenden Sie die Fortsetzung: Das Abfrageobjekt wird mit einer Seitengröße (bis zu 100) instanziiert. Sie können die digitalen Zwillinge einer Seite gleichzeitig abrufen, indem Sie das Fortsetzungstoken in nachfolgenden Aufrufen der API bereitstellen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das Schreiben von Abfragen, und untersuchen Sie Clientcodebeispiele unter [*Abfragen des Zwillingsdiagramms von Azure Digital Twins*](how-to-query-graph.md).
