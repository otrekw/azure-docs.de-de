---
title: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/07/2021
ms.author: tamram
ms.openlocfilehash: 7f2b15a57b42f87e749060f510dafff732a1d9e0
ms.sourcegitcommit: f9e368733d7fca2877d9013ae73a8a63911cb88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111904417"
---
Bevor Sie einem Sicherheitsprinzipal eine Azure RBAC-Rolle zuweisen, legen Sie den Zugriffsbereich fest, den der Sicherheitsprinzipal haben soll. Es hat sich als am besten bewährt, stets nur den kleinstmöglichen Umfang an Zugriffsrechten zu gewähren. Azure RBAC-Rollen, die in einem umfassenderen Bereich definiert sind, werden von den darunterliegenden Ressourcen geerbt.

In der folgenden Liste werden die Ebenen beschrieben, auf denen Sie den Zugriff auf Azure-Warteschlangenressourcen einschränken können, beginnend mit dem kleinstmöglichen Umfang:

- **Eine einzelne Warteschlange**. Bei diesem Umfang gilt eine Rollenzuweisung für Nachrichten in der Warteschlange sowie für Warteschlangeneigenschaften und Metadaten.
- **Das Speicherkonto**. Bei diesem Bereich gilt eine Rollenzuweisung für alle Warteschlangen und deren Nachrichten.
- **Die Ressourcengruppe**. Bei diesem Bereich gilt eine Rollenzuweisung für alle Warteschlangen in allen Speicherkonten der Ressourcengruppe.
- **Das Abonnement**. Bei diesem Bereich gilt eine Rollenzuweisung für alle Warteschlangen in allen Speicherkonten in allen Ressourcengruppen des Abonnements.
- **Eine Verwaltungsgruppe**. Bei diesem Bereich gilt eine Rollenzuweisung für alle Warteschlangen in allen Speicherkonten in allen Ressourcengruppen in allen Abonnements in der Ressourcengruppe.

Weitere Informationen zum Bereich für Azure RBAC-Rollenzuweisungen finden Sie unter [Grundlegendes zum Bereich für Azure RBAC](../articles/role-based-access-control/scope-overview.md).
