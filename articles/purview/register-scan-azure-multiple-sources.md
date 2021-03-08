---
title: Überprüfen mehrerer Azure-Quellen
description: Erfahren Sie, wie Sie ein gesamtes Azure-Abonnement oder eine gesamte Ressourcengruppe im Azure Purview-Datenkatalog überprüfen.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 2/26/2021
ms.openlocfilehash: c57ac9ddbebcf02cb0118705b63f97fd1880b0f2
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101695890"
---
# <a name="register-and-scan-azure-multiple-sources"></a>Registrieren und Überprüfen mehrerer Azure-Quellen

In diesem Artikel wird beschrieben, wie Sie mehrere Azure-Quellen (Azure-Abonnements oder -Ressourcengruppen) in Purview registrieren und eine Überprüfung dafür einrichten.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Mehrere Azure-Quellen unterstützen Scans zum Erfassen von Metadaten und Schemas für die meisten von Purview unterstützten Azure-Ressourcentypen. Darüber hinaus werden die Daten dabei basierend auf System- und benutzerdefinierten Klassifizierungsregeln automatisch klassifiziert.

## <a name="prerequisites"></a>Voraussetzungen

- Erstellen Sie vor dem Registrieren der Datenquellen zunächst ein Azure Purview-Konto. Weitere Informationen zum Erstellen eines Purview-Kontos finden Sie unter [Schnellstart: Erstellen eines Azure Purview-Kontos im Azure-Portal](create-catalog-portal.md).
- Sie müssen ein Azure Purview-Datenquellenadministrator sein.
- Einrichten der Authentifizierung, wie in den folgenden Abschnitten beschrieben

### <a name="setting-up-authentication-for-enumerating-resources-under-a-subscription-or-resource-group"></a>Einrichten der Authentifizierung für das Auflisten von Ressourcen unter einem Abonnement oder einer Ressourcengruppe

1. Navigieren Sie im Azure-Portal zum Abonnement oder zur Ressourcengruppe.  
1. Wählen Sie im linken Navigationsmenü  **Zugriffssteuerung (IAM)**   aus. 
1. Sie müssen Besitzer oder Benutzerzugriffsadministrator sein, um dem Abonnement oder der Ressourcengruppe eine Rolle hinzuzufügen. Wählen Sie die Schaltfläche *+Hinzufügen* aus. 
1. Legen Sie die Rolle auf **Leser** fest, und geben Sie unter „Auswählen“ den Namen Ihres Azure Purview-Kontos (der dessen MSI darstellt) ein. Klicken Sie auf *Speichern*, um die Rollenzuweisung abzuschließen.

### <a name="setting-up-authentication-to-scan-resources-under-a-subscription-or-resource-group"></a>Einrichten der Authentifizierung für das Überprüfen von Ressourcen unter einem Abonnement oder einer Ressourcengruppe

Es gibt zwei Möglichkeiten, die Authentifizierung für mehrere Azure-Quellen einzurichten:

- Verwaltete Identität
- Dienstprinzipal

> [!NOTE]
> Sie müssen die Authentifizierung für jede Ressource innerhalb Ihres Abonnements oder ihrer Ressourcengruppe einrichten, die Sie registrieren und überprüfen möchten. Azure Storage-Ressourcentypen (Azure Blob Storage und Azure Data Lake Storage Gen2) erleichtern den Vorgang, indem sie das Hinzufügen von MSI oder Dienstprinzipal auf Abonnement- bzw. Ressourcengruppenebene als Speicherblob-Datenleser zulassen. Die Berechtigungen werden dann auf alle Speicherkonten innerhalb des Abonnements oder der Ressourcengruppe übertragen. Für alle anderen Ressourcentypen müssen Sie MSI oder Dienstprinzipal auf jede Ressource anwenden bzw. dies durch ein Skript ausführen lassen. Hier erfahren Sie, wie Sie Berechtigungen für jeden Ressourcentyp innerhalb eines Abonnements oder einer Ressourcengruppe hinzufügen.
    
- [Azure Blob Storage](register-scan-azure-blob-storage-source.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen1](register-scan-adls-gen1.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen2](register-scan-adls-gen2.md#setting-up-authentication-for-a-scan)
- [Azure SQL-Datenbank](register-scan-azure-sql-database.md)
- [Verwaltete Azure SQL-Datenbank-Instanz](register-scan-azure-sql-database-managed-instance.md#setting-up-authentication-for-a-scan)
- [Azure Synapse Analytics](register-scan-azure-synapse-analytics.md#setting-up-authentication-for-a-scan)
 
## <a name="register-an-azure-multiple-source"></a>Registrieren mehrerer Azure-Quellen

So registrieren Sie in Ihrem Datenkatalog mehrere neue Azure-Quellen:

1. Navigieren Sie zu Ihrem Purview-Konto.
1. Wählen Sie im linken Navigationsbereich die Option **Quellen** aus.
1. Wählen Sie **Registrieren** aus.
1. Wählen Sie unter **Quellen registrieren** die Option **Azure (mehrere)** aus.
1. Wählen Sie **Weiter** aus.

   :::image type="content" source="media/register-scan-azure-multiple-sources/register-azure-multiple.png" alt-text="Registrieren mehrerer Azure-Quellen":::

Gehen Sie auf dem Bildschirm **Quellen registrieren (Azure, mehrere)** wie folgt vor:

1. Geben Sie unter **Name** einen Namen ein, unter dem die Datenquelle im Katalog aufgeführt werden soll. 
1. Wählen Sie optional eine **Verwaltungsgruppe** aus, nach der gefiltert werden soll.
1. **Wählen Sie ein Abonnement oder eine bestimmte Ressourcengruppe** unter einem bestimmten Abonnement in der Dropdownliste aus. Der Registrierungsbereich wird auf das ausgewählte Abonnement oder die Ressourcengruppe festgelegt.  
1. Wählen Sie eine **Sammlung** aus, oder erstellen Sie eine neue Sammlung (optional).
1. Wählen Sie **Fertig stellen** aus, um die Datenquelle zu registrieren.

   :::image type="content" source="media/register-scan-azure-multiple-sources/azure-multiple-source-setup.png" alt-text="Einrichten mehrerer Azure-Quellen":::

## <a name="creating-and-running-a-scan"></a>Erstellen und Ausführen einer Überprüfung

Gehen Sie zum Erstellen und Ausführen einer neuen Überprüfung wie folgt vor:

1. Navigieren Sie zum Abschnitt **Quellen**.

1. Wählen Sie die von Ihnen registrierte Datenquelle aus.

1. Klicken Sie auf „Details anzeigen“, und wählen Sie **+ Neue Überprüfung** aus, oder verwenden Sie das Schnellaktionssymbol für die Überprüfung auf der Quellkachel.

1. Geben Sie den *Namen* ein, und wählen Sie alle Typen von Ressourcen aus, die Sie innerhalb dieser Quelle überprüfen möchten.

    1. Sie können *Alle* belassen (dies schließt zukünftige Ressourcentypen ein, die in diesem Abonnement oder dieser Ressourcengruppe möglicherweise noch nicht vorhanden sind).
    1. Sie können **speziell Ressourcentypen auswählen**, die Sie überprüfen möchten. Wenn Sie diese Option auswählen, werden zukünftige Ressourcentypen, die in diesem Abonnement oder in dieser Ressourcengruppe erstellt werden könnten, nicht für Überprüfungen eingeschlossen, es sei denn, die Überprüfung wird in Zukunft explizit bearbeitet.
    
    :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-source-scan.png" alt-text="Überprüfung mehrerer Azure-Quellen":::

1. Wählen Sie die Anmeldeinformationen zum Herstellen der Verbindung mit den Ressourcen in Ihrer Datenquelle aus. 
    1. Sie können **Anmeldeinformationen auf der übergeordneten Ebene** als MSI oder bestimmte Anmeldeinformationen vom Typ Dienstprinzipal auswählen, die Sie für alle Ressourcentypen unter dem Abonnement oder der Ressourcengruppe verwenden können.
    1. Sie können auch speziell **den Ressourcentyp auswählen und andere Anmeldeinformationen** für diesen Ressourcentyp anwenden.
    1. Alle Anmeldeinformationen werden als Authentifizierungsmethode für alle Ressourcen unter einem bestimmten Typ betrachtet.
    1. Sie müssen die ausgewählten Anmeldeinformationen für die Ressourcen festlegen, um Sie wie in diesem [Abschnitt](#Setting-up-authentication-to-scan-resources-under-a-subscription-or-resource-group) beschrieben erfolgreich zu überprüfen.
1. Sie können in jedem Typ auswählen, ob Sie entweder alle Ressourcen oder eine Teilmenge nach Namen überprüfen möchten.
    1. Wenn Sie die Option **Alle** belassen, werden zukünftige Ressourcen dieses Typs in zukünftigen Überprüfungen ebenfalls überprüft.
    1. Wenn Sie bestimmte Speicherkonten oder SQL-Datenbanken auswählen, werden zukünftige Ressourcen dieses Typs, die innerhalb dieses Abonnements oder dieser Ressourcengruppe erstellt werden, nicht für Überprüfungen eingeschlossen, es sei denn, die Überprüfung wird in Zukunft explizit bearbeitet.
 
1.  Klicken Sie auf **Weiter**, um fortzufahren. Wir testen den Zugriff, um zu überprüfen, ob Sie das Purview-MSI als Leser des Abonnements oder der Ressourcengruppe angewendet haben. Wenn eine Fehlermeldung ausgegeben wird, befolgen Sie [diese](#Setting-up-authentication-for-enumerating-resources-under-a-subscription-or-resource-group) Anweisungen.

1.  Wählen Sie **Überprüfungsregelsätze** für jeden Ressourcentyp aus, den Sie im vorherigen Schritt ausgewählt haben. Sie können Überprüfungsregelsätze auch inline erstellen.
  :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-scan-rule-set.png" alt-text="Auswahl mehrerer Azure-Überprüfungsregelsätze":::

1. Wählen Sie den Auslöser für die Überprüfung. Sie können eine **wöchentliche/monatliche** oder **einmalige** Ausführung planen.

1. Überprüfen Sie die Auswahl der Überprüfung, und schließen Sie die Einrichtung mit „Speichern“ ab.   

## <a name="viewing-your-scans-and-scan-runs"></a>Anzeigen Ihrer Überprüfungen und Überprüfungsausführungen

1. Zeigen Sie Quelldetails an, indem Sie auf der Kachel im Quellenabschnitt auf **Details anzeigen** klicken. 

      :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-source-detail.png" alt-text="Details zu mehreren Azure-Quellen"::: 

1. Zeigen Sie die Details der Überprüfungsausführung an, indem Sie zur Seite **Überprüfungsdetails** navigieren.
    1. Die *Statusleiste* ist eine kurze Zusammenfassung zum Ausführungsstatus der untergeordneten Ressourcen. Sie wird auf Abonnement- oder Ressourcengruppenebene angezeigt.
    1. Grün bedeutet erfolgreich, Rot weist auf einen Fehler hin. Grau bedeutet, dass die Überprüfung noch ausgeführt wird.
    1. Sie können auf die einzelnen Überprüfungen klicken, um präzisere Details anzuzeigen.

      :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-scan-full-details.png" alt-text="Überprüfungsdetails zu mehreren Azure-Quellen":::

1. Zeigen Sie im unteren Bereich der Quelldetailsseite eine Zusammenfassung der letzten nicht erfolgreichen Überprüfungsausführungen an. Sie können auch auf präzisere Details zu diesen Ausführungen klicken.

## <a name="manage-your-scans---edit-delete-or-cancel"></a>Verwalten Ihrer Überprüfungen: Bearbeiten, Löschen oder Abbrechen
Gehen Sie zum Verwalten oder Löschen einer Überprüfung wie folgt vor:

- Navigieren Sie zum Verwaltungscenter. Wählen Sie im Abschnitt „Quellen und Überprüfung“ die Option „Datenquellen“ und anschließend die gewünschte Datenquelle aus.

- Wählen Sie die Überprüfung aus, die Sie verwalten möchten. Wenn Sie die Überprüfung bearbeiten möchten, wählen Sie „Bearbeiten“ aus.

- Wenn Sie Ihre Überprüfung löschen möchten, wählen Sie „Löschen“ aus.
- Wenn eine Überprüfung ausgeführt wird, können Sie sie auch abbrechen.

## <a name="next-steps"></a>Nächste Schritte

- [Browsen im Azure Purview-Datenkatalog](how-to-browse-catalog.md)
- [Suchen im Azure Purview-Datenkatalog](how-to-search-catalog.md)    
