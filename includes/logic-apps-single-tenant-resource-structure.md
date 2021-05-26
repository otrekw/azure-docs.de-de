---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 05/25/2021
ms.openlocfilehash: 88d0292048edcc6846d0e1637b79f115d1321e70
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110369110"
---
Im mehrinstanzenfähigen Modell kann die Logik-App-Ressourcenstruktur nur einen einzelnen Workflow enthalten. Aufgrund dieser 1:1-Beziehung werden Logik-App und Workflow häufig als synonym betrachtet und entsprechend referenziert. Im Modell mit einzelnem Mandanten kann die Ressourcenstruktur der Logik-App jedoch mehrere Workflows umfassen. Diese 1:N-Beziehung bedeutet, dass Workflows in derselben Logik-App andere Ressourcen freigeben und wiederverwenden können. Workflows in derselben Logik-App und demselben Mandanten bieten aufgrund dieses freigegebenen Mandanten und der Nähe zueinander auch eine verbesserte Leistung. Diese Ressourcenstruktur ähnelt Azure Functions, wo eine Funktions-App viele Funktionen hosten kann.

Weitere Informationen und bewährte Methoden zum Organisieren von Workflows, Leistung und Skalierung in Ihrer Logik-App finden Sie im ähnlichen [Leitfaden für Azure Functions](../articles/azure-functions/functions-best-practices.md), den Sie im Allgemeinen auf Azure Logic Apps mit einzelnem Mandanten anwenden können.