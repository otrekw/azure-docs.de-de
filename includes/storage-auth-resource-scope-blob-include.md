---
title: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/07/2021
ms.author: tamram
ms.openlocfilehash: 24bc49b82c24c560dea12c744097f643bec2b3f5
ms.sourcegitcommit: f9e368733d7fca2877d9013ae73a8a63911cb88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111904406"
---
Bevor Sie einem Sicherheitsprinzipal eine Azure RBAC-Rolle zuweisen, legen Sie den Zugriffsbereich fest, den der Sicherheitsprinzipal haben soll. Es hat sich als am besten bewährt, stets nur den kleinstmöglichen Umfang an Zugriffsrechten zu gewähren. Azure RBAC-Rollen, die in einem umfassenderen Bereich definiert sind, werden von den darunterliegenden Ressourcen geerbt.

In der folgenden Liste werden die Ebenen beschrieben, auf denen Sie den Zugriff auf Azure-Blobressourcen einschränken können, beginnend mit dem kleinstmöglichen Bereich:

- **Ein einzelner Container**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Blobs im Container sowie für Containereigenschaften und Metadaten.
- **Das Speicherkonto**. Bei diesem Bereich gilt eine Rollenzuweisung für alle Container und deren Blobs.
- **Die Ressourcengruppe**. Bei diesem Bereich gilt eine Rollenzuweisung für alle Container in allen Speicherkonten der Ressourcengruppe.
- **Das Abonnement**. Bei diesem Bereich gilt eine Rollenzuweisung für alle Container in allen Speicherkonten in allen Ressourcengruppen des Abonnements.
- **Eine Verwaltungsgruppe**. Bei diesem Bereich gilt eine Rollenzuweisung für alle Container in allen Speicherkonten in allen Ressourcengruppen und allen Abonnements in der Ressourcengruppe.

Weitere Informationen zum Bereich für Azure RBAC-Rollenzuweisungen finden Sie unter [Grundlegendes zum Bereich für Azure RBAC](../articles/role-based-access-control/scope-overview.md).
