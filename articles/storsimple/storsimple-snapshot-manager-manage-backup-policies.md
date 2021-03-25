---
title: StorSimple Snapshot Manager-Sicherungsrichtlinien | Microsoft Docs
description: Beschreibt, wie das StorSimple Snapshot Manager-MMC-Snap-in zum Erstellen und Verwalten von Sicherungsrichtlinien verwendet wird, die geplante Sicherungen steuern.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 04415d0b-42f0-4737-8afa-257fb2dbe5d0
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 745aeb9844f1abad075d34b34735563200ea618b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90054941"
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>Verwenden des StorSimple Snapshot Managers zum Erstellen und Verwalten von Sicherungsrichtlinien
## <a name="overview"></a>Übersicht
Eine Sicherungsrichtlinie erstellt einen Zeitplan für die Sicherung von Volumedaten lokal oder in der Cloud. Wenn Sie eine Sicherungsrichtlinie erstellen, können Sie auch eine Aufbewahrungsrichtlinie angeben. (Sie können bis zu 64 Momentaufnahmen beibehalten.) Weitere Informationen über Sicherungsrichtlinien finden Sie im Artikel [StorSimple 8000-Serie: eine Hybridcloud-Speicherlösung](storsimple-overview.md) unter [Sicherungstypen](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies).

In diesem Tutorial werden folgende Punkte erläutert:

* Erstellen einer Sicherungsrichtlinie
* Bearbeiten einer Sicherungsrichtlinie
* Löschen einer Sicherungsrichtlinie

## <a name="create-a-backup-policy"></a>Erstellen einer Sicherungsrichtlinie
Verwenden Sie das folgende Verfahren, um eine neue Sicherungsrichtlinie zu erstellen.

#### <a name="to-create-a-backup-policy"></a>So erstellen Sie eine Sicherungsrichtlinie
1. Klicken Sie auf das Desktopsymbol, um den StorSimple Snapshot Manager zu starten.
2. Klicken Sie im Fenster **Bereich** mit der rechten Maustaste auf **Sicherungsrichtlinien**, und klicken Sie dann auf **Sicherungsrichtlinie erstellen**.

    ![Erstellen einer Sicherungsrichtlinie](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    Das Dialogfeld **Richtlinie erstellen** wird angezeigt.

    ![Erstellen einer Richtlinie – Registerkarte "Allgemein"](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. Geben Sie auf der Registerkarte **Allgemein** die folgenden Informationen an:

   1. Geben Sie im Textfeld **Name** einen Namen für die Richtlinie ein.
   2. Geben Sie im Textfeld **Volumegruppe** den Namen der Volumegruppe ein, die der Richtlinie zugeordnet ist.
   3. Wählen Sie entweder **Lokale Momentaufnahme** oder **Cloudmomentaufnahme**.
   4. Wählen Sie die Anzahl der Momentaufnahmen, die beibehalten werden sollen. Bei Auswahl von **Alle** werden 64 Momentaufnahmen (die maximale Anzahl) beibehalten.
4. Klicken Sie auf die Registerkarte **Zeitplan**.

    ![Erstellen einer Richtlinie – Registerkarte "Zeitplan"](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. Geben Sie auf der Registerkarte **Zeitplan** die folgenden Informationen an:

   1. Klicken Sie auf das Kontrollkästchen **Aktivieren**, um die nächste Sicherung zu planen.
   2. Wählen Sie unter **Einstellungen** die Option **Einmal**, **Täglich**, **Wöchentlich** oder **Monatlich** aus.
   3. Klicken Sie im Textfeld **Start** auf das Kalendersymbol, und wählen Sie ein Startdatum aus.
   4. Unter **Erweiterte Einstellungen** können Sie optional Wiederholungszeitpläne und ein Enddatum festlegen.
   5. Klicken Sie auf **OK**.

Nachdem Sie eine Sicherungsrichtlinie erstellt haben, werden die folgenden Informationen im Bereich **Ergebnisse** angezeigt:

* **Name**: Der Name der Sicherungsrichtlinie.
* **Typ**: Lokale Momentaufnahme oder Cloudmomentaufnahme.
* **Volumegruppe**: Die der Richtlinie zugeordnete Volumegruppe.
* **Beibehaltung**: Die Anzahl der beibehaltenen Momentaufnahmen, maximal 64.
* **Erstellt**: Das Erstellungsdatum der Richtlinie.
* **Aktiviert**: Gibt an, ob die Richtlinie derzeit angewendet wird. Bei **True** ist die Richtline aktiviert, bei **False** ist die Richtlinie nicht aktiviert.

## <a name="edit-a-backup-policy"></a>Bearbeiten einer Sicherungsrichtlinie
Verwenden Sie das folgende Verfahren, um eine vorhandene Sicherungsrichtlinie bearbeiten.

#### <a name="to-edit-a-backup-policy"></a>So bearbeiten Sie eine Sicherungsrichtlinie
1. Klicken Sie auf das Desktopsymbol, um den StorSimple Snapshot Manager zu starten.
2. Klicken Sie im Fenster **Bereich** auf den Knoten **Sicherungsrichtlinien**. Alle Sicherungsrichtlinien werden im Bereich **Ergebnisse** angezeigt.
3. Klicken Sie mit der rechten Maustaste auf die zu bearbeitende Richtlinie, und klicken Sie dann auf **Bearbeiten**.

    ![Bearbeiten einer Sicherungsrichtlinie](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. Wenn das Fenster **Richtlinie erstellen** angezeigt wird, geben Sie die Änderungen ein, und klicken Sie auf **OK**.

## <a name="delete-a-backup-policy"></a>Löschen einer Sicherungsrichtlinie
Verwenden Sie das folgende Verfahren, um eine Sicherungsrichtlinie zu löschen.

#### <a name="to-delete-a-backup-policy"></a>So löschen Sie eine Sicherungsrichtlinie
1. Klicken Sie auf das Desktopsymbol, um den StorSimple Snapshot Manager zu starten.
2. Klicken Sie im Fenster **Bereich** auf den Knoten **Sicherungsrichtlinien**. Alle Sicherungsrichtlinien werden im Bereich **Ergebnisse** angezeigt.
3. Klicken Sie mit der rechten Maustaste auf die Sicherungsrichtlinie, die gelöscht werden soll, und klicken Sie dann auf **Löschen**.
4. Klicken Sie bei Anzeige der Bestätigungsmeldung auf **Ja**.

    ![Bestätigung der gelöschten Sicherungsrichtlinie](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [Verwenden von StorSimple Snapshot Manager zum Verwalten der StorSimple-Lösung](storsimple-snapshot-manager-admin.md).
* Weitere Informationen zum [Verwenden von StorSimple Snapshot Manager zum Anzeigen und Verwalten von Sicherungsaufträgen](storsimple-snapshot-manager-manage-backup-jobs.md).
