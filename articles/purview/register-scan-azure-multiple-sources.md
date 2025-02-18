---
title: Überprüfen Sie mehrerer Quellen in Azure Purview
description: Erfahren Sie, wie Sie ein gesamtes Azure-Abonnement oder eine gesamte Ressourcengruppe im Azure Purview-Datenkatalog überprüfen.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 05/08/2021
ms.openlocfilehash: ce78b36a07a0648e869a2a382554fdc41b408f96
ms.sourcegitcommit: 3de22db010c5efa9e11cffd44a3715723c36696a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2021
ms.locfileid: "109656324"
---
# <a name="register-and-scan-multiple-sources-in-azure-purview"></a>Registrieren und überprüfen Sie mehrere Quellen in Azure Purview

Dieser Artikel beschreibt, wie Sie mehrere Quellen (Azure-Abonnements oder Ressourcengruppen) in Azure Purview registrieren und überprüfen.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können mehrere Quellen überprüfen, um Metadaten und Schema für die meisten Azure-Ressourcentypen zu erfassen, die Azure Purview unterstützt. Azure Purview klassifiziert die Daten automatisch anhand von System- und benutzerdefinierten Klassifizierungsregeln.

## <a name="prerequisites"></a>Voraussetzungen

- Erstellen Sie ein Azure Purview-Konto bevor Sie Datenquellen registrieren. Weitere Informationen finden Sie unter [Quickstart: Erstellen eines Azure Purview-Kontos](create-catalog-portal.md).
- Stellen Sie sicher, dass Sie ein Azure Purview Datenquellen-Administrator sind. Sie müssen auch Eigentümer oder Benutzerzugriffsadministrator sein, um eine Rolle für ein Abonnement oder eine Ressourcengruppe hinzuzufügen.
- Richten Sie die Authentifizierung wie in den folgenden Abschnitten beschrieben ein.

### <a name="set-up-authentication-for-enumerating-resources-under-a-subscription-or-resource-group"></a>Richten Sie die Authentifizierung für die Aufzählung von Ressourcen unter einem Abonnement oder einer Ressourcengruppe ein

1. Gehen Sie zu dem Abonnement oder der Ressourcengruppe im Azure-Portal.  
1. Wählen Sie im linken Menü  **Zugriffskontrolle (IAM)**  aus. 
1. Wählen Sie **+ Hinzufügen** aus. 
1. Wählen Sie im **Eingabefeld Auswählen** die Rolle **Reader** aus und geben Sie den Namen Ihres Azure Purview-Kontos ein (der den Namen der MSI-Datei darstellt). 
1. Klicken Sie auf **Speichern**, um die Rollenzuweisung abzuschließen.

### <a name="set-up-authentication-to-scan-resources-under-a-subscription-or-resource-group"></a>Richten Sie die Authentifizierung für die Überprüng von Ressourcen unter einem Abonnement oder einer Ressourcengruppe ein

Es sind zwei Möglichkeiten verfügbar, die Authentifizierung für mehrere Quellen in Azure einzurichten:

- Verwaltete Identität
- Dienstprinzipal

Sie müssen die Authentifizierung für jede Ressource innerhalb Ihres Abonnements oder Ihrer Ressourcengruppe einrichten, die Sie registrieren und überprüfen möchten. Azure-Storage-Ressourcentypen (Azure Blob Storage und Azure Data Lake Storage Gen2) erleichtern es, indem sie es Ihnen ermöglichen, die MSI-Datei oder den Dienstprinzipal auf der Abonnement- oder Ressourcengruppenebene als Speicher-Blob-Datenleser hinzuzufügen. Die Berechtigungen werden dann auf jedes Speicherkonto innerhalb dieses Abonnements oder dieser Ressourcengruppe übertragen. Für alle anderen Ressourcentypen müssen Sie die MSI-Datei oder das Dienstprinzipal auf jede Ressource anwenden oder ein Skript zu diesem Zweck erstellen. 

Wie Sie Berechtigungen für jeden Ressourcentyp innerhalb eines Abonnements oder einer Ressourcengruppe hinzufügen, erfahren Sie in den folgenden Ressourcen:
    
- [Azure Blob Storage](register-scan-azure-blob-storage-source.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen1](register-scan-adls-gen1.md#setting-up-authentication-for-a-scan)
- [Azure Data Lake Storage Gen2](register-scan-adls-gen2.md#setting-up-authentication-for-a-scan)
- [Azure SQL-Datenbank](register-scan-azure-sql-database.md)
- [Verwaltete Azure SQL-Datenbank-Instanz](register-scan-azure-sql-database-managed-instance.md#setting-up-authentication-for-a-scan)
- [Azure Synapse Analytics](register-scan-azure-synapse-analytics.md#setting-up-authentication-for-a-scan)
 
## <a name="register-multiple-sources"></a>Registrieren Sie mehrere Quellen

Um neue Mehrfachquellen in Ihrem Datenkatalog zu registrieren, gehen Sie wie folgt vor:

1. Gehen Sie zu Ihrem Azure Purview-Konto.
1. Wählen Sie **Quellen** im linken Menü aus.
1. Wählen Sie **Registrieren**.
1. Wählen Sie unter **Quellen registrieren** die Option **Azure (mehrere)** aus.

   :::image type="content" source="media/register-scan-azure-multiple-sources/register-azure-multiple.png" alt-text="Screenshot, der die Fläche für Azure Multiple auf der Anzeige für die Registrierung von mehreren Quellen anzeigt.":::
1. Wählen Sie **Weiter**.
1. Auf der Anzeige **Quellen registrieren (Azure)** gehen Sie wie folgt vor:

   1. Geben Sie im **Namensfeld** einen Namen ein, unter dem die Datenquelle im Katalog aufgelistet werden soll. 
   1. Wählen Sie im **Managementgruppen-Feld** fakultativ eine Managementgruppe aus, nach der gefiltert werden soll.
   1. Wählen Sie in den Dropdownlistenfeldern **Abonnement** und **Ressourcengruppe** ein Abonnement, bzw. eine bestimmte Ressourcengruppe aus. Der Registrierungsbereich wird auf das ausgewählte Abonnement oder die Ressourcengruppe festgelegt.  

      :::image type="content" source="media/register-scan-azure-multiple-sources/azure-multiple-source-setup.png" alt-text="Screenshot, der die Felder zur Auswahl eines Abonnements und einer Ressourcengruppe anzeigt.":::
   1. Wählen **Sie** im Feld Sammlung auswählen eine Sammlung aus, oder erstellen Sie eine neue Sammlung (optional).
   1. Wählen Sie **Registrieren** aus, um die Datenquellen zu registrieren.


## <a name="create-and-run-a-scan"></a>Erstellen und führen Sie eine Überprüfung aus

Gehen Sie zum Erstellen und Ausführen einer neuen Überprüfung wie folgt vor:

1. Navigieren Sie zum **Quellen-** Abschnitt.
1. Wählen Sie die von Ihnen registrierte Datenquelle aus.
1. Wählen Sie **Details anzeigen** >  **+** Neue Überprüfung, oder verwenden Sie das Schnellaktionssymbol **Überprüfen** auf der Quellfläche.
1. Geben Sie unter **Name** den Namen ein.
1. Wählen Sie unter **Typ** die Ressourcentypen aus, die Sie in dieser Quelle überprüfen möchten. Wählen Sie eine der Optionen aus:

    - Belassen Sie es als **Alle**. Diese Auswahl umfasst zukünftige Ressourcentypen, die möglicherweise derzeit nicht in diesem Abonnement oder dieser Ressourcengruppe vorhanden sind.
    - Verwenden Sie die Felder, um gezielt Ressourcentypen auszuwählen, die Sie überprüfen möchten. Wenn Sie diese Option wählen, werden zukünftige Ressourcentypen, die innerhalb dieses Abonnements oder dieser Ressourcengruppe erstellt werden könnten, nicht für Überprüfungen berücksichtigt, es sei denn, die Überprüfung wird zukünftig explizit bearbeitet.
    
    :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-source-scan.png" alt-text="Screenshot, der die Optionen zum Überprüfen mehrerer Quellen anzeigt.":::

1. Wählen Sie die Zugangsdaten für die Verbindung zu den Ressourcen innerhalb Ihrer Datenquelle: 
    - Sie können Anmeldeinformationen auf der übergeordneten Ebene als MSI-Datei auswählen, oder Sie können Anmeldeinformationen für einen bestimmten Dienstprinzipaltyp auswählen. Sie können diese Anmeldeinformationen dann für alle Ressourcentypen unter dem Abonnement oder der Ressourcengruppe verwenden.
    - Sie können den Ressourcentyp gezielt auswählen und eine andere Anmeldeinformation für diesen Ressourcentyp anwenden.
    
    Jede Anmeldeinformation wird als Authentifizierungsmethode für alle Ressourcen unter einem bestimmten Typ betrachtet. Sie müssen die ausgewählten Anmeldeinformationen auf den Ressourcen einstellen, um sie erfolgreich zu überprüfen, wie [weiter oben in diesem Artikel](#set-up-authentication-to-scan-resources-under-a-subscription-or-resource-group) beschrieben.
1. Innerhalb jedes Typs können Sie wählen, ob Sie entweder alle Ressourcen oder eine Teilmenge davon nach Namen überprüfen möchten:
    - Wenn Sie die Option auf **Alle** belassen, werden auch zukünftige Ressourcen dieses Typs bei zukünftigen Überprüfungsläufen überprüft.
    - Wenn Sie bestimmte Speicherkonten oder SQL-Datenbanken auswählen, werden zukünftige Ressourcen dieses Typs, die innerhalb dieses Abonnements oder dieser Ressourcengruppe erstellt wurden, nicht für Überprüfungen berücksichtigt, es sei denn, die Überprüfung wird in Zukunft explizit bearbeitet.
 
1. die Option **Fortsetzen** aus, um fortzufahren. Azure Purview testet den Zugriff, um zu prüfen, ob Sie die Azure Purview MSI-Datei als Leser auf das Abonnement oder die Ressourcengruppe angewendet haben. Wenn Sie eine Fehlermeldung erhalten, befolgen Sie diese [Anweisungen](#set-up-authentication-for-enumerating-resources-under-a-subscription-or-resource-group), um es zu beheben.

1. Wählen Sie Überprüfungsregelsätze für jeden Ressourcentyp aus, den Sie im vorherigen Schritt ausgewählt haben. Sie können Überprüfungsregelsätze auch inline erstellen.
  
   :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-scan-rule-set.png" alt-text="Screenshot, der die Überprüfungsregeln für jeden Ressourcentyp anzeigt.":::

1. Wählen Sie den Auslöser für die Überprüfung. Sie können es so planen, dass es wöchentlich, monatlich oder einmal ausgeführt wird.

1. Begutachten Sie Ihre Überprüfung, und schließen Sie die Einrichtung mit **Speichern** ab. 

## <a name="view-your-scans-and-scan-runs"></a>Betrachten Sie Ihre Überprüfungen und Überprüfungsausführungen

1. Zeigen Sie die Quellendetails an, indem Sie auf der Fläche unter dem Abschnitt **Quellen** die Option **Details** anzeigen auswählen. 

    :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-source-detail.png" alt-text="Screenshot, der die Quellendetails anzeigt."::: 

1. Zeigen Sie die Details der Überprüfung-Ausführung an, indem Sie auf die Seite mit den **Überprüfungsdetails** gehen.
   
    Die *Statusleiste* ist eine kurze Zusammenfassung des Ausführungsstatus der untergeordneten Ressourcen. Sie wird auf Abonnement- oder Ressourcengruppenebene angezeigt. Die Farben haben die folgenden Bedeutungen:
    
    - Grün: Die Überprüfung war erfolgreich.
    - Rot: Die Überprüfung ist fehlgeschlagen. 
    - Grau: Der Überprüfungsvorgang wird noch ausgeführt.
   
    Sie können jede Überprüfung auswählen, um genauere Details anzuzeigen.

    :::image type="content" source="media/register-scan-azure-multiple-sources/multiple-scan-full-details.png" alt-text="Screenshot, das die Überprüfungsdetails anzeigt.":::

1. Zeigen Sie unten in den Quellendetails eine Zusammenfassung der letzten fehlgeschlagenen Überprüfungsausführungen an. Sie können auch genauere Details zu diesen Ausführungen anzeigen.

## <a name="manage-your-scans-edit-delete-or-cancel"></a>Verwalten Sie Ihre Überprüfungen: bearbeiten, löschen oder abbrechen
Gehen Sie zum Verwalten oder Löschen einer Überprüfung wie folgt vor:

1. Navigieren Sie zum Verwaltungscenter.
1. Wählen Sie **Datenquellen** unter dem Abschnitt **Quellen und Überprüfungen** aus und wählen Sie dann die gewünschte Datenquelle aus.
1. Wählen Sie die Überprüfung aus, die Sie verwalten möchten. Führen Sie anschließend Folgendes durch: 

   - Wenn Sie die Überprüfung bearbeiten möchten, wählen Sie **Bearbeiten** aus.
   - Sie können die Überprüfung löschen, indem Sie **Löschen** wählen.
   - Wenn die Überprüfung ausgeführt wird, kann sie abgebrochen werden, indem Sie **Abbrechen** auswählen.

## <a name="next-steps"></a>Nächste Schritte

- [Browsen Sie im Azure Purview Datenkatalog](how-to-browse-catalog.md)
- [Durchsuchen Sie den Azure Purview Datenkatalog](how-to-search-catalog.md)    
