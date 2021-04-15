---
title: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 02/10/2021
ms.author: tamram
ms.openlocfilehash: 483f5853c321eee4ac6d10543f0e360a0a5e54b9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100373748"
---
Bevor Sie einem Sicherheitsprinzipal eine Azure RBAC-Rolle zuweisen, legen Sie den Zugriffsbereich fest, den der Sicherheitsprinzipal haben soll. Es hat sich als am besten bewährt, stets nur den kleinstmöglichen Umfang an Zugriffsrechten zu gewähren. Azure RBAC-Rollen, die in einem umfassenderen Bereich definiert sind, werden von den darunterliegenden Ressourcen geerbt.

In der folgenden Liste werden die Ebenen beschrieben, auf denen Sie den Zugriff auf Azure-Blob- und -Warteschlangenressourcen einschränken können, beginnend mit dem kleinstmöglichen Umfang:

- **Ein einzelner Container**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Blobs im Container sowie für Containereigenschaften und Metadaten.
- **Eine einzelne Warteschlange**. Bei diesem Umfang gilt eine Rollenzuweisung für Nachrichten in der Warteschlange sowie für Warteschlangeneigenschaften und Metadaten.
- **Das Speicherkonto**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Container und deren Blobs oder für alle Warteschlangen und deren Nachrichten.
- **Die Ressourcengruppe**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Container oder Warteschlangen in allen Speicherkonten der Ressourcengruppe.
- **Das Abonnement**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Container oder Warteschlangen in allen Speicherkonten in allen Ressourcengruppen des Abonnements.
- **Eine Verwaltungsgruppe**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Container oder Warteschlangen in allen Speicherkonten in allen Ressourcengruppen und allen Abonnements in der Ressourcengruppe.

Weitere Informationen zu Azure-Rollenzuweisungen und zum Umfang finden Sie unter [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../articles/role-based-access-control/overview.md).
