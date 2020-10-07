---
title: 'Schnellstart: Erstellen einer Instanz im Hybridmodus über das Azure-Portal'
titleSuffix: Azure Database Migration Service
description: Verwenden des Azure-Portals zum Erstellen einer Instanz von Azure Database Migration Service im Hybridmodus
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: quickstart
ms.date: 03/13/2020
ms.openlocfilehash: e63f073c4f7166cd205b85ef06589f6056b2a70f
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "84195687"
---
# <a name="quickstart-create-a-hybrid-mode-instance-with-azure-portal--azure-database-migration-service"></a>Schnellstart: Erstellen einer Instanz im Hybridmodus mit dem Azure-Portal und dem Azure Database Migration Service

Azure Database Migration Service im Hybridmodus verwaltet Datenbankmigrationen mithilfe eines lokal gehosteten Migrationsworkers und einer in der Cloud ausgeführten Instanz von Azure Database Migration Service. Der Hybridmodus ist insbesondere in Szenarien nützlich, in denen es keine Site-to-Site-Konnektivität zwischen dem lokalen Netzwerk und Azure gibt oder die Bandbreite der Site-to-Site-Konnektivität eingeschränkt ist.

>[!NOTE]
>Zurzeit unterstützt der Azure Database Migration Service im Hybridmodus SQL Server-Migrationen zu Folgendem:
>
>- Verwaltete Azure SQL-Instanz nahezu ohne Ausfallzeit (online).
>- Einzelne Azure SQL-Datenbank mit einem gewissen Maß an Ausfallzeit (offline).
>- MongoDB zu Cosmos DB nahezu ohne Ausfallzeit (online).
>- MongoDB zu Cosmos DB mit einem gewissen Maß an Ausfallzeit (offline).

In dieser Schnellstartanleitung verwenden Sie das Azure-Portal, um eine Instanz von Azure Database Migration Service im Hybridmodus zu erstellen. Danach laden Sie den Hybrid Worker herunter, installieren ihn in Ihrem lokalen Netzwerk und richten ihn ein. Während der Vorschau können Sie den Hybridmodus von Azure Database Migration Service verwenden, um Daten von einer lokalen SQL Server-Instanz zu Azure SQL-Datenbank zu migrieren.

> [!NOTE]
> Das Installationsprogramm für Azure Database Migration Service im Hybridmodus kann unter Microsoft Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 und Windows 10 ausgeführt werden.

> [!IMPORTANT]
> Für das Azure Database Migration Service-Hybridinstallationsprogramm ist mindestens .NET 4.7.2 erforderlich. Die aktuellen .NET-Versionen finden Sie auf der Seite [Herunterladen von .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Öffnen Sie Ihren Webbrowser, navigieren Sie zum [Microsoft Azure-Portal](https://portal.azure.com/), und geben Sie dann Ihre Anmeldeinformationen ein, um sich am Portal anzumelden.

Die Standardansicht ist Ihr Dienstdashboard.

## <a name="register-the-resource-provider"></a>Registrieren des Ressourcenanbieters

Registrieren Sie den Microsoft.DataMigration-Ressourcenanbieter, bevor Sie Ihre erste Instanz von Azure Database Migration Service erstellen.

1. Klicken Sie im Azure-Portal auf **Abonnements**, wählen Sie das Abonnement aus, in dem Sie die Azure Database Migration Service-Instanz erstellen möchten, und klicken Sie auf **Ressourcenanbieter**.

    ![Suchen nach einem Ressourcenanbieter](media/quickstart-create-data-migration-service-hybrid-portal/dms-portal-search-resource-provider.png)

2. Suchen Sie nach „Migration“, und wählen Sie rechts neben **Microsoft.DataMigration** die Option **Registrieren** aus.

    ![Registrieren des Ressourcenanbieters](media/quickstart-create-data-migration-service-hybrid-portal/dms-portal-register-resource-provider.png)

## <a name="create-an-instance-of-the-service"></a>Erstellen einer Instanz des Diensts

1. Klicken Sie auf **+ Ressource erstellen**, um eine Instanz von Azure Database Migration Service zu erstellen.

2. Suchen Sie im Marketplace nach „Migration“, wählen Sie **Azure Database Migration Service** aus, und klicken Sie dann auf dem Bildschirm **Azure Database Migration Service** auf **Erstellen**.

3. Auf dem Bildschirm **Create Migration Service** (Migrationsdienst erstellen):

    - Wählen Sie einen **Dienstnamen** aus, der einprägsam und in Ihrer Azure Database Migration Service-Instanz eindeutig ist.
    - Wählen Sie das Azure-**Abonnement** aus, in dem Sie die Instanz erstellen möchten.
    - Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder erstellen Sie eine neue.
    - Wählen Sie den **Ort** aus, der die geringste Entfernung zum Quell- oder Zielserver aufweist.
    - Wählen Sie unter **Dienstmodus** die Option **Hybrid (Vorschau)** aus.

         ![Migrationsdienst erstellen: Allgemeine Informationen](media/quickstart-create-data-migration-service-hybrid-portal/dms-create-service-basics.png)

4. Klicken Sie auf **Überprüfen + erstellen**.

5. Lesen Sie auf der Registerkarte **Überprüfen + erstellen** die Bedingungen, überprüfen Sie die weiteren bereitgestellten Informationen, und klicken Sie dann auf **Erstellen**.

    ![Migrationsdienst erstellen: Überprüfen + erstellen](media/quickstart-create-data-migration-service-hybrid-portal/dms-create-service-review-and-create.png)

    Nach einigen Augenblicken wird Ihre Azure Database Migration Service-Instanz im Hybridmodus erstellt und kann eingerichtet werden. Die Azure Database Migration Service-Instanz wird wie in der folgenden Abbildung angezeigt:

    ![Azure Database Migration Service-Instanz im Hybridmodus](media/quickstart-create-data-migration-service-hybrid-portal/dms-instance-hybrid-mode.png)

6. Nachdem der Dienst erstellt wurde, klicken Sie auf **Eigenschaften**, und kopieren Sie den im Feld **Ressourcen-ID** angezeigten Wert. Diesen Wert verwenden Sie, um den Hybrid Worker für Azure Database Migration Service zu installieren.

    ![Azure Database Migration Service im Hybridmodus: Eigenschaften](media/quickstart-create-data-migration-service-hybrid-portal/dms-copy-resource-id.png)

## <a name="create-azure-app-registration-id"></a>Erstellen einer Azure-App-Registrierungs-ID

Sie müssen eine Azure-App-Registrierungs-ID erstellen, die der lokale Hybrid Worker für die Kommunikation mit Azure Database Migration Service in der Cloud verwenden kann.

1. Wählen Sie im Azure-Portal **Azure Active Directory**, dann **App-Registrierungen** und dann **Neue Registrierung** aus.
2. Geben Sie einen Namen für die Anwendung an, und wählen Sie dann unter **Unterstützte Kontotypen** den zu unterstützenden Kontotyp aus, um anzugeben, wer die Anwendung verwenden darf.

    ![Azure Database Migration Service im Hybridmodus: Anwendung registrieren](media/quickstart-create-data-migration-service-hybrid-portal/dms-register-application.png)

3. Verwenden Sie für die Felder **Umleitungs-URI (optional)** die Standardwerte, und klicken Sie dann auf **Registrieren**.

4. Nachdem die App-ID registriert wurde, notieren Sie sich die **Anwendungs-ID (Client)** , die Sie beim Installieren des Hybrid Workers verwenden werden.

5. Navigieren Sie im Azure-Portal zum Azure Database Migration Service, wählen Sie **Zugriffssteuerung (IAM)** aus, und klicken Sie dann auf **Rollenzuweisung hinzufügen**, um den Zugriff als Mitwirkender zur App-ID hinzuzufügen.

    ![Azure Database Migration Service im Hybridmodus: Rollenzuweisung als „Mitwirkender“](media/quickstart-create-data-migration-service-hybrid-portal/dms-app-assign-contributor.png)

6. Wählen Sie **Mitwirkender** als Rolle aus, weisen Sie den Zugriff für **Azure AD-Benutzer oder Dienstprinzipal** zu, und wählen Sie dann den App-ID-Namen aus.

    ![Azure Database Migration Service im Hybridmodus: Details der Rollenzuweisung als „Mitwirkender“](media/quickstart-create-data-migration-service-hybrid-portal/dms-add-role-assignment.png)

7. Klicken Sie auf **Speichern**, um die Rollenzuweisung für die App-ID in der Azure Database Migration Service-Ressource zu speichern.

## <a name="download-and-install-the-hybrid-worker"></a>Herunterladen und Installieren des Hybrid Workers

1. Navigieren Sie im Azure-Portal zu Ihrer Instanz von Azure Database Migration Service.

2. Wählen Sie unter **Einstellungen** die Option **Hybrid** aus, und klicken Sie dann auf **Installationsprogramm herunterladen**.

    ![Azure Database Migration Service: Hybrid Worker herunterladen](media/quickstart-create-data-migration-service-hybrid-portal/dms-installer-download.png)

3. Extrahieren Sie die ZIP-Datei auf dem Server, der den Hybrid Worker für Azure Database Migration Service hosten soll.

    > [!IMPORTANT]
    > Für das Azure Database Migration Service-Hybridinstallationsprogramm ist mindestens .NET 4.7.2 erforderlich. Die aktuellen .NET-Versionen finden Sie auf der Seite [Herunterladen von .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework).

4. Suchen und öffnen Sie im Installationsordner die Datei **dmsSettings.json**, geben Sie den Wert für **ApplicationId** und **resourceId** an, und speichern Sie dann die Datei.

    ![Azure Database Migration Service: Hybrid Worker-Einstellungen](media/quickstart-create-data-migration-service-hybrid-portal/dms-settings.png)

5. Generieren Sie mithilfe des folgenden Befehls ein Zertifikat, mit dem Azure Database Migration Service die Kommunikation vom Hybrid Worker authentifizieren kann.

    ```
    <drive>:\<folder>\Install>DMSWorkerBootstrap.exe -a GenerateCert
    ```

    Das Zertifikat wird im Installationsordner generiert.

    ![Azure Database Migration Service: Hybrid Worker-Zertifikat](media/quickstart-create-data-migration-service-hybrid-portal/dms-certificate.png)

6. Navigieren Sie im Azure-Portal zur App-ID, und wählen Sie unter **Verwalten** die Option **Zertifikate & Geheimnisse** und dann **Zertifikat hochladen** aus, um das generierte öffentliche Zertifikat auszuwählen.

    ![Azure Database Migration Service: Hybrid Worker-Zertifikat hochladen](media/quickstart-create-data-migration-service-hybrid-portal/dms-app-upload-certificate.png)

7. Installieren Sie den Hybrid Worker für Azure Database Migration Service auf Ihrem lokalen Server, indem Sie den folgenden Befehl ausführen:

    ```
    <drive>:\<folder>\Install>DMSWorkerBootstrap.exe -a Install -IAcceptDMSLicenseTerms -d
    ```

    > [!NOTE]
    > Beim Ausführen des Installationsbefehls können Sie auch die folgenden Parameter verwenden:
    >
    > - **-TelemetryOptOut**: Beendet das Senden von Telemetriedaten durch den Worker, sorgt aber weiterhin für eine minimale lokale Protokollierung.  Das Installationsprogramm sendet weiterhin Telemetriedaten.
    > - **-p {InstallLocation}** . Ermöglicht das Ändern des Installationspfads, der standardmäßig „C:\Programme\DatabaseMigrationServiceHybrid“ lautet.

8. Wenn das Installationsprogramm ohne Fehler ausgeführt wird, zeigt der Dienst in Ihrer Azure Database Migration Service-Instanz den Status „Online“ an, und Sie können Ihre Datenbanken migrieren.

    ![Azure Database Migration Service: Status „online“](media/quickstart-create-data-migration-service-hybrid-portal/dms-instance-hybrid-mode-online.png)

## <a name="uninstall-azure-database-migration-service-hybrid-mode"></a>Deinstallieren der Azure Database Migration Service-Instanz im Hybridmodus

Zurzeit wird das Deinstallieren einer Azure Database Migration Service-Instanz im Hybridmodus nur über das Installationsprogramm unterstützt, mit dem der Hybrid Worker für Azure Database Migration Service auf Ihrem lokalen Server installiert wird. Führen Sie den folgenden Befehl aus:

```
<drive>:\<folder>\Install>DMSWorkerBootstrap.exe -a uninstall
```

> [!NOTE]
> Beim Ausführen des Deinstallationsbefehls können Sie auch den Parameter „-ReuseCert“ verwenden, der dafür sorgt, dass das vom generateCert-Workflow generierte AdApp-Zertifikat beibehalten wird.  Damit kann das gleiche Zertifikat wiederverwendet werden, das zuvor generiert und hochgeladen wurde.

## <a name="set-up-the-azure-database-migration-service-hybrid-worker-using-powershell"></a>Einrichten des Hybrid Workers für Azure Database Migration Service mithilfe von PowerShell

Zusätzlich zu der Möglichkeit, den Hybrid Worker für Azure Database Migration Service über das Azure-Portal zu installieren, stellen wir auch ein [PowerShell-Skript](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/119/1/DMS_Hybrid_Script.zip) bereit, mit dem Sie die Workerinstallationsschritte automatisieren können, nachdem Sie eine neue Instanz von Azure Database Migration Service im Hybridmodus erstellt haben. Mit dem Skript wird Folgendes durchgeführt:

1. Erstellt eine neue AdApp.
2. Lädt das Installationsprogramm herunter.
3. Führt den generateCert-Workflow aus.
4. Lädt das Zertifikat hoch.
5. Fügt die AdApp als Mitwirkenden zu Ihrer Azure Database Migration Service-Instanz hinzu.
6. Führt den Installationsworkflow aus.

Das Skript dient zur schnellen Prototyperstellung, wenn ein Benutzer bereits über alle erforderlichen Berechtigungen in der Umgebung verfügt. Beachten Sie, dass AdApp und Zertifikat in Ihrer Produktionsumgebung möglicherweise unterschiedliche Anforderungen aufweisen, daher kann beim Skript ein Fehler auftreten.

> [!IMPORTANT]
> Das Skript setzt voraus, dass bereits eine Instanz von Azure Database Migration Service im Hybridmodus vorhanden ist und dass das verwendete Azure-Konto über die erforderlichen Berechtigungen verfügt, um AdApps im Mandanten zu erstellen und den rollenbasierten Benutzerzugriff im Abonnement zu ändern.

Füllen Sie die Parameter am Anfang des Skripts aus, und führen Sie das Skript dann in einer PowerShell-Instanz mit Administratorrechten aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Migrieren von SQL Server zu einer verwalteten Azure SQL-Instanz online](tutorial-sql-server-managed-instance-online.md)
> [Migrieren von SQL Server zu einer verwalteten Azure SQL-Instanz offline](tutorial-sql-server-to-azure-sql.md)
