---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/17/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 74ef8270b5efcd3b7cdf756c103dcc2e1c935508
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75460499"
---
Bevor Sie einem Sicherheitsprinzipal eine RBAC-Rolle zuweisen, legen Sie den Zugriffsbereich fest, den der Sicherheitsprinzipal haben soll. Es hat sich als am besten bewährt, stets nur den kleinstmöglichen Umfang an Zugriffsrechten zu gewähren.

In der folgenden Liste werden die Ebenen beschrieben, auf denen Sie den Zugriff auf Azure-Blob- und -Warteschlangenressourcen einschränken können, beginnend mit dem kleinstmöglichen Umfang:

- **Ein einzelner Container**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Blobs im Container sowie für Containereigenschaften und Metadaten.
- **Eine einzelne Warteschlange**. Bei diesem Umfang gilt eine Rollenzuweisung für Nachrichten in der Warteschlange sowie für Warteschlangeneigenschaften und Metadaten.
- **Das Speicherkonto**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Container und deren Blobs oder für alle Warteschlangen und deren Nachrichten.
- **Die Ressourcengruppe**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Container oder Warteschlangen in allen Speicherkonten der Ressourcengruppe.
- **Das Abonnement**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Container oder Warteschlangen in allen Speicherkonten in allen Ressourcengruppen des Abonnements.

> [!IMPORTANT]
> Wenn Ihr Abonnement einen Azure DataBricks-Namespace enthält, wird Rollen, die auf das Abonnement begrenzt sind, kein Zugriff auf Blob- und Warteschlangendaten gewährt. Legen Sie den Umfang von Rollen stattdessen auf die Ressourcengruppe, das Speicherkonto, den Container oder die Warteschlange fest.     
