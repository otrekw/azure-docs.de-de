---
title: Konfigurieren der operativen Sicherung für Azure-Blobs
description: Informationen zum Konfigurieren und Verwalten der operativen Sicherung für Azure-Blobs (Vorschau)
ms.topic: conceptual
ms.date: 02/16/2021
ms.openlocfilehash: 0dc490842389ba9286799aef5d37c1cf7c1ba64e
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102051071"
---
# <a name="configure-operational-backup-for-azure-blobs-in-preview"></a>Konfigurieren der operativen Sicherung für Azure-Blobs (Vorschau)

Mit Azure Backup können Sie die operative Sicherung zum Schutz von Blockblobs in Ihren Speicherkonten einfach konfigurieren. In diesem Artikel wird erläutert, wie Sie die operative Sicherung für ein oder mehrere Speicherkonten über das Azure-Portal konfigurieren. In dem Artikel werden die folgenden Themen behandelt:

- Wissenswertes vor dem Start
- Erstellen eines Sicherungstresors
- Erteilen von Berechtigungen für den Sicherungstresor auf den zu schützenden Speicherkonten
- Erstellen einer Sicherungsrichtlinie
- Konfigurieren der operativen Sicherung für ein oder mehrere Speicherkonten
- Auswirkungen auf die Sicherung von Speicherkonten

## <a name="before-you-start"></a>Vorbereitung

- Die operative Sicherung von Blobs ist eine lokale Sicherungslösung, die Daten für eine bestimmte Dauer im Quellspeicherkonto selbst verwaltet. Bei dieser Lösung wird keine zusätzliche Kopie der Daten im Tresor aufbewahrt.
- Mit dieser Lösung können Sie Ihre Daten bis zu 360 Tage lang für die Wiederherstellung aufbewahren. Lange Aufbewahrungszeiträume können jedoch zu einer längeren Dauer des Wiederherstellungsvorgangs führen.
- Die Lösung kann nur für Wiederherstellungen im Quellspeicherkonto verwendet werden und kann dazu führen, dass Daten überschrieben werden.
- Wenn Sie einen Container aus dem Speicherkonto löschen, indem Sie den Vorgang Container löschen aufrufen, kann dieser Container nicht mit einem Wiederherstellungsvorgang wiederhergestellt werden. Löschen Sie die einzelnen Blobs, anstatt einen ganzen Container zu löschen, wenn Sie sie möglicherweise später wiederherstellen möchten. Außerdem empfiehlt Microsoft, zusätzlich zur operativen Sicherung das vorläufige Löschen für Container zu aktivieren, um ein versehentliches Löschen von Containern zu verhindern.
- Unter [Unterstützungsmatrix](blob-backup-support-matrix.md) erfahren Sie mehr über unterstützte Szenarien, Einschränkungen und Verfügbarkeit.

## <a name="create-a-backup-vault"></a>Erstellen eines Sicherungstresors

Ein [Sicherungstresor](backup-vault-overview.md) ist eine Verwaltungsentität, in der die im Lauf der Zeit erstellten Wiederherstellungspunkte gespeichert werden. Er bietet eine Schnittstelle zum Durchführen von sicherungsbezogenen Vorgängen. Hierzu gehören das Erstellen von bedarfsgesteuerten Sicherungen, Durchführen von Wiederherstellungen und Erstellen von Sicherungsrichtlinien. Die operative Sicherung von Blobs ist zwar eine lokale Sicherung und „speichert“ keine Daten im Tresor, der Tresor wird jedoch für verschiedene Verwaltungsvorgänge benötigt.

>[!NOTE]
>Der Sicherungstresor ist eine neue Ressource, die für die Sicherung neuer unterstützter Workloads verwendet wird und sich von dem bereits vorhandenen Recovery Services-Tresor unterscheidet.

Anweisungen zum Erstellen eines Sicherungstresors finden Sie in der [Dokumentation zum Sicherungstresor](backup-vault-overview.md#create-a-backup-vault).

## <a name="grant-permissions-to-the-backup-vault-on-storage-accounts"></a>Erteilen von Berechtigungen für den Sicherungstresor auf Speicherkonten

Die operative Sicherung schützt auch das Speicherkonto (das die zu schützenden Blobs enthält) vor versehentlichem Löschen, indem eine sicherungseigene Löschsperre angewendet wird. Hierfür ist es erforderlich, dass der Sicherungstresor über bestimmte Berechtigungen auf den Speicherkonten verfügt, die geschützt werden müssen. Aus Gründen der Benutzerfreundlichkeit wurden diese Berechtigungen unter der Rolle „Mitwirkender für Speicherkontosicherung“ zusammengefasst. Befolgen Sie die nachstehenden Anweisungen für Speicherkonten, die geschützt werden müssen:

1. Navigieren Sie in dem zu schützenden Speicherkonto zur Registerkarte **Access Control (IAM)** im linken Navigationsbereich.
1. Wählen Sie **Rollenzuordnungen hinzufügen** aus, um die gewünschte Rolle zuzuweisen.

    ![Hinzufügen von Rollenzuweisungen](./media/blob-backup-configure-manage/add-role-assignments.png)

1. Im Bereich „Rollenzuweisung hinzufügen“:

    1. Wählen Sie unter **Rolle** die Option **Mitwirkender für Speicherkontosicherung** aus.
    1. Wählen Sie unter **Zugriff zuweisen zu** die Option **Benutzer, Gruppe oder Dienstprinzipal** aus.
    1. Geben Sie den **Namen des Sicherungstresors** ein, in dem Sie die Blobs in diesem Speicherkonto schützen möchten, und wählen Sie ihn in den Suchergebnissen aus.
    1. Wählen Sie anschließend **Save** (Speichern) aus.

        ![Rollenzuweisungsoptionen](./media/blob-backup-configure-manage/role-assignment-options.png)

        >[!NOTE]
        >Es kann bis zu 10 Minuten dauern, bis die Rollenzuweisung wirksam wird.

## <a name="create-a-backup-policy"></a>Erstellen einer Sicherungsrichtlinie

Eine Sicherungsrichtlinie regelt in der Regel die Aufbewahrung und den Zeitplan für Ihre Sicherungen. Da die operative Sicherung für Blobs kontinuierlicher Natur ist, benötigen Sie keinen Zeitplan für die Durchführung von Sicherungen. Die Richtlinie ist im Grunde genommen erforderlich, um den Aufbewahrungszeitraum festzulegen. Sie können die Sicherungsrichtlinie verwenden und wiederverwenden, um die Sicherung für mehrere Speicherkonten in einem Tresor zu konfigurieren.

Hier finden Sie die Schritte zum Erstellen einer Sicherungsrichtlinie für die operative Sicherung Ihrer Blobs:

1. Navigieren Sie in Ihrem Sicherungstresor zu **Sicherungsrichtlinien**, und wählen Sie **+ Hinzufügen** aus, um mit der Erstellung einer Sicherungsrichtlinie zu beginnen.

    ![Sicherungsrichtlinien](./media/blob-backup-configure-manage/backup-policies.png)

1. Geben Sie auf der Registerkarte **Grundlagen** einen Namen für die Sicherungsrichtlinie ein, und wählen Sie **Azure-Blobs** als Datenquellentyp aus. Sie können auch die Details für den ausgewählten Tresor anzeigen.

    ![Erstellen der Sicherungsrichtlinie](./media/blob-backup-configure-manage/create-backup-policy.png)

    >[!NOTE]
    >Obwohl die **Sicherungsspeicherredundanz** des Tresors angezeigt wird, gilt die Redundanz nicht wirklich für die operative Sicherung von Blobs, da die Sicherung lokaler Natur ist und keine Daten im Sicherungstresor gespeichert werden. Der Sicherungstresor hier ist die Verwaltungsentität, die Sie bei der Verwaltung des Schutzes von Blockblobs in ihren Speicherkonten unterstützt.

1. Geben Sie auf der Registerkarte **Sicherungsrichtlinie** die Details zur Aufbewahrung an. Sie sehen, dass bereits eine Aufbewahrungsregel mit der Bezeichnung **Standard** mit einem Aufbewahrungszeitraum von 30 Tagen vorhanden ist. Wenn Sie den Aufbewahrungszeitraum bearbeiten möchten, verwenden Sie das Symbol **Aufbewahrungsregel bearbeiten**, um die Dauer der Aufbewahrung der Daten anzugeben und zu bearbeiten. Sie können eine Aufbewahrungsdauer von bis zu 360 Tagen angeben.

    ![Registerkarte „Sicherungsrichtlinie“](./media/blob-backup-configure-manage/backup-policy-tab.png)

    >[!NOTE]
    >Eine Wiederherstellung über einen längeren Zeitraum kann dazu führen, dass die Wiederherstellungsvorgänge länger dauern. Zudem ist die erforderliche Zeit zum Wiederherstellen einer Gruppe von Daten von der Anzahl der Schreib- und Löschvorgänge abhängig, die während des Wiederherstellungszeitraums vorgenommen werden. Beispielsweise dauert bei einem Konto mit 1 Million Objekten, in dem pro Tag 3.000 Objekte hinzugefügt und 1.000 Objekte gelöscht werden, die Wiederherstellung zu einem 30 Tage in der Vergangenheit liegenden Zeitpunkt ungefähr zwei Stunden. Bei einem Konto mit einer solchen Änderungsrate sollte weder ein Aufbewahrungszeitraum von mehr als 90 Tagen noch eine Wiederherstellung zu einem so weit in der Vergangenheit liegenden Zeitpunkt angewendet werden.

1. Überprüfen Sie im Bereich **Überprüfen + Erstellen** alle Details für die Richtlinie, und wählen Sie anschließend **Erstellen** aus, um die Erstellung der Richtlinie abzuschließen. Eine Benachrichtigung zur Bestätigung wird angezeigt, sobald die Sicherungsrichtlinie erstellt wurde und zur Verwendung bereit ist.

    ![Richtlinie zum Überprüfen und Erstellen](./media/blob-backup-configure-manage/review-create.png)

## <a name="configure-backup"></a>Konfigurieren der Sicherung

Die Sicherung von Blobs wird auf der Ebene des Speicherkontos konfiguriert. So sind alle Blobs im Speicherkonto mit der operativen Sicherung geschützt.

So beginnen Sie mit dem Konfigurieren der Sicherung

1. Suchen Sie in der Suchleiste nach **Backup Center**.
1. Navigieren Sie zu **Übersicht** ->  **+ Sicherung**.

    ![Backup Center-Übersicht](./media/blob-backup-configure-manage/backup-center-overview.png)

1. Wählen Sie auf der Registerkarte **Initiieren: Sicherung konfigurieren** die Option **Azure-Blobs (Azure Storage)** als Datenquellentyp aus.

    ![Registerkarte „Initiieren: Sicherung konfigurieren“](./media/blob-backup-configure-manage/initiate-configure-backup.png)

1. Geben Sie auf der Registerkarte **Grundlagen** die Option **Azure-Blobs (Azure Storage)** als **Datenquellentyp** an, und wählen Sie den Sicherungstresor aus, dem Sie Ihre Speicherkonten zuordnen möchten. In dem Bereich können Sie Details zum ausgewählten Tresor anzeigen.

    ![Registerkarte „Grundlagen“](./media/blob-backup-configure-manage/basics-tab.png)

1. Wählen Sie als Nächstes die Sicherungsrichtlinie aus, die Sie zum Festlegen der Aufbewahrung verwenden möchten. Im unteren Teil des Bildschirms können Sie die Details der ausgewählten Richtlinie anzeigen. In der Spalte „Speicher für operative Daten“ wird die in der Richtlinie definierte Aufbewahrungsdauer angezeigt. „Operativ“ bedeutet, dass die Daten lokal im Quellspeicherkonto selbst verwaltet werden.

    ![Auswählen der Sicherungsrichtlinie](./media/blob-backup-configure-manage/choose-backup-policy.png)

    Sie können auch eine neue Sicherungsrichtlinie erstellen. Wählen Sie dazu **Neu erstellen** aus, und führen Sie die folgenden Schritte aus:

    1. Geben Sie einen Namen für die Richtlinie an, die Sie erstellen möchten. Stellen Sie sicher, dass in den anderen Feldern der richtige Datenquellentyp und Tresorname angezeigt werden.
    1. Wählen Sie auf der Registerkarte **Sicherungsrichtlinie** das Symbol „Aufbewahrungsregel bearbeiten“ aus, um die Dauer der Aufbewahrung der Daten zu bearbeiten und festzulegen. Sie können eine Aufbewahrungsdauer von bis zu 360 Tagen angeben. Eine Wiederherstellung über einen längeren Zeitraum kann dazu führen, dass die Wiederherstellungsvorgänge länger dauern.

        ![Erstellen einer neuen Sicherungsrichtlinie](./media/blob-backup-configure-manage/new-backup-policy.png)

    1. Wählen Sie anschließend **Überprüfen + Erstellen** aus, um die Sicherungsrichtlinie zu erstellen.

1. Als Nächstes müssen Sie die Speicherkonten auswählen, für die Sie den Schutz von Blobs konfigurieren möchten. Sie können mehrere Speicherkonten auf einmal auswählen und dann auf **Auswählen** klicken.

    Vergewissern Sie sich jedoch, dass der von Ihnen gewählte Tresor über die erforderlichen Berechtigungen verfügt, um die Sicherung auf den Speicherkonten zu konfigurieren, wie oben unter [Erteilen von Berechtigungen für den Sicherungstresor auf Speicherkonten](#grant-permissions-to-the-backup-vault-on-storage-accounts) beschrieben.

    ![Auswählen der zu sichernden Ressourcen](./media/blob-backup-configure-manage/select-resources.png)

    Azure Backup überprüft, ob der Tresor über ausreichende Berechtigungen verfügt, um die Konfiguration der Sicherung auf den ausgewählten Speicherkonten zu ermöglichen.

    ![Azure Backup überprüft Berechtigungen](./media/blob-backup-configure-manage/validate-permissions.png)

    Wenn die Validierung zu Fehlern führt (wie bei einem der Speicherkonten im Screenshot), navigieren Sie zu den ausgewählten Speicherkonten, und weisen Sie die entsprechenden Rollen zu, wie [hier](#grant-permissions-to-the-backup-vault-on-storage-accounts) beschrieben, und wählen Sie **Erneut überprüfen** aus. Die Zuweisung der neuen Rolle kann bis zu 10 Minuten dauern.

1. Wenn die Validierung für alle ausgewählten Speicherkonten erfolgreich war, fahren Sie mit **Überprüfen und Konfigurieren** fort, um die Sicherung zu konfigurieren. Es werden Benachrichtigungen angezeigt, die Sie über den Status der Schutzkonfiguration und deren Abschluss informieren.

## <a name="effects-on-backed-up-storage-accounts"></a>Auswirkungen auf gesicherte Speicherkonten

Sobald die Sicherung konfiguriert ist, werden Änderungen an den Blockblobs in den Speicherkonten nachverfolgt, und die Daten werden entsprechend der Sicherungsrichtlinie aufbewahrt. Beachten Sie die folgenden Änderungen in den Speicherkonten, für die die Sicherung konfiguriert ist:

- Die folgenden Funktionen sind für das Speicherkonto aktiviert. Diese können auf der Registerkarte **Datenschutz** des Speicherkontos angezeigt werden.
  - Point-in-Time-Wiederherstellung für Container: Mit der in der Sicherungsrichtlinie festgelegten Aufbewahrungsdauer
  - Vorläufiges Löschen für Blobs: Mit der in der Sicherungsrichtlinie festgelegten Aufbewahrungsdauer +5 Tage
  - Versionsverwaltung für Blobs
  - Blobänderungsfeed

  Wenn für das für die Sicherung konfigurierte Speicherkonto bereits die **Point-in-Time-Wiederherstellung für Container** oder das **Vorläufige Löschen für Blobs** aktiviert war (bevor die Sicherung konfiguriert wurde), stellt Azure Backup eine Aufbewahrung für mindestens die in der Sicherungsrichtlinie definierte Dauer sicher. Daher gilt für jede Eigenschaft Folgendes:

  - Die Aufbewahrungsdauer in der Sicherungsrichtlinie ist länger als die ursprünglich im Speicherkonto angegebene Aufbewahrungsdauer: Die Aufbewahrungsdauer auf dem Speicherkonto wird entsprechend der Sicherungsrichtlinie geändert
  - Die Aufbewahrung in der Sicherungsrichtlinie ist kürzer als die ursprünglich im Speicherkonto angegebene Aufbewahrungsdauer: Die Aufbewahrungsdauer auf dem Speicherkonto wird bei der ursprünglich festgelegten Dauer belassen.

  ![Registerkarte „Datenschutz“](./media/blob-backup-configure-manage/data-protection.png)

- Eine **Löschsperre** wird von Azure Backup auf das geschützte Speicherkonto angewendet. Die Sperre dient zum Schutz vor dem versehentlichen Löschen des Speicherkontos. Dies kann unter **Speicherkonto** > **Sperren** angezeigt werden.

    ![Löschen von Sperren](./media/blob-backup-configure-manage/delete-lock.png)

## <a name="manage-operational-backup"></a>Verwalten der operativen Sicherung

Sie können Backup Center als zentralisierte Benutzeroberfläche für die Verwaltung aller Sicherungen verwenden. Im Zusammenhang mit der operativen Sicherung für Azure-Blobs können Sie mit Backup Center folgende Aktionen ausführen:

- Wie oben beschrieben, können Sie es zum Erstellen von Sicherungstresor und -richtlinien verwenden. Sie können auch alle Tresore und Richtlinien unter den ausgewählten Abonnements anzeigen.
- Backup Center bietet Ihnen eine einfache Möglichkeit zum Überwachen des Schutzzustands geschützter Speicherkonten und von Speicherkonten, für die derzeit keine Sicherung konfiguriert ist.
- Sie können die Sicherung für beliebige Speicherkonten über die Schaltfläche **+ Sicherung** konfigurieren.
- Sie können Wiederherstellungen mithilfe der Schaltfläche **Wiederherstellen** initiieren und über **Sicherungsaufträge** nachverfolgen. Weitere Informationen zum Ausführen von Wiederherstellungen finden Sie unter [Wiederherstellungen von Azure-Blobs](blob-backup-support-matrix.md).
- Analysieren Sie die Sicherungsverwendung mithilfe von Sicherungsberichten.

    ![Backup Center](./media/blob-backup-configure-manage/backup-center.png)

Weitere Informationen finden Sie in der [Übersicht über Backup Center (Vorschau)](backup-center-overview.md).

## <a name="next-steps"></a>Nächste Schritte

- [Wiederherstellen von Azure-Blobs](blob-restore.md)
