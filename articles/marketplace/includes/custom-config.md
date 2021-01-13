---
title: include file
description: file
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: include
author: emuench
ms.author: mingshen
ms.date: 10/09/2020
ms.openlocfilehash: a1bc7cf1fd339ca3660c7b39326f37d2763c74b2
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92283205"
---
Falls eine zusätzliche Konfiguration erforderlich ist, können Sie eine geplante Aufgabe einrichten, die beim Start ausgeführt wird. So ist es möglich, nach der Bereitstellung der VM letzte Änderungen vorzunehmen. Berücksichtigen Sie außerdem folgende Punkte:

- Wenn es sich um eine Aufgabe mit einmaliger Ausführung handelt, sollte sie nach der erfolgreichen Ausführung automatisch gelöscht werden.
- Für Konfigurationen sollten keine anderen Laufwerke als C oder D verwendet werden, da nur für diese beiden Laufwerke garantiert ist, dass sie immer vorhanden sind (Laufwerk C ist der Betriebssystemdatenträger, und Laufwerk D ist der temporäre lokale Datenträger).

Weitere Informationen zu Linux-Anpassungen finden Sie unter [Erweiterungen und Features für virtuelle Computer für Linux](../../virtual-machines/extensions/features-linux.md).