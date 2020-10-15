---
title: Abfragesprache
titleSuffix: Azure Digital Twins
description: Grundlagen der Azure Digital Twins-Abfragesprache
author: baanders
ms.author: baanders
ms.date: 3/26/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: d656f19f6f4030025ff1393c3e5017466b3333fd
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92044393"
---
# <a name="about-the-query-language-for-azure-digital-twins"></a>Abfragesprache für Azure Digital Twins

Beachten Sie, dass im Mittelpunkt von Azure Digital Twins der [**Zwillingsgraph**](concepts-twins-graph.md) steht, der sich aus **digitalen Zwillingen** und **Beziehungen** zusammensetzt. Dieser Graph kann abgefragt werden, um Informationen zu den darin enthaltenen digitalen Zwillingen und Beziehungen abzurufen. Diese Abfragen werden in einer benutzerdefinierten SQL-ähnlichen Abfragesprache mit der Bezeichnung **Azure Digital Twins-Abfragesprache** geschrieben.

Wenn Sie eine Abfrage aus einer Client-App an den Dienst senden möchten, verwenden Sie die Azure Digital Twins-[**Abfrage-API**](/dotnet/api/azure.digitaltwins.core.digitaltwinsclient.query?view=azure-dotnet-preview). Damit können Entwickler Abfragen schreiben und Filter anwenden, um Gruppen von digitalen Zwillingen im Zwillingsgraphen sowie andere Informationen zum Azure Digital Twins-Szenario zu finden.

[!INCLUDE [digital-twins-query-operations.md](../../includes/digital-twins-query-operations.md)]

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das Schreiben von Abfragen, und untersuchen Sie Clientcodebeispiele unter [*Abfragen des Zwillingsdiagramms von Azure Digital Twins*](how-to-query-graph.md).