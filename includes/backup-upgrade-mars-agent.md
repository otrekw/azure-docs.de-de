---
title: Durchführen eines Upgrades für den Azure Backup-Agent
description: In diesem Artikel wird erläutert, warum Sie ein Upgrade für den Azure Backup-Agent durchführen sollten und wo das Upgrade heruntergeladen werden kann.
services: backup
cloud: ''
suite: ''
author: dcurwin
manager: carmonm
ms.service: backup
ms.tgt_pltfrm: <optional>
ms.devlang: <optional>
ms.topic: article
ms.date: 8/29/2018
ms.author: dacurwin
ms.openlocfilehash: 227ec87081817b273b66684e31a0ebd0e11134a0
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78197118"
---
## <a name="upgrade-the-mars-agent"></a>Durchführen eines Upgrades für den MARS-Agent

Versionen des MARS-Agents (Microsoft Azure Recovery Services) unter 2.0.9083.0 weisen eine Abhängigkeit vom Azure Access Control Service auf. Der MARS-Agent wird auch als Azure Backup-Agent bezeichnet.

2018 wird der [Azure Access Control Service in Azure von Microsoft als veraltet gekennzeichnet](../articles/active-directory/azuread-dev/active-directory-acs-migration.md). Seit dem 19. März 2018 treten in allen Versionen des MARS-Agents unter 2.0.9083.0 Sicherungsfehler auf. Um Sicherungsfehler zu vermeiden oder zu beheben, [führen Sie ein Upgrade für den MARS-Agent auf die neueste Version durch](https://go.microsoft.com/fwlink/?linkid=229525). Um Server zu ermitteln, bei denen ein Upgrade für den MARS-Agent erforderlich ist, führen Sie die Schritte im [Sicherungsblog zur Durchführung eines Upgrades für MARS-Agents](https://blogs.technet.microsoft.com/srinathv/2018/01/17/updating-azure-backup-agents/) durch.

Der MARS-Agent wird verwendet, um Dateien, Ordner und Systemstatusdaten in Azure sichern. System Center DPM und Azure Backup Server verwenden den MARS-Agent zum Sichern von Daten in Azure.
