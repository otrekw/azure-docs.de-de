---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 617608f382d3331e59ae92c9eb272347c736b768
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91828960"
---
Mit Functions können Sie Schlüssel verwenden, wodurch der Zugriff auf Ihre HTTP-Funktionsendpunkte während der Entwicklung erschwert wird. Sofern die HTTP-Zugriffsebene für eine über HTTP ausgelöste Funktion nicht auf `anonymous` festgelegt ist, müssen Anforderungen einen API-Zugriffsschlüssel in der Anforderung enthalten. 

Schlüssel bieten zwar einen Standardsicherheitsmechanismus, für den Schutz eines HTTP-Endpunkts in der Produktion sollten jedoch gegebenenfalls zusätzliche Optionen in Erwägung gezogen werden. So ist es beispielsweise in der Regel nicht empfehlenswert, den gemeinsamen geheimen Schlüssel in öffentlichen Apps zu verteilen. Wenn Ihre Funktion über einen öffentlichen Client aufgerufen wird, empfiehlt es sich gegebenenfalls, einen anderen Sicherheitsmechanismus zu implementieren. Weitere Informationen hierzu finden Sie unter [Schützen eines HTTP-Endpunkts in einer Produktionsumgebung](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#secure-an-http-endpoint-in-production).

Wenn Sie Ihre Funktionsschlüsselwerte erneuern, müssen die aktualisierten Schlüsselwerte manuell an alle Clients verteilt werden, von denen Ihre Funktion aufgerufen wird.  

#### <a name="authorization-scopes-function-level"></a>Autorisierungsbereiche (Funktionsebene)

Es gibt zwei Zugriffsbereiche für Schlüssel auf Funktionsebene:

* **Funktion**: Diese Schlüssel gelten nur für die Funktionen, für die sie definiert sind. Bei Verwendung als API-Schlüssel ermöglichen diese nur Zugriff auf diese spezielle Funktion.

* **Host**: Schlüssel mit einem Hostbereich können für den Zugriff auf alle Funktionen in der Funktions-App verwendet werden. Bei Verwendung als API-Schlüssel ermöglichen diese Zugriff auf jede Funktion in der Funktionen-App. 

Jeder Schlüssel ist zu Referenzzwecken benannt. Auf Funktions- und Hostebene gibt es einen Standardschlüssel (mit dem Namen „default“). Funktionsschlüssel haben Vorrang vor Hostschlüsseln. Wenn zwei Schlüssel mit dem gleichen Namen definiert wurden, wird immer der Funktionsschlüssel verwendet.

#### <a name="master-key-admin-level"></a>Hauptschlüssel (Administratorebene) 

Jede Funktions-App verfügt außerdem über einen Hostschlüssel auf Administratorebene mit dem Namen `_master`. Zusätzlich zur Bereitstellung des Zugriffs auf Hostebene auf alle Funktionen in der App bietet der Hauptschlüssel auch administrativen Zugriff auf die Laufzeit-REST-APIs. Dieser Schlüssel kann nicht widerrufen werden. Wenn Sie die Zugriffsebene `admin` festlegen, muss für Anforderungen der Hauptschlüssel verwendet werden. Alle anderen Schlüssel führen zu einem Zugriffsfehler.

> [!CAUTION]  
> Aufgrund der erhöhten Berechtigungen, die der Hauptschlüssel in Ihrer Funktions-App gewährt, sollten Sie diesen Schlüssel nicht für Dritte freigeben oder in nativen Clientanwendungen verteilen. Gehen Sie umsichtig vor, wenn Sie die Zugriffsebene „Administrator“ auswählen.
