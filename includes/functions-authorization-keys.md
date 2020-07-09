---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 926434d7110877e234888682cb6c946afe3ae685
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648949"
---
Mit Functions können Sie Schlüssel verwenden, wodurch der Zugriff auf Ihre HTTP-Funktionsendpunkte während der Entwicklung erschwert wird. Sofern die HTTP-Zugriffsebene für eine über HTTP ausgelöste Funktion nicht auf `anonymous` festgelegt ist, müssen Anforderungen einen API-Zugriffsschlüssel in der Anforderung enthalten. 

#### <a name="authorization-scopes-function-level"></a>Autorisierungsbereiche (Funktionsebene)

Es gibt zwei Zugriffsbereiche für Schlüssel auf Funktionsebene:

* **Funktion**: Diese Schlüssel gelten nur für die Funktionen, für die sie definiert sind. Bei Verwendung als API-Schlüssel ermöglichen diese nur Zugriff auf diese spezielle Funktion.

* **Host**: Schlüssel mit einem Hostbereich können für den Zugriff auf alle Funktionen in der Funktions-App verwendet werden. Bei Verwendung als API-Schlüssel ermöglichen diese Zugriff auf jede Funktion in der Funktionen-App. 

Jeder Schlüssel ist zu Referenzzwecken benannt. Auf Funktions- und Hostebene gibt es einen Standardschlüssel (mit dem Namen „default“). Funktionsschlüssel haben Vorrang vor Hostschlüsseln. Wenn zwei Schlüssel mit dem gleichen Namen definiert wurden, wird immer der Funktionsschlüssel verwendet.

#### <a name="master-key-admin-level"></a>Hauptschlüssel (Administratorebene) 

Jede Funktions-App verfügt außerdem über einen Hostschlüssel auf Administratorebene mit dem Namen `_master`. Zusätzlich zur Bereitstellung des Zugriffs auf Hostebene auf alle Funktionen in der App bietet der Hauptschlüssel auch administrativen Zugriff auf die Laufzeit-REST-APIs. Dieser Schlüssel kann nicht widerrufen werden. Wenn Sie die Zugriffsebene `admin` festlegen, muss für Anforderungen der Hauptschlüssel verwendet werden. Alle anderen Schlüssel führen zu einem Zugriffsfehler.

> [!CAUTION]  
> Aufgrund der erhöhten Berechtigungen, die der Hauptschlüssel in Ihrer Funktions-App gewährt, sollten Sie diesen Schlüssel nicht für Dritte freigeben oder in nativen Clientanwendungen verteilen. Gehen Sie umsichtig vor, wenn Sie die Zugriffsebene „Administrator“ auswählen.
