---
title: include file
description: include file
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 01/15/2020
ms.custom: include file
ms.openlocfilehash: cb43c8b8c952d8db6cf450a7015c22c85e7fe4b5
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2020
ms.locfileid: "76268282"
---
`objectIdType` (oder **Objektbezeichnertyp**) bezieht sich auf den Typ der Identität, der einer Rolle zugewiesen wird. Mit Ausnahme der Typen `DeviceId` und `UserDefinedFunctionId` entsprechen Objektbezeichnertypen Eigenschaften von Azure Active Directory-Objekten.

Die folgende Tabelle enthält die unterstützten Objektbezeichnertypen in Azure Digital Twins:

| type | Beschreibung |
| --- | --- |
| UserId | Weist einem Benutzer eine Rolle zu. |
| deviceId | Weist einem Gerät eine Rolle zu. |
| DomainName | Weist einem Domänennamen eine Rolle zu. Jeder Benutzer mit dem angegebenen Domänennamen hat die Zugriffsrechte der entsprechenden Rolle. |
| TenantId | Weist einem Mandanten eine Rolle zu. Jeder Benutzer, der zu der angegebenen Azure AD-Mandanten-ID gehört, hat die Zugriffsrechte der entsprechenden Rolle. |
| ServicePrincipalId | Weist einer Dienstprinzipalobjekt-ID eine Rolle zu. |
| UserDefinedFunctionId | Weist einer benutzerdefinierten Funktion (User-defined Function, UDF) eine Rolle zu. |