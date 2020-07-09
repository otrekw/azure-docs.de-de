---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/06/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 8b97a62626666fa39a5b0622852d9eec47c2410a
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86024888"
---
Bevor Sie einem Sicherheitsprinzipal eine RBAC-Rolle zuweisen, legen Sie den Zugriffsbereich fest, den der Sicherheitsprinzipal haben soll. Es hat sich als am besten bewährt, stets nur den kleinstmöglichen Umfang an Zugriffsrechten zu gewähren.

In der folgenden Liste werden die Ebenen beschrieben, auf denen Sie den Zugriff auf Azure-Blob- und -Warteschlangenressourcen einschränken können, beginnend mit dem kleinstmöglichen Umfang:

- **Ein einzelner Container**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Blobs im Container sowie für Containereigenschaften und Metadaten.
- **Eine einzelne Warteschlange**. Bei diesem Umfang gilt eine Rollenzuweisung für Nachrichten in der Warteschlange sowie für Warteschlangeneigenschaften und Metadaten.
- **Das Speicherkonto**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Container und deren Blobs oder für alle Warteschlangen und deren Nachrichten.
- **Die Ressourcengruppe**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Container oder Warteschlangen in allen Speicherkonten der Ressourcengruppe.
- **Das Abonnement**. Bei diesem Umfang gilt eine Rollenzuweisung für alle Container oder Warteschlangen in allen Speicherkonten in allen Ressourcengruppen des Abonnements.
