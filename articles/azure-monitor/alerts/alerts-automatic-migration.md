---
title: Grundlegendes zur Funktionsweise des automatischen Migrationsprozesses für klassische Azure Monitor-Warnungen
description: Erfahren Sie, wie der automatische Migrationsprozess funktioniert.
ms.topic: conceptual
ms.date: 02/14/2021
ms.openlocfilehash: 92aaffcea7a7c96cd77aade318520b093eed3e14
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102045461"
---
# <a name="understand-the-automatic-migration-process-for-your-classic-alert-rules"></a>Grundlegendes zur Funktionsweise des automatischen Migrationsprozesses für klassische Warnungen

Wie [zuvor angekündigt](monitoring-classic-retirement.md) werden klassische Warnungen in Azure Monitor für Benutzer der öffentlichen Cloud eingestellt. Bis zum **31. Mai 2021** ist die Verwendung noch eingeschränkt möglich. Klassische Warnungen für die Azure Government-Cloud und Azure China 21Vianet werden am **29. Februar 2024** eingestellt.

Ein [Migrationstool](alerts-using-migration-tool.md) steht im Azure-Portal für Kunden bereit, die die Migration selbst auslösen möchten. In diesem Artikel wird der automatische Migrationsprozess in der öffentlichen Cloud beschrieben, der nach dem 31. Mai 2021 beginnt. Außerdem finden Sie Details zu Problemen, die auftreten können, sowie dazugehörige Lösungen.

## <a name="important-things-to-note"></a>Wichtige Hinweise

Im Migrationsprozess werden klassische Warnungsregeln in neue entsprechende Warnungsregeln umgewandelt und Aktionsgruppen erstellt. Bei der Vorbereitung sind folgende Punkte zu beachten:

- Die Formate der Benachrichtigungsnutzlast neuer Warnungsregeln unterscheiden sich von denen der klassischen Warnungsregeln, da mehr Features unterstützt werden. Wenn Sie über eine klassische Warnungsregel für Logik-Apps, Runbooks oder Webhooks verfügen, ist die Funktionsweise nach der Migration aufgrund von Unterschieden bei der Nutzlast erwartungsgemäß nicht mehr gegeben. [Erfahren Sie, wie Sie die Migration vorbereiten](alerts-prepare-migration.md).

- Einige klassische Warnungsregeln können mit dem Tool nicht migriert werden. [Erfahren Sie, welche Regeln nicht migriert werden können und wie Sie mit diesen Regeln verfahren](alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts).

## <a name="what-will-happen-during-the-automatic-migration-process-in-public-cloud"></a>Was geschieht während des automatischen Migrationsprozesses in der öffentlichen Cloud?

- Ab dem 31. Mai 2021 können Sie keine neuen klassischen Warnungsregeln mehr erstellen, und die Migration klassischer Warnungen wird in Batches ausgelöst.
- Alle klassischen Warnungsregeln, die gelöschte Zielressourcen oder [Metriken überwachen, die nicht mehr unterstützt werden](alerts-understand-migration.md#classic-alert-rules-on-deprecated-metrics), gelten als ungültig.
- Ungültige klassische Warnungsregeln werden zu einem Zeitpunkt nach dem 31. Mai 2021 entfernt.
- Sobald die Migration für Ihr Abonnement gestartet wurde, sollte sie innerhalb einer Stunde abgeschlossen sein. Kunden können den Status der Migration über das [Migrationstool in Azure Monitor](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel) überwachen.
- Abonnementbesitzer erhalten eine E-Mail, ob die Migration erfolgreich war oder ein Fehler aufgetreten ist.

    > [!NOTE]
    > Wenn Sie nicht bis zum Start des automatischen Migrationsprozesses warten möchten, können Sie die Migration weiterhin freiwillig mit dem Migrationstool einleiten.

## <a name="what-if-the-automatic-migration-fails"></a>Was geschieht, wenn die automatische Migration fehlschlägt?

Wenn der automatische Migrationsprozess fehlschlägt, erhalten Abonnementbesitzer eine E-Mail, in der Sie über das Problem informiert werden. Mithilfe des Migrationstools in Azure Monitor können Sie die vollständigen Details zum Problem anzeigen. Im [Leitfaden zur Problembehandlung](alerts-understand-migration.md#common-problems-and-remedies) finden Sie Informationen zu Problemen, die während der Migration auftreten können.

  > [!NOTE]
  > Falls eine Aktion seitens der Kunden erforderlich ist (z. B. das vorübergehende Deaktivieren einer Ressourcensperre oder das Ändern einer Richtlinienzuweisung), müssen Kunden solche Probleme beheben. Wenn die Probleme bis dahin nicht behoben sind, kann eine erfolgreiche Migration Ihrer klassischen Warnungen nicht garantiert werden.

## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der Migration](alerts-prepare-migration.md)
- [Funktionsweise des Migrationstools](alerts-understand-migration.md)