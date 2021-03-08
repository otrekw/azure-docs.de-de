---
title: Migrieren von Azure Monitor-Warnungsregeln
description: Erfahren Sie, wie Sie mithilfe des freiwilligen Migrationstools Ihre klassischen Warnungsregeln migrieren können.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: fa487bec49ab9faa0f7c3dce752a30e4440fb873
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102037675"
---
# <a name="use-the-voluntary-migration-tool-to-migrate-your-classic-alert-rules"></a>Verwenden des freiwilligen Migrationstools zum Migrieren Ihrer klassischen Warnungsregeln

Wie [zuvor angekündigt](monitoring-classic-retirement.md) werden klassische Warnungen in Azure Monitor für Benutzer der öffentlichen Cloud eingestellt. Bis zum **31. Mai 2021** ist die Verwendung noch eingeschränkt möglich. Klassische Warnungen für die Azure Government-Cloud und Azure China 21Vianet werden am **29. Februar 2024** eingestellt.

Für Kunden, die klassische Warnungsregeln verwendet haben und die Migration selbst auslösen möchten, ist ein Migrationstool im Azure-Portal verfügbar. In diesem Artikel wird die Verwendung dieses Migrationstools erläutert.

## <a name="before-you-migrate"></a>Vor der Migration

Im Migrationsprozess werden klassische Warnungsregeln in neue entsprechende Warnungsregeln umgewandelt und Aktionsgruppen erstellt. Bei der Vorbereitung sind folgende Punkte zu beachten:

- Das Format der Benachrichtigungsnutzlast sowie die APIs zum Erstellen und Verwalten neuer Warnungsregeln unterscheiden sich von denen der klassischen Warnungsregeln, da mehr Features unterstützt werden. [Erfahren Sie, wie Sie die Migration vorbereiten](alerts-prepare-migration.md).

- Einige klassische Warnungsregeln können mit dem Tool nicht migriert werden. [Erfahren Sie, welche Regeln nicht migriert werden können und wie Sie mit diesen Regeln verfahren](alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts).

    > [!NOTE]
    > Der Migrationsprozess hat keine Auswirkung auf die Auswertung Ihrer klassischen Warnungsregeln. Diese werden weiterhin ausgeführt und senden Warnungen, bis sie migriert werden und die neuen Warnungsregeln wirksam werden.

## <a name="how-to-use-the-migration-tool"></a>Verwenden des Migrationstools

Gehen Sie folgendermaßen vor, um die Migration Ihrer klassischen Warnungsregeln im Azure-Portal auszulösen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Monitor** aus.

1. Wählen Sie **Warnungen** und dann **Warnungsregeln verwalten** oder **Klassische Warnungen anzeigen** aus.

1. Wählen Sie **Zu neuen Regeln migrieren** aus, um zur Zielseite der Migration zu wechseln. Auf dieser Seite werden alle Ihre Abonnements und der jeweilige Migrationsstatus angezeigt:

    ![Screenshot: Seite „Warnungsregeln migrieren“](media/alerts-using-migration-tool/migration-landing.png "Regeln migrieren")

    Alle Abonnements, die mit dem Tool migriert werden können, sind als **Bereit zur Migration** gekennzeichnet.

    > [!NOTE]
    > Der Rollout des Migrationstools erfolgt in mehreren Phasen für alle Abonnements, die klassische Warnungsregeln verwenden. In den frühen Phasen des Rollouts werden einige Abonnements möglicherweise als nicht bereit zur Migration angezeigt.

1. Wählen Sie ein oder mehrere Abonnements und dann **Migrationsvorschau anzeigen** aus.

    Auf der dann geöffneten Seite werden die Details der klassischen Warnungsregeln angezeigt, die jeweils für ein Abonnement migriert werden. Sie können auch **Migrationsdetails für dieses Abonnement herunterladen** auswählen, um die Details im CSV-Format herunterzuladen.

    ![Screenshot: Seite „Warnungsregeln migrieren“ mit dem Link „Migrationsdetails für dieses Abonnement herunterladen“ und einem Feld für die Angabe von E-Mail-Adressen für Migrationsbenachrichtigungen](media/alerts-using-migration-tool/migration-preview.png "Migrationsvorschau anzeigen")

1. Geben Sie eine oder mehrere E-Mail-Adressen an, an die Benachrichtigungen zum Migrationsstatus gesendet werden sollen. Sie erhalten eine E-Mail, wenn die Migration abgeschlossen oder eine Aktion von Ihnen erforderlich ist.

1. Wählen Sie **Migration starten** aus. Lesen Sie die Informationen im Bestätigungsdialogfeld, und bestätigen Sie, dass der Migrationsprozess gestartet werden kann.

    > [!IMPORTANT]
    > Nachdem Sie die Migration für ein Abonnement initiiert haben, können Sie für das Abonnement keine klassischen Warnungsregeln mehr bearbeiten oder erstellen. Durch diese Einschränkung wird sichergestellt, dass bei der Migration zu den neuen Regeln keine Änderungen an den klassischen Warnungsregeln verloren gehen. Obwohl Sie Ihre klassischen Warnungsregeln nicht ändern können, werden sie weiterhin ausgeführt und stellen Warnungen bereit, bis sie migriert wurden. Nachdem die Migration für Ihr Abonnement abgeschlossen ist, können Sie keine klassischen Warnungsregeln mehr verwenden.

    ![Screenshot: Bestätigungsaufforderung für Ihre Migration, einschließlich Links zu weiteren Informationen](media/alerts-using-migration-tool/migration-confirm.png "Migrationsstart bestätigen")

1. Wenn die Migration abgeschlossen oder eine Aktion von Ihnen erforderlich ist, erhalten Sie eine E-Mail an die zuvor angegebenen E-Mail-Adressen. Sie können den Status auch regelmäßig auf der Zielseite der Migration im Portal überprüfen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="why-is-my-subscription-listed-as-not-ready-for-migration"></a>Warum ist mein Abonnement als nicht bereit zur Migration aufgeführt?

Der Rollout des Migrationstools an Kunden erfolgt in mehreren Phasen. In den frühen Phasen sind die meisten oder alle Ihre Abonnements möglicherweise als **Nicht bereit zur Migration** gekennzeichnet. 

Wenn ein Abonnement zur Migration bereit ist, wird der Abonnementbesitzer per E-Mail über die Verfügbarkeit des Tools benachrichtigt. Achten Sie auf diese E-Mail-Nachricht.

### <a name="who-can-trigger-the-migration"></a>Wer kann die Migration auslösen?

Benutzer, denen die Rolle „Überwachungsmitwirkender“ auf Abonnementebene zugewiesen ist, können die Migration auslösen. [Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure für den Migrationsprozess](alerts-understand-migration.md#who-can-trigger-the-migration).

### <a name="how-long-will-the-migration-take"></a>Wie lange dauert die Migration?

Bei den meisten Abonnements ist die Migration nach weniger als einer Stunde abgeschlossen. Sie können den Migrationsfortschritt auf der Zielseite der Migration verfolgen. Sie können sicher sein, dass Ihre Warnungen während der Migration entweder im klassischen Warnungssystem oder im neuen System weiterhin ausgeführt werden.

### <a name="what-can-i-do-if-i-run-into-a-problem-during-migration"></a>Was kann ich tun, wenn während der Migration ein Problem auftritt?

Im [Leitfaden zur Problembehandlung](alerts-understand-migration.md#common-problems-and-remedies) finden Sie Informationen zu Problemen, die während der Migration auftreten können. Wenn zum Abschließen der Migration eine Aktion von Ihnen erforderlich ist, werden Sie über die E-Mail-Adressen, die Sie beim Einrichten des Tools angegeben haben, darüber benachrichtigt.

## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der Migration](alerts-prepare-migration.md)
- [Funktionsweise des Migrationstools](alerts-understand-migration.md)