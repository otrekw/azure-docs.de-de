---
title: API-Überlegungen – Azure Marketplace
description: Versionsverwaltung, Fehlerbehandlung und Autorisierungsprobleme beim Verwenden der Marketplace-APIs.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 0920bec160874f27c8b1b6f2132951b57719b31c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88035279"
---
# <a name="api-considerations"></a>API-Überlegungen

<a name="api-versioning"></a>API-Versionsverwaltung
--------------

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und funktionieren auch weiterhin. Der Übergang führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](./cloud-partner-portal-api-overview.md) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach dem Übergang zu Partner Center weiterhin funktioniert. CPP-APIs sollten nur für vorhandene Produkte verwendet werden, die bereits vor dem Übergang zu Partner Center integriert wurden. Neue Produkte sollten die Partner Center-APIs für die Übermittlung verwenden.

Es gibt möglicherweise mehrere Versionen der API, die gleichzeitig verfügbar sind. Clients müssen angeben, welche Version sie aufrufen möchten. Dazu wird in ihnen der `api-version`-Parameter als Bestandteil der Abfragezeichenfolge bereitgestellt.

   `GET https://cloudpartner.azure.com/api/offerTypes?api-version=2017-10-31`

Die Antwort auf eine Anforderung mit einer unbekannten oder ungültigen API-Version ist ein HTTP-Code von 400. Bei diesem Fehler wird die Auflistung der bekannten API-Versionen im Antworttext zurückgegeben.

``` json
    {
        "error": { 
            "code":"InvalidAPIVersion",
            "message":"Invalid api version. Allowed values are [2016-08-01-preview]"
        }
    }
```            

<a name="errors"></a>Errors
------

Die API reagiert auf Fehler mit den entsprechenden HTTP-Statuscodes und ggf. zusätzlichen Informationen in der Antwort, die als JSON serialisiert ist.
Wenn Sie eine Fehlermeldung erhalten, insbesondere einen Fehler der Klasse 400, sollten Sie die Anforderung erst erneut versuchen, nachdem Sie die Fehlerursache beseitigt haben. Korrigieren Sie z. B. in der obigen Beispielantwort den API-Versionsparameter, bevor Sie die Anforderung erneut senden.

<a name="authorization-header"></a>Authorization header (Autorisierungsheader)
--------------------

Für alle APIs in dieser Referenz müssen Sie den Autorisierungsheader zusammen mit dem Bearer-Token übergeben, das aus Azure Active Directory (Azure AD) abgerufen wurde. Dieser Header ist erforderlich, um eine gültige Antwort zu empfangen. Ist er nicht vorhanden, wird ein `401 Unauthorized`-Fehler zurückgegeben. 

``` HTTP
  GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview

    Accept: application/json 
    Authorization: Bearer <YOUR_TOKEN> 
```
