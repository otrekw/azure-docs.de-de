---
title: Überprüfen von Azure Synapse-Arbeitsbereichen
description: Hier erfahren Sie, wie Sie einen Synapse-Arbeitsbereich in Ihrem Azure Purview-Datenkatalog überprüfen.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 06/11/2021
ms.openlocfilehash: 20ad72a1c0e464c8d34442dd2d7056d3f7b4eea7
ms.sourcegitcommit: 23040f695dd0785409ab964613fabca1645cef90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112060946"
---
# <a name="register-and-scan-azure-synapse-workspaces"></a>Registrieren und Überprüfen von Azure Synapse-Arbeitsbereichen

In diesem Artikel erfahren Sie, wie Sie ein Azure Synapse-Arbeitsbereich in Purview registrieren und eine Überprüfung dafür einrichten.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Überprüfungen von Azure Synapse-Arbeitsbereichen unterstützen das Erfassen von Metadaten und Schemas für darin befindliche dedizierte und serverlose SQL-Datenbanken. Darüber hinaus werden die Daten dabei basierend auf System- und benutzerdefinierten Klassifizierungsregeln automatisch klassifiziert.

## <a name="prerequisites"></a>Voraussetzungen

- Erstellen Sie vor dem Registrieren der Datenquellen zunächst ein Azure Purview-Konto. Weitere Informationen zum Erstellen eines Purview-Kontos finden Sie unter [Schnellstart: Erstellen eines Azure Purview-Kontos im Azure-Portal](create-catalog-portal.md).
- Sie müssen ein Azure Purview-Datenquellenadministrator sein.
- Einrichten der Authentifizierung, wie in den folgenden Abschnitten beschrieben

## <a name="steps-to-register-and-scan-a-synapse-workspace"></a>Schritte zum Registrieren und Überprüfen eines Synapse-Arbeitsbereichs

> [!NOTE]
> Diese Schritte **müssen** genau in der angegebenen Reihenfolge ausgeführt werden und es müssen ggf. die genauen Berechtigungen angewendet werden, die in jedem Schritt angegeben sind, um Ihren Arbeitsbereich erfolgreich zu überprüfen.

### <a name="step-1-register-your-source-a-user-with-at-least-reader-role-on-the-synapse-workspace-who-is-also-a-data-source-admin-in-purview-can-carry-out-this-step"></a>**Schritt 1**: Registrieren Ihrer Quelle (Ein Benutzer mit mindestens der Rolle Leser im Synapse-Arbeitsbereich, der auch ein Datenquellenadministrator in Purview ist, kann diesen Schritt ausführen)

So registrieren Sie in Ihrem Datenkatalog eine neue Azure Synapse-Quelle:

1. Navigieren Sie zu Ihrem Purview-Konto.
1. Wählen Sie im linken Navigationsbereich die Option **Quellen** aus.
1. Wählen Sie **Registrieren** aus.
1. Wählen Sie unter **Register sources** (Quellen registrieren) die Option **Azure Synapse Analytics (mehrere)** aus.
1. Wählen Sie **Weiter** aus.

   :::image type="content" source="media/register-scan-synapse-workspace/register-synapse-source.png" alt-text="Einrichten einer Azure Synapse-Quelle":::

Gehen Sie unter **Register sources (Azure Synapse Analytics)** (Quellen registrieren (Azure Synapse Analytics)) wie folgt vor:

1. Geben Sie unter **Name** einen Namen ein, unter dem die Datenquelle im Katalog aufgeführt werden soll.
1. Wählen Sie optional ein **Abonnement** aus, nach dem gefiltert werden soll.
1. **Wählen Sie einen Synapse-Arbeitsbereichsnamen** im Dropdownmenü aus. Die SQL-Endpunkte werden basierend auf Ihrer Arbeitsbereichsauswahl automatisch ausgefüllt. 
1. Wählen Sie eine **Sammlung** aus, oder erstellen Sie eine neue Sammlung (optional).
1. Wählen Sie **Fertig stellen** aus, um die Datenquelle zu registrieren.

    :::image type="content" source="media/register-scan-synapse-workspace/register-synapse-source-details.png" alt-text="Ausfüllen der Details für die Azure-Synapse-Quelle":::


### <a name="step-2-applying-permissions-to-enumerate-the-contents-of-the-workspace"></a>**Schritt 2**: Anwenden von Berechtigungen zur Enumeration des Arbeitsbereichsinhalts

#### <a name="setting-up-authentication-for-enumerating-dedicated-sql-database-resources-under-a-synapse-workspace"></a>Einrichten der Authentifizierung zum Aufzählen dedizierter SQL-Datenbank-Ressourcen in einem Synapse-Arbeitsbereich

1. Navigieren Sie im Azure-Portal zur Azure Synapse-Arbeitsbereichsressource.  
1. Wählen Sie im linken Navigationsmenü  **Zugriffssteuerung (IAM)**   aus. 
1. Sie müssen Besitzer oder Benutzerzugriffsadministrator sein, um der Ressource eine Rolle hinzuzufügen. Wählen Sie die Schaltfläche *+Hinzufügen* aus. 
1. Legen Sie die Rolle auf **Leser** fest, und geben Sie unter „Auswählen“ den Namen Ihres Azure Purview-Kontos (der dessen MSI darstellt) ein. Klicken Sie auf *Speichern*, um die Rollenzuweisung abzuschließen.

> [!NOTE]
> Die Rolle kann auch von einer höheren Ebene wie **Ressourcengruppe** oder **Abonnement** zugewiesen werden, wenn Sie mehrere Azure Synapse-Arbeitsbereiche in Ihrem Azure Purview-Konto registrieren und überprüfen möchten. 

#### <a name="setting-up-authentication-for-enumerating-serverless-sql-database-resources-under-a-synapse-workspace"></a>Einrichten der Authentifizierung zum Aufzählen serverloser SQL-Datenbank-Ressourcen in einem Synapse-Arbeitsbereich

> [!NOTE]
> Sie müssen **Synapse-Administrator** im Arbeitsbereich sein, um diese Befehle ausführen zu können. [Hier](../synapse-analytics/security/how-to-set-up-access-control.md) erfahren Sie mehr über Synapse-Berechtigungen.

1. Navigieren Sie zu Ihrem Synapse-Arbeitsbereich.
1. Navigieren Sie zum Abschnitt **Daten** und zu einer Ihrer serverlosen SQL-Datenbanken.
1. Klicken Sie auf das Symbol mit den Auslassungspunkten, und starten Sie ein neues SQL-Skript.
1. Fügen Sie die MSI des Azure Purview-Kontos (dargestellt durch den Kontonamen) in den serverlosen SQL-Datenbanken als **db_datareader** hinzu, indem Sie den folgenden Befehl in Ihrem SQL-Skript ausführen:
    ```sql
    CREATE LOGIN [PurviewAccountName] FROM EXTERNAL PROVIDER;
    CREATE USER [PurviewAccountName] FOR LOGIN [PurviewAccountName];
    ALTER ROLE db_datareader ADD MEMBER [PurviewAccountName]; 
    ```
> [!NOTE]
> Wiederholen Sie den vorherigen Schritt für alle serverlosen SQL-Datenbanken in Ihrem Synapse-Arbeitsbereich. 


1. Navigieren Sie im Azure-Portal zu der **Ressourcengruppe** oder dem **Abonnement**, in der bzw. dem sich der Synapse-Arbeitsbereich befindet.
1. Wählen Sie im linken Navigationsmenü  **Zugriffssteuerung (IAM)**   aus. 
1. Sie müssen **Besitzer** oder **Benutzerzugriffsadministrator** sein, um der Ressourcengruppe oder dem Abonnement eine Rolle hinzuzufügen. Wählen Sie die Schaltfläche *+Hinzufügen* aus. 
1. Legen Sie die Rolle auf **Leser von Speicherblobdaten** fest, und geben Sie im Eingabefeld „Auswählen“ den Namen Ihres Azure Purview-Kontos (der dessen MSI darstellt) ein. Klicken Sie auf *Speichern*, um die Rollenzuweisung abzuschließen.

### <a name="step-3-applying-permissions-to-scan-the-contents-of-the-workspace"></a>**Schritt 3**: Anwenden von Berechtigungen zum Überprüfen des Arbeitsbereichsinhalts

Es gibt zwei Möglichkeiten, die Authentifizierung für eine Azure Synapse-Quelle einzurichten:

- Verwaltete Identität
- Dienstprinzipal

> [!NOTE]
> Sie müssen die Authentifizierung für jede dedizierte SQL-Datenbank innerhalb Ihres Synapse-Arbeitsbereichs einrichten, die Sie registrieren und überprüfen möchten. Die unten genannten Berechtigungen für die serverlose SQL-Datenbank gelten für alle in Ihrem Arbeitsbereich, d. h., Sie müssen sie nur einmal ausführen.

#### <a name="using-managed-identity-for-dedicated-sql-databases"></a>Verwenden der verwalteten Identität für dedizierte SQL-Datenbanken

1. Navigieren Sie zu Ihrem **Synapse-Arbeitsbereich**.
1. Navigieren Sie zum Abschnitt **Daten** und zu einer Ihrer dedizierten SQL-Datenbanken.
1. Klicken Sie auf das Symbol mit den Auslassungspunkten, und starten Sie ein neues SQL-Skript.
1. Fügen Sie die MSI des Azure Purview-Kontos (dargestellt durch den Kontonamen) als **db_datareader** in der dedizierten SQL-Datenbank hinzu, indem Sie den folgenden Befehl in Ihrem SQL-Skript ausführen:

    ```sql
    CREATE USER [PurviewAccountName] FROM EXTERNAL PROVIDER
    GO
    
    EXEC sp_addrolemember 'db_datareader', [PurviewAccountName]
    GO
    ```
> [!NOTE]
> Wiederholen Sie den vorherigen Schritt für alle dedizierten SQL-Datenbanken in Ihrem Synapse-Arbeitsbereich. 

#### <a name="using-managed-identity-for-serverless-sql-databases"></a>Verwenden der verwalteten Identität für serverlose SQL-Datenbanken

1. Navigieren Sie zu Ihrem **Synapse-Arbeitsbereich**.
1. Navigieren Sie zum Abschnitt **Daten** und zu einer Ihrer serverlosen SQL-Datenbanken.
1. Klicken Sie auf das Symbol mit den Auslassungspunkten, und starten Sie ein neues SQL-Skript.
1. Fügen Sie die MSI des Azure Purview-Kontos (dargestellt durch den Kontonamen) in den serverlosen SQL-Datenbanken als **db_datareader** hinzu, indem Sie den folgenden Befehl in Ihrem SQL-Skript ausführen:
    ```sql
    CREATE LOGIN [PurviewAccountName] FROM EXTERNAL PROVIDER;
    CREATE USER [PurviewAccountName] FOR LOGIN [PurviewAccountName];
    ALTER ROLE db_datareader ADD MEMBER [PurviewAccountName]; 
    ```
> [!NOTE]
> Wiederholen Sie den vorherigen Schritt für alle serverlosen SQL-Datenbanken in Ihrem Synapse-Arbeitsbereich. 

#### <a name="using-service-principal-for-dedicated-sql-databases"></a>Verwenden des Dienstprinzipals für dedizierte SQL-Datenbanken

> [!NOTE]
> Sie müssen zunächst neue **Anmeldeinformationen** vom Typ „Dienstprinzipal“ einrichten, indem Sie die Anweisungen [hier](manage-credentials.md)befolgen.

1. Navigieren Sie zu Ihrem **Synapse-Arbeitsbereich**.
1. Navigieren Sie zum Abschnitt **Daten** und zu einer Ihrer dedizierten SQL-Datenbanken.
1. Klicken Sie auf das Symbol mit den Auslassungspunkten, und starten Sie ein neues SQL-Skript.
1. Fügen Sie die **Dienstprinzipal-ID** als **db_datareader** für die dedizierte SQL-Datenbank-Instanz hinzu, indem Sie den folgenden Befehl in Ihrem SQL-Skript ausführen:

    ```sql
    CREATE USER [ServicePrincipalID] FROM EXTERNAL PROVIDER
    GO
    
    EXEC sp_addrolemember 'db_datareader', [ServicePrincipalID]
    GO
    ```
> [!NOTE]
> Wiederholen Sie den vorherigen Schritt für alle dedizierten SQL-Datenbanken in Ihrem Synapse-Arbeitsbereich. 

#### <a name="using-service-principal-for-serverless-sql-databases"></a>Verwenden des Dienstprinzipals für serverlose SQL-Datenbanken

1. Navigieren Sie zu Ihrem **Synapse-Arbeitsbereich**.
1. Navigieren Sie zum Abschnitt **Daten** und zu einer Ihrer serverlosen SQL-Datenbanken.
1. Klicken Sie auf das Symbol mit den Auslassungspunkten, und starten Sie ein neues SQL-Skript.
1. Fügen Sie die MSI des Azure Purview-Kontos (dargestellt durch den Kontonamen) in den serverlosen SQL-Datenbanken als **db_datareader** hinzu, indem Sie den folgenden Befehl in Ihrem SQL-Skript ausführen:
    ```sql
    CREATE LOGIN [PurviewAccountName] FROM EXTERNAL PROVIDER;
    CREATE USER [PurviewAccountName] FOR LOGIN [PurviewAccountName];
    ALTER ROLE db_datareader ADD MEMBER [PurviewAccountName]; 
    ```
> [!NOTE]
> Wiederholen Sie den vorherigen Schritt für alle serverlosen SQL-Datenbanken in Ihrem Synapse-Arbeitsbereich. 

### <a name="step-4-setting-up-synapse-workspace-firewall-access"></a>**Schritt 4**: Einrichten des Firewallzugriffs für den Synapse-Arbeitsbereich

1. Navigieren Sie im Azure-Portal zum Synapse-Arbeitsbereich. 

3. Wählen Sie im linken Navigationsbereich die Option Firewalls aus.

4. Klicken Sie unter **Anderen Azure-Diensten und -Ressourcen den Zugriff auf diesen Arbeitsbereich gestatten** auf **EIN**.

5. Klicken Sie auf Speichern.

### <a name="step-5-setting-up-a-scan-on-the-workspace"></a>**Schritt 5**: Einrichten einer Überprüfung für den Arbeitsbereich

Gehen Sie zum Erstellen und Ausführen einer neuen Überprüfung wie folgt vor:

1. Navigieren Sie zum Abschnitt **Quellen**.

1. Wählen Sie die von Ihnen registrierte Datenquelle aus.

1. Klicken Sie auf „Details anzeigen“, und wählen Sie **+ Neue Überprüfung** aus, oder verwenden Sie das Schnellaktionssymbol für die Überprüfung auf der Quellkachel.

1. Geben Sie den *Namen* ein, und wählen Sie alle Typen von Ressourcen aus, die Sie innerhalb dieser Quelle überprüfen möchten. **SQL-Datenbank** ist der einzige Typ, den wir derzeit in einem Synapse-Arbeitsbereich unterstützen.
   
    :::image type="content" source="media/register-scan-synapse-workspace/synapse-scan-setup.png" alt-text="Überprüfen der Azure Synapse-Quelle":::

1. Wählen Sie die **Anmeldeinformationen** zum Herstellen der Verbindung mit den Ressourcen in Ihrer Datenquelle aus. 
  
1. Sie können in jedem Typ auswählen, ob Sie entweder alle Ressourcen oder eine Teilmenge nach Namen überprüfen möchten.
1.  Klicken Sie auf **Weiter**, um fortzufahren. 

1.  Wählen Sie **Überprüfungsregelsätze** vom Typ „Azure Synapse SQL“ aus. Sie können Überprüfungsregelsätze auch inline erstellen.

1. Wählen Sie den Auslöser für die Überprüfung. Sie können eine **wöchentliche/monatliche** oder **einmalige** Ausführung planen.

1. Überprüfen Sie die Auswahl der Überprüfung, und schließen Sie die Einrichtung mit „Speichern“ ab.   

#### <a name="viewing-your-scans-and-scan-runs"></a>Anzeigen Ihrer Überprüfungen und Überprüfungsausführungen

1. Zeigen Sie Quelldetails an, indem Sie auf der Kachel im Quellenabschnitt auf **Details anzeigen** klicken. 

      :::image type="content" source="media/register-scan-synapse-workspace/synapse-source-details.png" alt-text="Details zur Azure Synapse-Quelle"::: 

1. Zeigen Sie die Details der Überprüfungsausführung an, indem Sie zur Seite **Überprüfungsdetails** navigieren.
    1. Die *Statusleiste* ist eine kurze Zusammenfassung zum Ausführungsstatus der untergeordneten Ressourcen. Sie wird in der Überprüfung auf Arbeitsbereichsebene angezeigt.
    1. Grün bedeutet erfolgreich, Rot weist auf einen Fehler hin. Grau bedeutet, dass die Überprüfung noch ausgeführt wird.
    1. Sie können auf die einzelnen Überprüfungen klicken, um präzisere Details anzuzeigen.

      :::image type="content" source="media/register-scan-synapse-workspace/synapse-scan-details.png" alt-text="Details der Azure Synapse-Überprüfung" lightbox="media/register-scan-synapse-workspace/synapse-scan-details.png"::: 

1. Zeigen Sie im unteren Bereich der Quelldetailsseite eine Zusammenfassung der letzten nicht erfolgreichen Überprüfungsausführungen an. Sie können auch auf präzisere Details zu diesen Ausführungen klicken.

#### <a name="manage-your-scans---edit-delete-or-cancel"></a>Verwalten Ihrer Überprüfungen: Bearbeiten, Löschen oder Abbrechen
Gehen Sie zum Verwalten oder Löschen einer Überprüfung wie folgt vor:

- Navigieren Sie zum Verwaltungscenter. Wählen Sie im Abschnitt Sources and scanning (Quellen und Überprüfung) die Option Datenquellen und anschließend die gewünschte Datenquelle aus.

- Wählen Sie die Überprüfung aus, die Sie verwalten möchten. Wenn Sie die Überprüfung bearbeiten möchten, wählen Sie Bearbeiten aus.

- Wenn Sie Ihre Überprüfung löschen möchten, wählen Sie Löschen aus.
- Wenn eine Überprüfung ausgeführt wird, können Sie sie auch abbrechen.

## <a name="next-steps"></a>Nächste Schritte

- [Browsen im Azure Purview-Datenkatalog](how-to-browse-catalog.md)
- [Suchen im Azure Purview-Datenkatalog](how-to-search-catalog.md)   
