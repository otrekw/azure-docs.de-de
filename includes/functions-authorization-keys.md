---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 046bd8fcbb8fab50269c8d35da0956bdc63f2304
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85298598"
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
