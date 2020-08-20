---
title: Globale Parameter
description: Festlegen von globalen Parametern für jede Azure Data Factory-Umgebung
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: djpmsft
ms.author: daperlov
ms.date: 08/05/2020
ms.openlocfilehash: 052f502ed27db9ade0fd2916f91d6922c52a5a98
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87854125"
---
# <a name="global-parameters-in-azure-data-factory"></a>Globale Parameter in Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Globale Parameter sind Konstanten in einer Data Factory, die von einer Pipeline in beliebigen Ausdrücken verbraucht werden können. Sie sind hilfreich, wenn Sie über mehrere Pipelines mit identischen Parameternamen und -werten verfügen. Beim Höherstufen einer Data Factory per CI/CD-Prozess (Continuous Integration/Deployment) können Sie diese Parameter in einer Umgebung jeweils außer Kraft setzen. 

## <a name="creating-global-parameters"></a>Erstellen globaler Parameter

Navigieren Sie zum Erstellen eines globalen Parameters im Abschnitt *Verwalten* zur Registerkarte *Globale Parameter*. Wählen Sie **Neu** aus, um den Seitennavigationsbereich für die Erstellung zu öffnen.

![Erstellen globaler Parameter](media/author-global-parameters/create-global-parameter-1.png)

Geben Sie im Seitennavigationsbereich einen Namen ein, wählen Sie einen Datentyp aus, und geben Sie den Wert Ihres Parameters an.

![Erstellen globaler Parameter](media/author-global-parameters/create-global-parameter-2.png)

Nachdem ein globaler Parameter erstellt wurde, können Sie ihn bearbeiten, indem Sie auf seinen Namen klicken. Wählen Sie zum gleichzeitigen Ändern mehrerer Parameter die Option **Edit all** (Alle bearbeiten) aus.

![Erstellen globaler Parameter](media/author-global-parameters/create-global-parameter-3.png)

## <a name="using-global-parameters-in-a-pipeline"></a>Verwenden globaler Parameter in einer Pipeline

Globale Parameter können in jedem [Pipelineausdruck](control-flow-expression-language-functions.md) verwendet werden. Wenn eine Pipeline auf eine andere Ressource verweist, z. B. ein Dataset oder einen Datenfluss, können Sie den Wert des globalen Parameters über die Parameter dieser Ressource an die untergeordneten Elemente übergeben. Auf globale Parameter wird wie folgt verwiesen: `pipeline().globalParameters.<parameterName>`.

![Verwenden von globalen Parametern](media/author-global-parameters/expression-global-parameters.png)

## <a name="global-parameters-in-cicd"></a><a name="cicd"></a> Globale Parameter in CI/CD

Globale Parameter verfügen über einen eindeutigen CI/CD-Prozess, der relativ zu anderen Entitäten in Azure Data Factory erfolgt. Beim Veröffentlichen einer Factory oder Exportieren einer ARM-Vorlage mit globalen Parametern wird der Ordner *globalParameters* mit der Datei *Ihr_Factory-Name_GlobalParameters.json* erstellt. Bei dieser Datei handelt es sich um ein JSON-Objekt, das jeden globalen Parametertyp und -wert der veröffentlichten Factory enthält.

![Veröffentlichen von globalen Parametern](media/author-global-parameters/global-parameters-adf-publish.png)

Bei einer Bereitstellung in einer neuen Umgebung, z. B. vom Typ TEST oder PRODUKTION, empfehlen wir Ihnen die Erstellung einer Kopie dieser Datei mit den globalen Parametern und die Überschreibung der entsprechenden umgebungsspezifischen Werte. Beim erneuten Veröffentlichen wird die ursprüngliche Datei mit den globalen Parametern überschrieben, aber die Kopie für die andere Umgebung bleibt erhalten.

Wenn Sie beispielsweise über eine Factory mit dem Namen „ADF-DEV“ und einen globalen Parameter vom Typ „Zeichenfolge“ mit dem Namen „environment“ und dem Wert „dev“ verfügen, wird beim Veröffentlichen eine Datei mit dem Namen *ADF-DEV_GlobalParameters.json* generiert. Wenn Sie die Bereitstellung für eine Testfactory mit dem Namen „ADF_TEST“ durchführen, sollten Sie eine Kopie der JSON-Datei erstellen (z. B. mit dem Namen „ADF-TEST_GlobalParameters.json“) und die Parameterwerte durch die umgebungsspezifischen Werte ersetzen. Der Parameter „environment“ verfügt nun ggf. über den Wert „test“. 

![Bereitstellen von globalen Parametern](media/author-global-parameters/powershell-task.png)

Verwenden Sie das unten angegebene PowerShell-Skript, um globale Parameter für weitere Umgebungen bereitzustellen. Fügen Sie eine Azure PowerShell-DevOps-Aufgabe hinzu, bevor Sie die ARM-Vorlagenbereitstellung durchführen. In der DevOps-Aufgabe müssen Sie den Speicherort der neuen Parameterdatei, die Zielressourcengruppe und die Ziel-Data Factory angeben.

> [!NOTE]
> Zum Bereitstellen von globalen Parametern mit PowerShell müssen Sie mindestens Version 4.4.0 des Az-Moduls verwenden.

```powershell
param
(
    [parameter(Mandatory = $true)] [String] $globalParametersFilePath,
    [parameter(Mandatory = $true)] [String] $resourceGroupName,
    [parameter(Mandatory = $true)] [String] $dataFactoryName
)

Import-Module Az.DataFactory

$newGlobalParameters = New-Object 'system.collections.generic.dictionary[string,Microsoft.Azure.Management.DataFactory.Models.GlobalParameterSpecification]'

Write-Host "Getting global parameters JSON from: " $globalParametersFilePath
$globalParametersJson = Get-Content $globalParametersFilePath

Write-Host "Parsing JSON..."
$globalParametersObject = [Newtonsoft.Json.Linq.JObject]::Parse($globalParametersJson)

foreach ($gp in $globalParametersObject.GetEnumerator()) {
    Write-Host "Adding global parameter:" $gp.Key
    $globalParameterValue = $gp.Value.ToObject([Microsoft.Azure.Management.DataFactory.Models.GlobalParameterSpecification])
    $newGlobalParameters.Add($gp.Key, $globalParameterValue)
}

$dataFactory = Get-AzDataFactoryV2 -ResourceGroupName $resourceGroupName -Name $dataFactoryName
$dataFactory.GlobalParameters = $newGlobalParameters

Write-Host "Updating" $newGlobalParameters.Count "global parameters."

Set-AzDataFactoryV2 -InputObject $dataFactory -Force
```

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über den [CI/CD-Prozess (Continuous Integration/Deployment)](continuous-integration-deployment.md) von Azure Data Factory.
* Informieren Sie sich über die Nutzung der [Ausdruckssprache für die Ablaufsteuerung](control-flow-expression-language-functions.md).