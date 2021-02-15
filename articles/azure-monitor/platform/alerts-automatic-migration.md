---
title: Grundlegendes zur Funktionsweise des automatischen Migrationsprozesses für klassische Azure Monitor-Warnungen
description: Erfahren Sie, wie der automatische Migrationsprozess funktioniert.
ms.topic: conceptual
ms.date: 08/19/2019
ms.subservice: alerts
ms.openlocfilehash: 9ce3550dcef174a4d970917fabfda3c4fd2a8df4
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2021
ms.locfileid: "99526550"
---
# <a name="understand-the-automatic-migration-process-for-your-classic-alert-rules"></a>Grundlegendes zur Funktionsweise des automatischen Migrationsprozesses für klassische Warnungen

Wie [bereits angekündigt](monitoring-classic-retirement.md) werden klassische Warnungen in Azure Monitor für Benutzer der öffentlichen Cloud eingestellt, sind jedoch weiterhin für Ressourcen, die die neuen Warnungen noch nicht unterstützen, beschränkt im Einsatz. Als Teil des Ausmusterungsprozesses steht ein [Migrationstool](alerts-using-migration-tool.md) im Azure-Portal für Kunden bereit, die die Migration selbst auslösen möchten.
Dieser Artikel führt Sie durch den automatischen Migrationsprozess und hilft Ihnen, Probleme zu beheben, die möglicherweise auftreten.

  > [!NOTE]
  > Dieser Artikel gilt nur für die öffentliche Azure-Cloud. Der Ausmusterungsprozess für klassische Azure Monitor-Warnungen in Azure Government Cloud und Azure China 21ViaNet wird zu einem späteren Zeitpunkt angekündigt.

## <a name="what-will-happen-during-the-automatic-migration-process"></a>Was geschieht während des automatischen Migrationsprozesses?

- Ab dem **1. September 2019** können Kunden außer für [bestimmte Metriken](alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts) keine neuen klassischen Warnungsregeln mehr erstellen.
- Für die Ausnahmen können Kunden weiterhin neue klassische Warnungsregeln erstellen und ihre klassischen Warnungen bis zur nächsten Ankündigung verwenden.
- Ab dem **1. September2019** wird die Migration klassischer Warnungen in Batches für alle Kunden ausgelöst, die über klassische Warnungen verfügen.
- Wie beim Tool für die freiwillige Migration bleiben bestimmte klassische Warnungsregeln, die sich nicht migrieren lassen, unverändert. Diese klassischen Warnungsregeln werden bis zu den nächsten Ankündigungen weiterhin unterstützt. Allerdings werden alle ungültigen klassischen Warnungsregeln gelöscht, da sie nicht funktionsfähig sind.
Alle klassischen Warnungsregeln, die gelöschte Zielressourcen oder [Metriken überwachen, die nicht mehr unterstützt werden](alerts-understand-migration.md#classic-alert-rules-on-deprecated-metrics), gelten als ungültig.
- Sobald die Migration für Ihr Abonnement beginnt, sollte die Migration innerhalb einer Stunde abgeschlossen sein, sofern es keine Probleme gibt. Kunden können den Status der Migration auf dem [Blatt „Migration“ in Azure Monitor](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel) überwachen.
- Abonnementbesitzer erhalten nach erfolgreichem Abschluss der Migration eine E-Mail.
- Wenn es während der Migration zu Problemen kommt, erhalten Abonnementbesitzer auch eine E-Mail mit entsprechenden Informationen. Auf dem Blatt „Migration“ können Kunden alle Details des Problems einsehen.
- Falls eine Aktion seitens der Kunden erforderlich ist (z. B. das vorübergehende Deaktivieren einer Ressourcensperre oder das Ändern einer Richtlinienzuweisung), müssen Kunden solche Probleme beheben. Wenn die Probleme bis dahin nicht behoben sind, kann eine erfolgreiche Migration Ihrer klassischen Warnungen nicht garantiert werden.

    > [!NOTE]
    > Wenn Sie nicht bis zum Start des automatischen Migrationsprozesses warten möchten, können Sie die Migration weiterhin freiwillig mit dem Migrationstool einleiten.

## <a name="important-things-to-note"></a>Wichtige Hinweise

Im Migrationsprozess werden klassische Warnungsregeln in neue entsprechende Warnungsregeln umgewandelt und Aktionsgruppen erstellt. Bei der Vorbereitung sind folgende Punkte zu beachten:

- Die Formate der Benachrichtigungsnutzlast neuer Warnungsregeln unterscheiden sich von denen der klassischen Warnungsregeln, da mehr Funktionen unterstützt werden. Wenn Sie Logik-Apps, Runbooks oder Webhooks haben, die durch eine klassische Warnungsregel ausgelöst werden, funktionieren sie möglicherweise nicht mehr wie erwartet, sobald die Migration abgeschlossen ist, da die Nutzlasten der Benachrichtigungen unterschiedlich sind. [Erfahren Sie, wie Sie die Migration vorbereiten](alerts-prepare-migration.md).

- Einige klassische Warnungsregeln können mit dem Tool nicht migriert werden. [Erfahren Sie, welche Regeln nicht migriert werden können und wie Sie mit diesen Regeln verfahren](alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts).

    > [!NOTE]
    > Der Migrationsprozess hat keine Auswirkung auf die Auswertung Ihrer klassischen Warnungsregeln. Diese werden weiterhin ausgeführt und senden Warnungen, bis sie migriert werden und die neuen Warnungsregeln wirksam werden.

## <a name="what-if-the-automatic-migration-fails"></a>Was geschieht, wenn die automatische Migration fehlschlägt?

Wenn der automatische Migrationsprozess fehlschlägt, erhalten Abonnementbesitzer eine E-Mail, in der Sie über das Problem informiert werden. Auf dem Blatt „Migration“ in Azure Monitor können Sie alle Details des Problems einsehen.

Im [Leitfaden zur Problembehandlung](alerts-understand-migration.md#common-problems-and-remedies) finden Sie Informationen zu Problemen, die während der Migration auftreten können.

  > [!NOTE]
  > Falls eine Aktion seitens der Kunden erforderlich ist (z. B. das vorübergehende Deaktivieren einer Ressourcensperre oder das Ändern einer Richtlinienzuweisung), müssen Kunden solche Probleme beheben. Wenn die Probleme bis dahin nicht behoben sind, kann eine erfolgreiche Migration Ihrer klassischen Warnungen nicht garantiert werden.

## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der Migration](alerts-prepare-migration.md)
- [Funktionsweise des Migrationstools](alerts-understand-migration.md)
