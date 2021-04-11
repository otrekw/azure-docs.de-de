---
title: Durchführen eines Upgrades für den Azure Backup-Agent
description: In diesem Artikel wird erläutert, warum Sie ein Upgrade für den Azure Backup-Agent durchführen sollten und wo das Upgrade heruntergeladen werden kann.
services: backup
cloud: ''
suite: ''
author: v-amallick
manager: carmonm
ms.service: backup
ms.tgt_pltfrm: <optional>
ms.devlang: <optional>
ms.topic: article
ms.date: 03/03/2020
ms.author: v-amallick
ms.openlocfilehash: bf77103db93652e1df837f6b1032b5e53bd41e1f
ms.sourcegitcommit: af6eba1485e6fd99eed39e507896472fa930df4d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2021
ms.locfileid: "106294139"
---
## <a name="upgrade-the-mars-agent"></a>Durchführen eines Upgrades für den MARS-Agent

Versionen des MARS-Agents (Microsoft Azure Recovery Services) unter 2.0.9083.0 weisen eine Abhängigkeit vom Azure Access Control Service auf. Der MARS-Agent wird auch als Azure Backup-Agent bezeichnet.

2018 wird der [Azure Access Control Service in Azure von Microsoft als veraltet gekennzeichnet](../articles/active-directory/azuread-dev/active-directory-acs-migration.md). Seit dem 19. März 2018 treten in allen Versionen des MARS-Agents unter 2.0.9083.0 Sicherungsfehler auf. Um Sicherungsfehler zu vermeiden oder zu beheben, [führen Sie ein Upgrade für den MARS-Agent auf die neueste Version durch](https://support.microsoft.com/help/4538314/update-for-azure-backup-for-microsoft-azure-recovery-services-agent). Um Server zu ermitteln, bei denen ein Upgrade für den MARS-Agent erforderlich ist, befolgen Sie die Schritte unter [Durchführung eines Upgrades des MARS-Agents (Microsoft Azure Recovery Services)](../articles/backup/upgrade-mars-agent.md).

Der MARS-Agent wird verwendet, um Dateien, Ordner und Systemstatusdaten in Azure sichern. System Center DPM und Azure Backup Server verwenden den MARS-Agent zum Sichern von Daten in Azure.
