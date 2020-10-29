---
services: storage
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/29/2020
ms.author: normesta
ms.custom: include file
ms.openlocfilehash: 8b793a28c31df185555b9a548df52e4beeccf305
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92131631"
---
| Mechanismus | `Scope` |Einschränkungen | Unterstützte Berechtigungsebene |
|---|---|---|---|
| RBAC | Speicherkonten, Container. <br>Ressourcenübergreifende RBAC-Rollenzuweisungen auf Abonnement- oder Ressourcengruppenebene | 2\.000 RBAC-Rollenzuweisungen in einem Abonnement | RBAC-Rollen (integriert oder benutzerdefiniert) |
| ACL| Verzeichnis, Datei |32 ACL-Einträge (im Grunde 28 ACL-Einträge) pro Datei und pro Verzeichnis. Für Zugriffs- und Standard-ACLs gilt jeweils ein eigener Zugriffsgrenzwert von 32 ACLs. |ACL-Berechtigung|
