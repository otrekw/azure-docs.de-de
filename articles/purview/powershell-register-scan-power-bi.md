---
title: Verwenden von PowerShell, um Power BI zu registrieren und zu überprüfen (Vorschau)
description: Erfahren Sie, wie Sie PowerShell verwenden, um einen Power BI-Mandanten in Azure Purview zu registrieren und zu überprüfen.
author: darrenparker
ms.author: hophan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 10/12/2020
ms.openlocfilehash: f0b541baf49307006c18f07d92bd409763a29e3a
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2020
ms.locfileid: "96550338"
---
# <a name="use-powershell-to-register-and-scan-power-bi-in-azure-purview-preview"></a>Verwenden von PowerShell, um Power BI in Azure Purview zu registrieren und zu überprüfen (Vorschau) 

In diesem Artikel ist beschrieben, wie Sie PowerShell verwenden, um eine Überprüfung eines Power BI-Mandanten in einem Azure Purview-Katalog einzurichten.

## <a name="power-bi-authentication-background"></a>Hintergrund für Power BI-Authentifizierung

Der Purview-Katalog muss eine Verbindung mit der Power BI-Administrator-API herstellen, um Artefakte in einem Power BI-Mandanten überprüfen zu können. Die Power BI-Administrator-API unterstützt derzeit zwei Arten von Authentifizierung:

- Verwaltete Identität (MSI)
- Delegierte Benutzerauthentifizierung

> [!Note]
> MSI wird empfohlen, es sei denn, es ist eine delegierte Benutzerauthentifizierung erforderlich.

## <a name="create-a-security-group"></a>Erstellen einer Sicherheitsgruppe

Jeder Purview-Katalog hat eine eigene vom System zugewiesene verwaltete Identität, der Zugriff auf den Power BI-Mandanten erteilt werden muss, um Überprüfen zu ermöglichen. Der Katalogname kann verwendet werden, um die Identität des Katalogs im Azure-Portal zu finden.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach „Azure Active Directory“.
1. Erstellen Sie eine neue Sicherheitsgruppe in Ihrem Azure Active Directory-System, indem Sie gemäß [Erstellen einer Basisgruppe und Hinzufügen von Mitgliedern mit Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal) vorgehen.

    > [!Tip]
    > Sie können diesen Schritt überspringen, wenn Sie bereits eine zu verwendende Sicherheitsgruppe haben.

1. Wählen Sie als **Gruppentyp** die Option „Sicherheit“ aus.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/security-group.png" alt-text="Gruppentyp „Sicherheit“":::

1. Fügen Sie dieser Sicherheitsgruppe die verwaltete Identität Ihres Katalogs hinzu, indem Sie „Mitglieder“ und dann **+ Mitglieder hinzufügen** auswählen.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/add-group-member.png" alt-text="Der Gruppe die verwaltete Instanz des Katalogs hinzufügen":::

1. Suchen Sie nach Ihrem Katalog, und wählen Sie ihn aus.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/add-catalog-to-group-by-search.png" alt-text="Katalog hinzufügen, indem nach ihm gesucht wird":::

1. Es sollte eine Erfolgsmeldung angezeigt werden, die Ihnen mitteilt, dass der Katalog hinzugefügt wurde.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/success-add-catalog-msi.png" alt-text="Katalog hinzufügen, MSI-Erfolg":::

## <a name="associate-the-security-group-with-power-bi"></a>Verknüpfen der Sicherheitsgruppe mit Power BI

1. Melden Sie sich beim [Power BI-Verwaltungsportal](https://app.powerbi.com/admin-portal/tenantSettings?allowServicePrincipalsUseReadAdminAPIsUI=1) an. Fügen Sie dieses Featureflag an: `allowServicePrincipalsUseReadAdminAPIsUI=1`. Dieses Flag aktiviert das Feature, das es Ihnen ermöglicht, die Sicherheitsgruppe zuzuordnen. Beispiel:

    ```http
    https://app.powerbi.com/admin-portal/tenantSettings?allowServicePrincipalsUseReadAdminAPIsUI=1`
    ```

    > [!Important]
    > Sie müssen Power BI-Administrator sein, um die Seite mit den Mandanteneinstellungen anzeigen zu können.

1. Wählen Sie **Entwicklereinstellungen** > **Dienstprinzipalen die Verwendung schreibgeschützter Power BI-Administrator-APIs gestatten (Vorschau)** aus.
1. Wählen Sie **Sicherheitsgruppen angeben** aus.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/allow-service-principals-power-bi-admin.png" alt-text="Bild, das zeigt, wie Dienstprinzipale schreibgeschützte Power BI-Administrator-API-Berechtigungen erhalten können ":::

    > [!Caution]
    > Wenn Sie der von Ihnen erstellten Sicherheitsgruppe (die Ihre verwaltete Datenkatalogidentität als Mitglied hat) gestatten, schreibgeschützte Power BI-Administrator-APIs zu verwenden, gestatten Sie ihr auch, auf die Metadaten (z. B. Dashboard- und Berichtsnamen, Besitzer, Beschreibungen usw.) für sämtliche Ihrer Power BI-Artefakte in diesem Mandanten zugreifen zu können. Sobald die Metadaten in Azure Purview eingelesen wurden, wird anhand der Pruview-Berechtigungen, nicht anhand der Power BI-Berechtigungen, festgelegt, wer diese Metadaten anzeigen kann.

    > [!Note]
    > Sie können die Sicherheitsgruppe aus Ihren Entwicklereinstellungen entfernen, die zuvor extrahierten Metadaten werden jedoch nicht aus dem Purview-Konto entfernt. Sie können sie separat löschen.

## <a name="register-power-bi-and-set-up-a-scan"></a>Registrieren von Power BI und Einrichten einer Überprüfung

Nachdem Sie dem Katalog Berechtigungen zum Herstellen einer Verbindung mit der Administrator-API Ihres Power BI-Mandanten erteilt haben, müssen Sie Ihre Überprüfung im Katalog einrichten. Zu diesem Zweck konfigurieren Sie ein PowerShell-Skript, und führen Sie dieses Skript aus.

1. Laden Sie die ADC-PowerShell-Cmdlets herunter, und extrahieren Sie diese.
1. Konfigurieren Sie Ihr Skript, indem Sie Werte für die Zuweisungen am Anfang des Skripts angeben.

    ```PowerShell
    #Purview Account Info
    $azuretenantId = '<Tenant Id>'
    $azuresubscriptionId = '<Tenant Id>'
    $azureResourceGroupName = '<Resource Group Name>'
    $azurePurviewAccountName = '<Catalog Name>'
    $createCatalog = $false #Change to true if you need a new catalog to be created
    $azureCatalogLocation = 'East Us' #The region of your account
    $dataSourceName = 'pbi-msi-datasource01' #provide a unique data source name
    $dataScanName = 'scan01' #provide a unique scan name

    #PowerShell Command Module Path
    $ModulePath = '<Full path to where you extracted the PS zip files>\Microsoft.DataCatalog.Management.Commands.dll'
    Import-Module $ModulePath

    If ($createCatalog -eq $true)
    {
      Connect-AzAccount -Tenant $azuretenantId -SubscriptionId $azuresubscriptionId
      Set-AzDataCatalog -ResourceGroupName $azureResourceGroupName -Name $azurePurviewAccountName -Location $azureCatalogLocation
    }

    Set-AzDataCatalogSessionSettings -DataCatalogSession -UserAuthentication -TenantId $azuretenantId  -DataCatalogAccountName $azurePurviewAccountName

    Set-AzDataCatalogDataSource -Name $dataSourceName -AccountType PowerBI  -Tenant $azuretenantId

    Set-AzDataCatalogScan -DataSourceName $dataSourceName -ScanName $dataScanName -PowerBIManagedInstanceMsi
    ```

    > [!Note]
    > Sie müssen ein Mitwirkender oder Besitzer für das Abonnement sein, unter dem Sie die Befehle ausführen.

1. Starten Sie die Überprüfung, indem Sie das folgende Skript ausführen:

    ```PowerShell
    Start-AzDataCatalogScan -DataSourceName $dataSourceName -Name $dataScanName -AsJob
    ```

## <a name="register-and-scan-power-bi"></a>Registrieren und Überprüfen von Power BI

Die empfohlene Authentifizierungsmethode ist MSI. Um jedoch einen Power BI-Mandanten zu überprüfen, der sich in einem anderen Azure-Mandant als Ihr Katalog befindet, verwenden Sie die delegierte Authentifizierung.

Damit Sie delegierte Authentifizierung nutzen können, benötigen Sie Administratorbenutzer-Anmeldeinformationen und Power BI-Administrator-Anmeldeinformationen. Außerdem müssen Sie eine Azure-App erstellen und dieser Power BI-Tenant.ReadAll-Berechtigungen erteilen.

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), und suchen Sie nach **App-Registrierungen**.

1. Wählen Sie in **App-Registrierungen** die Option **+ Neue Registrierung** aus.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/new-app-registration.png" alt-text="Bild, das zeigt, wie eine neue Azure-App-Registrierung erstellt wird":::

1. Geben Sie einen Namen für die App ein.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/register-new-app.png" alt-text="Neue App registrieren":::

1. Nachdem Ihre App erstellt ist, wählen Sie **API-Berechtigungen** und dann **+ Berechtigung hinzufügen** aus.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/app-permissions.png" alt-text="Bild, das zeigt, wie der App eine Berechtigung hinzugefügt wird":::

1. Wählen Sie **Power BI-Dienst** auf **API-Berechtigungen anfordern** aus.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/select-power-bi-service.png" alt-text="Bild, das zeigt, wie der PBI-Dienst ausgewählt wird":::

1. Wählen Sie **Delegierte Berechtigungen** und **Tenant.Read.All** aus. Klicken Sie dann auf **Berechtigungen hinzufügen**.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/request-api-permissions.png" alt-text="Bild, das zeigt, wie API-Berechtigungen angefordert werden":::

1. Wählen Sie **Administratoreinwilligung erteilen** aus.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/grant-admin-perms.png" alt-text="Bild, das zeigt, wie die Zustimmung des Administrators erteilt wird":::

1. Kopieren Sie die Werte von **Anwendungs-ID (Client)** und **Verzeichnis-ID (Mandant)** .  Sie werden diese Werte beim Einrichten der Überprüfung verwenden.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/copy-client-and-tenantid.png" alt-text="Bild, das zeigt, wie die Client-ID und die Mandanten-ID kopiert werden":::

1. Konfigurieren Sie Ihre Überprüfung in PowerShell. Sie werden vom Skript zur Eingabe von Anmeldeinformationen aufgefordert. Sie müssen für das Azure-Abonnement, das Sie verwenden möchten, mindestens die Rolle „Mitwirkender“ und die Rolle „Purview-Datenquellenadministrator“ haben.

    ```PowerShell
    # Purview Account Info
    $azuretenantId = '<Tenant Id>'
    $azuresubscriptionId = '<Tenant Id>'
    $azureResourceGroupName = '<Resource Group Name>'
    $azurePurviewAccountName = '<Catalog Name>'
    $createCatalog = $false
    $azureCatalogLocation = 'East Us' #The region of your account
    $dataSourceName = 'pbi-delegated-datasource01' #provide a unique data source name
    $dataScanName = 'scan01' #provide a unique scan name

    # Power BI Tenant Info
    $powerBITenantIdToScan = '<Power BI Tenant ID copied from above steps>'
    $ServicePrincipalId = '<Client ID copied from above steps>'
    $UserName = '<Power BI Admin emil ex: admin@firsttomarket.onmicrosoft.com>'
    $Password = '<Power BI Admin Password>'

    #PowerShell Command Module Path
    $ModulePath = '<Full path to where you extracted the PS zip files>\Microsoft.DataCatalog.Management.Commands.dll'
    Import-Module $ModulePath

    #Commands To Create catalog, Create DataSource, Create Datascan, Start Scan
    If($createCatalog -eq $true)
    {
      Connect-AzAccount -Tenant $azuretenantId -SubscriptionId $azuresubscriptionId
      Set-AzDataCatalog -ResourceGroupName $azureResourceGroupName -Name $azurePurviewAccountName -Location $azureCatalogLocation
    }

    Set-AzDataCatalogSessionSettings -DataCatalogSession -UserAuthentication -TenantId $azuretenantId   -DataCatalogAccountName $azurePurviewAccountName

    Set-AzDataCatalogDataSource -Name $dataSourceName -AccountType PowerBI -Tenant $powerBITenantIdToScan

    Set-AzDataCatalogScan -DataSourceName $dataSourceName -ScanName $dataScanName -PowerBIDelegated -ServicePrincipalId $ServicePrincipalId -UserName $UserName -Password $Password
    ```

1. Führen Sie Ihre Überprüfung aus.

      ```PowerShell
      Start-AzDataCatalogScan -DataSourceName $dataSourceName -Name $dataScanName -AsJob
      ```

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit Azure Purview finden Sie unter [Schnellstart: Erstellen eines Azure Purview-Kontos im Azure-Portal](create-catalog-portal.md).
