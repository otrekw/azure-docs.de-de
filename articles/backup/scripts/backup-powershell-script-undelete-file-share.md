---
title: 'PowerShell-Skript: Wiederherstellen einer gelöschten Dateifreigabe'
description: Erfahren Sie, wie Sie ein Azure PowerShell-Skript verwenden, um eine versehentlich gelöschte Dateifreigabe wiederherzustellen.
ms.topic: sample
ms.date: 02/02/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 99cd980e13b3b9b9a7a450520218c76ccf26fd69
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "89079862"
---
# <a name="powershell-script-to-undelete-an-accidentally-deleted-file-share"></a>PowerShell-Skript zum Wiederherstellen einer versehentlich gelöschten Dateifreigabe

Dieses Skript unterstützt Sie beim Wiederherstellen einer Dateifreigabe, wenn sie versehentlich gelöscht wurde. Die Sicherheitsfunktion „Vorläufiges Löschen“ für Dateifreigaben bietet Ihnen die Möglichkeit, eine Dateifreigabe innerhalb der 14-tägigen Aufbewahrungsfrist wiederherzustellen, sodass Sie den gesamten Inhalt Ihrer Dateifreigabe sowie alle Momentaufnahmen und Wiederherstellungspunkte wiederherstellen können. Weitere Informationen zum vorläufigen Löschen finden Sie unter diesem [Link](../soft-delete-azure-file-share.md).

## <a name="sample-script"></a>Beispielskript

```powershell
#Import-Module Az.Storage -MinimumVersion 1.7.0 -Scope Local
Param(
        [Parameter(Mandatory=$True)][System.String] $ResourceGroupName,
        [Parameter(Mandatory=$True)][System.String] $StorageAccountName,
        [Parameter(Mandatory=$True)][System.String] $FileShareName,
        [Parameter(Mandatory=$True)][System.String] $SubscriptionId,
        [Parameter(Mandatory=$False)][System.Boolean] $ListOption,
        [Parameter(Mandatory=$False)][System.String] $DeletedShareVersion
    )

Function Restore-DeletedFileShare
{
    Param(
        [Parameter(Mandatory=$True)][Microsoft.WindowsAzure.Commands.Common.Storage.LazyAzureStorageContext] $Context,
        [Parameter(Mandatory=$True)][System.String] $FileShareName,
        [Parameter(Mandatory=$False)][System.String] $DeletedShareVersion
        )

    if ([string]::IsNullOrWhiteSpace($FileShareName))
    {
        Write-Error "Please specify the required input parameter: FileShareName" -ErrorAction Stop
    }

    $FileShareName = $FileShareName.ToLowerInvariant()

    Write-Verbose "Restoring a file share with the name: $FileShareName" -Verbose


    Write-Information -MessageData "Started: Creating SASToken to List File Shares" -InformationAction Continue

    $listToken = New-AzStorageAccountSASToken -Context $Context -Service File -ResourceType Service -Permission "l" -Protocol HttpsOrHttp -StartTime (Get-Date).AddHours(-1) -ExpiryTime (Get-Date).AddHours(1)

    Write-Information -MessageData "Completed: Creating SASToken to List File Shares" -InformationAction Continue

    Write-Information -MessageData "Started: Listing File Shares to find the deleted file share" -InformationAction Continue

    $listSharesUrl = [string]::Concat($Context.FileEndPoint, "?include=metadata,deleted&comp=list&api-version=2019-10-10&", $listToken.Substring(1))

    $listSharesResponse = Invoke-WebRequest $listSharesUrl -Method "GET" -Verbose

    if ($listSharesResponse.StatusCode -ne 200)
    {
        Write-Error "Request to list file shares failed." -ErrorAction Stop
    }

    Write-Verbose $listSharesResponse.RawContent -Verbose

    $listSharesResponseContent = $listSharesResponse.Content.Substring(3)

    Write-Information -MessageData "Completed: Listing File Shares to find the deleted file share" -InformationAction Continue

    Write-Information -MessageData "Started: Search for a deleted file share with the specified name" -InformationAction Continue

    $deletedFileShares = Select-Xml -Content $listSharesResponseContent -XPath "/EnumerationResults/Shares/Share[Deleted=""true"" and Name=""$FileShareName""]"

    $matchedCount = 0
    $deletedShareVersions = New-Object System.Collections.Generic.List[string]

    foreach($share in $deletedFileShares)
    {
        if($matchedCount -eq 0)
        {
          Write-Verbose $share.Node.InnerXml -Verbose

          Write-Information -MessageData "Completed: Search for a deleted file share with the specified name And Found versions" -InformationAction Continue
        }

        $shareVer = $share.Node.Item("Version").InnerText
        $shareDelTime = $share.Node.Item("Properties").Item("DeletedTime").InnerText
        $retDays = $share.Node.Item("Properties").Item("RemainingRetentionDays").InnerText

        $deletedShareVersions.Add($share.Node.Item("Version").InnerText)

        Write-Information -MessageData "DeletedVersion: $shareVer, DeletedTime: $shareDelTime, RemainingRetentionDays: $retDays"  -InformationAction Continue

        $matchedCount++
    }

    if($ListOption -eq $True)
    {
       return;
    }

    if ($matchedCount -eq 0)
    {
        Write-Error "Deleted file share with the specified name was not found." -ErrorAction Stop
    }
    elseif($matchedCount -eq 1 -and ([string]::IsNullOrWhiteSpace($DeletedShareVersion) -or $deletedShareVersions.Contains($DeletedShareVersion)))
    {
      $DeletedShareVersion = $deletedShareVersions
    }
    elseif ($matchedCount -gt 1)
    {
      if ([string]::IsNullOrWhiteSpace($DeletedShareVersion) -or !$deletedShareVersions.Contains($DeletedShareVersion))
      {
        Write-Error "More than one share with the specified name was found. Please specify a valid DeletedShareVersion parameter from above possible values." -ErrorAction Stop
      }
    }

    Write-Information -MessageData "Completed: Search for a deleted file share with the specified name And Found version: $DeletedShareVersion" -InformationAction Continue

    Write-Information -MessageData "Started: Creating SASToken to Restore File Share" -InformationAction Continue

    $restoreToken = New-AzStorageAccountSASToken -Context $Context -Service File -ResourceType Container -Permission "w" -Protocol HttpsOrHttp -StartTime (Get-Date).AddHours(-1) -ExpiryTime (Get-Date).AddHours(1)

    Write-Information -MessageData "Completed: Creating SASToken to Restore File Share" -InformationAction Continue

    Write-Information -MessageData "Started: Restore File Share" -InformationAction Continue

 $restoreShareUrl = [string]::Concat($Context.FileEndPoint, $FileShareName, "?restype=share&comp=undelete&api-version=2019-10-10&", $restoreToken.Substring(1))

    $restoreHeaders = @{"x-ms-deleted-share-name" = $FileShareName; "x-ms-deleted-share-version" = $DeletedShareVersion}

    $restoreResponse = Invoke-WebRequest $restoreShareUrl -Headers $restoreHeaders -Method "PUT" -Verbose

    if ($restoreResponse.StatusCode -ne 201)
    {
        Write-Error "Request to restore a file share failed." -ErrorAction Stop
    }

    Write-Verbose $restoreResponse.RawContent -Verbose

    Write-Information -MessageData "Completed: Restore File Share" -InformationAction Continue
}

Connect-AzAccount
Select-AzSubscription -Subscription $SubscriptionId
$sa = Get-AzStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName


Restore-DeletedFileShare $sa.Context $FileShareName $DeletedShareVersion
```

## <a name="how-to-use-the-script-in-different-scenarios"></a>Verwenden des Skripts in verschiedenen Szenarien

### <a name="prerequisites"></a>Voraussetzungen

1. Installieren Sie die neuesten Azure PowerShell Az-Module von [diesem Link](/powershell/azure/install-az-ps), bevor Sie das Skript ausführen.
2. Halten Sie die folgenden Angaben griffbereit, da Sie sie als Werte für verschiedene Parameter des Skripts übergeben müssen:

    * **-SubscriptionId**: ID des Abonnements, unter dem sich die Dateifreigabe befindet.
    * **-ResourceGroupName**: Ressourcengruppe des Speicherkontos, das die Dateifreigabe hostet.
    * **-StorageAccountName**: Name des Speicherkontos, das die Dateifreigabe hostet.
    * **-FileShareName**: Name der Dateifreigabe, die wiederhergestellt werden soll.

### <a name="execution-steps"></a>Ausführungsschritte

1. Speichern Sie das obige Skript auf Ihrem Computer mit einem Namen Ihrer Wahl. In diesem Beispiel haben wir es als *Undelete.ps1* gespeichert.
2. Führen Sie das Skript gemäß dem Szenario aus, das Ihren Anforderungen entspricht.

#### <a name="scenario-1"></a>Szenario 1

Es gibt keine mehrfach gelöschten Versionen mit dem gleichen Namen wie die Dateifreigabe, die Sie wiederherstellen möchten.

Im folgenden Beispiel wird die Dateifreigabe *share1* im Speicherkonto *afsshare* wiederhergestellt.

```powershell
   .\UnDelete.ps1 -ResourceGroupName afsshare -StorageAccountName afsshare -SubscriptionId f75d8d8b-6735-4697-82e1-1a7a3ff0d5d4 -FileShareName share1
```

Die Ausgabe sollte die Meldung `Completed:Restore File Share` anzeigen.

#### <a name="scenario-2"></a>Szenario 2

Es gibt mehrfach gelöschten Versionen mit dem gleichen Namen wie die Dateifreigabe, die Sie wiederherstellen möchten.

Im folgenden Beispiel wird eine Version der Dateifreigabe gelöscht *share1* wiederhergestellt.

##### <a name="step-1"></a>Schritt 1

Führen Sie das Skript unter Angabe des Dateifreigabenamens wie folgt aus.

```PowerShell
   .\UnDelete.ps1 -ResourceGroupName afsshare -StorageAccountName afsshare -SubscriptionId f75d8d8b-6735-4697-82e1-1a7a3ff0d5d4 -FileShareName share1
```

```Output
Completed: Search for a deleted file share with the specified name and Found versions
DeletedVersion: 01D5D7F77ACC7864, DeletedTime: Fri, 31 Jan 2020 05:30:33 GMT, RemainingRetentionDays: 14
DeletedVersion: 01D5D7F7A76CAF42, DeletedTime: Fri, 31 Jan 2020 05:31:25 GMT, RemainingRetentionDays: 14
Restore-DeletedFileShare : More than one share with the specified name was found. Please specify a valid DeletedShareVersion parameter from above possible values.
```

##### <a name="step-2"></a>Schritt 2

Wählen Sie die Version aus der Ausgabe von Schritt 1 aus, die Sie wiederherstellen möchten, und übergeben Sie sie als Wert für den Parameter **-DeletedShareVersion**.

Im folgenden Beispiel wird die Version *01D5D7F77ACC7864* der Dateifreigabe *share1* wiederhergestellt.

```powershell
   .\UnDelete.ps1 -ResourceGroupName afsshare-StorageAccountName afsshare -SubscriptionId f75d8d8b-6735-4697-82e1-1a7a3ff0d5d4 -FileShareName share1 -DeletedShareVersion 01D5D7F77ACC7864
```
