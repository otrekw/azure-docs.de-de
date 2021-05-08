---
title: Übersicht über Sicherungstresore
description: Übersicht über Sicherungstresore
ms.topic: conceptual
ms.date: 04/19/2021
ms.openlocfilehash: e2d720da9474a35870de01559201d22c9e5b567f
ms.sourcegitcommit: 425420fe14cf5265d3e7ff31d596be62542837fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107739078"
---
# <a name="backup-vaults-overview"></a>Übersicht über Sicherungstresore

In diesem Artikel werden die Features eines Sicherungstresors beschrieben. Ein Sicherungstresor ist eine Speicherentität in Azure für Sicherungsdaten bestimmter neuerer, von Azure Backup unterstützter Workloads. Sie können Sicherungstresore für Sicherungsdaten von verschiedenen Azure-Diensten verwenden, z. B. von Azure Database for PostgreSQL-Servern und neueren Workloads, die Azure Backup unterstützen. Sicherungstresore vereinfachen die Organisation Ihrer Sicherungsdaten und minimieren gleichzeitig den Verwaltungsaufwand. Sicherungstresore basieren auf dem Azure Resource Manager-Modell von Azure, das Features wie die folgenden bietet:

- **Erweiterte Funktionen zum Schutz von Sicherungsdaten**: Durch Sicherungstresore bietet Azure Backup Sicherheitsfunktionen zum Schutz von Cloudsicherungen. Mit diesen Sicherheitsfunktionen wird sichergestellt, dass Sie Ihre Sicherungen schützen und Daten sicher wiederherstellen können, selbst wenn Produktions- und Sicherungsserver kompromittiert sind. [Weitere Informationen](backup-azure-security-feature.md)

- **Rollenbasierte Zugriffssteuerung von Azure (Azure RBAC)** : Azure RBAC ermöglicht eine differenzierte Verwaltung der Zugriffssteuerung in Azure. [Azure bietet verschiedene integrierte Rollen](../role-based-access-control/built-in-roles.md), während Azure Backup über drei [integrierte Rollen zum Verwalten von Wiederherstellungspunkten](backup-rbac-rs-vault.md) verfügt. Sicherungstresore sind mit der Azure RBAC kompatibel, die den Zugriff auf Sicherungen und Wiederherstellungen auf die definierten Benutzerrollen beschränkt. [Weitere Informationen](backup-rbac-rs-vault.md)

## <a name="storage-settings-in-the-backup-vault"></a>Speichereinstellungen im Sicherungstresor

Ein Sicherungstresor ist eine Entität, in der die im Lauf der Zeit erstellten Sicherungen und Wiederherstellungspunkte gespeichert werden. Der Sicherungstresor enthält außerdem die den geschützten VMs zugeordneten Sicherungsrichtlinien.

- Azure Backup übernimmt automatisch die Speicherung für den Tresor. Wählen Sie beim Erstellen des Sicherungstresors die Speicherredundanz entsprechend Ihren Geschäftsanforderungen aus.

- Weitere Informationen zur Speicherredundanz finden Sie in diesen Artikeln zur [Georedundanz](../storage/common/storage-redundancy.md#geo-redundant-storage) und zur [lokalen](../storage/common/storage-redundancy.md#locally-redundant-storage) Redundanz.

## <a name="encryption-settings-in-the-backup-vault"></a>Verschlüsselungseinstellungen im Sicherungstresor

In diesem Abschnitt werden die verfügbaren Optionen zur Verschlüsselung Ihrer im Sicherungstresor gespeicherten Sicherungsdaten behandelt. Der Azure Backup-Dienst verwendet die **Backup Management Service**-App, um auf Azure Key Vault, aber nicht auf die verwaltete Identität des Sicherungstresors zu zugreifen.


### <a name="encryption-of-backup-data-using-platform-managed-keys"></a>Verschlüsselung von Sicherungsdaten mit von der Plattform verwalteten Schlüsseln

Standardmäßig werden alle Ihre Daten mit von der Plattform verwalteten Schlüsseln verschlüsselt. Sie müssen Ihrerseits keine besonderen Maßnahmen ergreifen, um diese Verschlüsselung zu aktivieren. Dies gilt für alle Workloads, die in Ihrem Sicherungstresor gesichert werden.

## <a name="create-a-backup-vault"></a>Erstellen eines Sicherungstresors

Ein Sicherungstresor ist eine Verwaltungsentität, in der die im Lauf der Zeit erstellten Wiederherstellungspunkte gespeichert werden. Er bietet eine Schnittstelle zum Durchführen von sicherungsbezogenen Vorgängen. Hierzu gehören das Erstellen von bedarfsgesteuerten Sicherungen, Durchführen von Wiederherstellungen und Erstellen von Sicherungsrichtlinien.

Gehen Sie folgendermaßen vor, um einen Sicherungstresor zu erstellen.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter <https://portal.azure.com> beim Azure-Portal an.

### <a name="create-backup-vault"></a>Erstellen des Sicherungstresors

1. Geben Sie im Suchfeld **Sicherungstresore** ein.
1. Wählen Sie unter **Dienste** die Option **Sicherungstresore** aus.
1. Wählen Sie auf der Seite **Sicherungstresore** die Option **Hinzufügen** aus.
1. Stellen Sie auf der Registerkarte **Grundlagen** unter **Projektdetails** sicher, dass das richtige Abonnement ausgewählt ist, und wählen Sie dann **Neue Ressourcengruppe erstellen** aus. Geben Sie als Namen *myResourceGroup* ein.

  ![Erstellen einer neuen Ressourcengruppe](./media/backup-vault-overview/new-resource-group.png)

1. Geben Sie unter **Instanzendetails** die Zeichenfolge *myVault* als **Sicherungstresornamen** ein, und wählen Sie die gewünschte **Region** aus, in diesem Fall *USA, Osten*.
1. Wählen Sie nun die **Speicherredundanz** aus. Die Speicherredundanz kann nach dem Schützen von Elementen im Tresor nicht mehr geändert werden.
1. Wir empfehlen, dass Sie, wenn Sie Azure als primären Endpunkt für den Sicherungsspeicher verwenden, weiterhin die Standardeinstellung **Georedundant** verwenden.
1. Wenn Sie Azure nicht als primären Speicherendpunkt für die Sicherung verwenden, wählen Sie **Lokal redundant** aus. Dadurch verringern sich die Kosten für Azure-Speicher.
1. Erfahren Sie mehr über [Georedundanz](../storage/common/storage-redundancy.md#geo-redundant-storage) and [lokale Redundanz](../storage/common/storage-redundancy.md#locally-redundant-storage).

  ![Auswählen der Speicherredundanz](./media/backup-vault-overview/storage-redundancy.png)

1. Wählen Sie unten auf der Seite die Schaltfläche „Überprüfen + erstellen“ aus.

    ![Auswählen von „Überprüfen + erstellen“](./media/backup-vault-overview/review-and-create.png)

## <a name="delete-a-backup-vault"></a>Löschen eines Backup-Tresors

In diesem Abschnitt wird das Löschen eines Sicherungstresors beschrieben. Außerdem enthält er Anweisungen zum Entfernen von Abhängigkeiten und anschließenden Löschen eines Tresors.

### <a name="before-you-start"></a>Vorbereitung

Es ist nicht möglich, einen Sicherungstresor zu löschen, der eine der folgenden Abhängigkeiten aufweist:

- Sie können keinen Tresor löschen, der geschützte Datenquellen (z. B. Azure Database for PostgreSQL-Server) enthält.
- Es ist nicht möglich, einen Tresor zu löschen, der Sicherungsdaten enthält.

Wenn Sie versuchen, den Tresor zu löschen, ohne die Abhängigkeiten zu entfernen, werden die folgenden Fehlermeldungen angezeigt:

>Der Backup-Tresor kann nicht gelöscht werden, da im Tresor vorhandene Sicherungsinstanzen oder Sicherungsrichtlinien enthalten sind. Löschen Sie alle Sicherungsinstanzen und Sicherungsrichtlinien, die im Tresor vorhanden sind, und versuchen Sie dann, den Tresor zu löschen.

### <a name="proper-way-to-delete-a-vault"></a>So Löschen Sie einen Tresor ordnungsgemäß

>[!WARNING]
Der folgende Vorgang ist destruktiv und kann nicht rückgängig gemacht werden. Alle Sicherungsdaten und Sicherungselemente, die dem geschützten Server zugeordnet sind, werden dauerhaft gelöscht. Gehen Sie daher mit Bedacht vor.

Zum ordnungsgemäßen Löschen eines Tresors müssen Sie die Schritte in dieser Reihenfolge ausführen:

- Überprüfen Sie, ob geschützte Elemente vorhanden sind:
  - Wechseln Sie in der linken Navigationsleiste zu **Sicherungsinstanzen**. Alle hier aufgelisteten Elemente müssen zuerst gelöscht werden.

Nachdem Sie diese Schritte abgeschlossen haben, können Sie mit dem Löschen des Tresors fortfahren.

### <a name="delete-the-backup-vault"></a>Löschen des Sicherungstresors

Wenn sich keine Elemente mehr im Tresor befinden, klicken Sie im Tresordashboard auf **Löschen**. Sie werden in einem Bestätigungstext gefragt, ob Sie den Tresor löschen möchten.

![Löschen des Tresors](./media/backup-vault-overview/delete-vault.png)

1. Wählen Sie **Ja** aus, um das Löschen des Tresors zu bestätigen. Der Tresor wird gelöscht. Sie werden im Portal zum Dienstmenü **Neu** zurückgeführt.

## <a name="monitor-and-manage-the-backup-vault"></a>Überwachen und Verwalten des Sicherungstresors

In diesem Abschnitt wird erläutert, wie Sie das Dashboard **Übersicht** für Sicherungstresore zum Überwachen und Verwalten von Sicherungstresoren verwenden. Der Übersichtsbereich enthält zwei Kacheln: **Aufträge** und **Instanzen**.

![Übersichtsdashboard](./media/backup-vault-overview/overview-dashboard.png)

### <a name="manage-backup-instances"></a>Verwalten von Sicherungsinstanzen

Die Kachel **Aufträge** bietet eine zusammenfassende Ansicht aller Sicherungs- und Wiederherstellungsaufträge in Ihrem Sicherungstresor. Wenn Sie eine der Zahlen auf dieser Kachel auswählen, können Sie weitere Informationen zu Aufträgen für einen bestimmten Datenquellentyp, Vorgangstyp und Status anzeigen.

![Sicherungsinstanzen](./media/backup-vault-overview/backup-instances.png)

### <a name="manage-backup-jobs"></a>Verwalten von Sicherungsaufträgen

Die Kachel **Sicherungsinstanzen** bietet eine zusammenfassende Ansicht aller Sicherungsinstanzen im Sicherungstresor. Durch Auswählen einer der Zahlen auf dieser Kachel können Sie weitere Informationen zu Sicherungsinstanzen für einen bestimmten Datenquellentyp und Schutzstatus anzeigen.

![Sicherungsaufträge](./media/backup-vault-overview/backup-jobs.png)

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren der Sicherung für Azure-PostgreSQL-Datenbanken](backup-azure-database-postgresql.md#configure-backup-on-azure-postgresql-databases)
