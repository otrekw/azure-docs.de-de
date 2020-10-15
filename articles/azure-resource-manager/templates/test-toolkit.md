---
title: Resource Manager-Vorlagen-Testtoolkit
description: Hier wird beschrieben, wie Sie das Resource Manager-Vorlagen-Testtoolkit für Ihre Vorlage ausführen. Mit dem Toolkit können Sie festzustellen, ob Sie die empfohlenen Vorgehensweisen implementiert haben.
ms.topic: conceptual
ms.date: 09/02/2020
ms.author: tomfitz
author: tfitzmac
ms.openlocfilehash: 73f6db8cbd5e4d7a0670c394f6af338aae8e9e79
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89439559"
---
# <a name="use-arm-template-test-toolkit"></a>Verwenden des Resource Manager-Vorlagen-Testtoolkits

Das [ARM-Vorlagen-Testtoolkit](https://aka.ms/arm-ttk) (Azure Resource Manager) überprüft, ob Ihre Vorlage die empfohlenen Vorgehensweisen verwendet. Wenn Ihre Vorlage nicht mit den empfohlenen Vorgehensweisen kompatibel ist, wird eine Liste mit Warnungen ausgegeben, die vorgeschlagene Änderungen enthält. Mit dem Testtoolkit erfahren Sie, wie Sie häufige Probleme bei der Vorlagenentwicklung vermeiden.

Das Testtoolkit stellt einen [Satz von Standardtests](test-cases.md) bereit. Diese Tests werden empfohlen, sind aber nicht erforderlich. Sie können entscheiden, welche Tests für Ihre Ziele relevant sind, und anpassen, welche Tests ausgeführt werden.

In diesem Artikel wird beschrieben, wie Sie das Testtoolkit ausführen und Tests hinzufügen oder entfernen. Beschreibungen der Standardtests finden Sie unter [Toolkit-Testfälle](test-cases.md).

Das Toolkit ist ein Satz von PowerShell-Skripts, die über einen Befehl in PowerShell oder in der Befehlszeilenschnittstelle ausgeführt werden können.

## <a name="install-on-windows"></a>Installieren unter Windows

1. Wenn Sie noch nicht über PowerShell verfügen, [installieren Sie PowerShell unter Windows](/powershell/scripting/install/installing-powershell-core-on-windows).

1. [Laden Sie die neueste ZIP-Datei](https://aka.ms/arm-ttk-latest) für das Testtoolkit herunter, und extrahieren Sie sie.

1. Starten Sie PowerShell.

1. Navigieren Sie zu dem Ordner, in dem Sie das Testtoolkit extrahiert haben. Navigieren Sie in diesem Ordner zum Ordner **arm-ttk**.

1. Wenn die [Ausführungsrichtlinie](/powershell/module/microsoft.powershell.core/about/about_execution_policies) Skripts aus dem Internet blockiert, müssen Sie die Blockierung der Skriptdateien deaktivieren. Vergewissern Sie sich, dass Sie sich im Ordner **arm-ttk** befinden.

   ```powershell
   Get-ChildItem *.ps1, *.psd1, *.ps1xml, *.psm1 -Recurse | Unblock-File
   ```

1. Importieren Sie das Modul.

   ```powershell
   Import-Module .\arm-ttk.psd1
   ```

1. Verwenden Sie den folgenden Befehl, um die Tests auszuführen:

   ```powershell
   Test-AzTemplate -TemplatePath \path\to\template
   ```

## <a name="install-on-linux"></a>Installation unter Linux

1. Wenn Sie noch nicht über PowerShell verfügen, [installieren Sie PowerShell unter Linux](/powershell/scripting/install/installing-powershell-core-on-linux).

1. [Laden Sie die neueste ZIP-Datei](https://aka.ms/arm-ttk-latest) für das Testtoolkit herunter, und extrahieren Sie sie.

1. Starten Sie PowerShell.

   ```bash
   pwsh
   ```

1. Navigieren Sie zu dem Ordner, in dem Sie das Testtoolkit extrahiert haben. Navigieren Sie in diesem Ordner zum Ordner **arm-ttk**.

1. Wenn die [Ausführungsrichtlinie](/powershell/module/microsoft.powershell.core/about/about_execution_policies) Skripts aus dem Internet blockiert, müssen Sie die Blockierung der Skriptdateien deaktivieren. Vergewissern Sie sich, dass Sie sich im Ordner **arm-ttk** befinden.

   ```powershell
   Get-ChildItem *.ps1, *.psd1, *.ps1xml, *.psm1 -Recurse | Unblock-File
   ```

1. Importieren Sie das Modul.

   ```powershell
   Import-Module ./arm-ttk.psd1
   ```

1. Verwenden Sie den folgenden Befehl, um die Tests auszuführen:

   ```powershell
   Test-AzTemplate -TemplatePath /path/to/template
   ```

## <a name="install-on-macos"></a>Installieren unter macOS

1. Wenn Sie noch nicht über PowerShell verfügen, [installieren Sie PowerShell unter macOS](/powershell/scripting/install/installing-powershell-core-on-macos).

1. Installieren Sie `coreutils`:

   ```bash
   brew install coreutils
   ```

1. [Laden Sie die neueste ZIP-Datei](https://aka.ms/arm-ttk-latest) für das Testtoolkit herunter, und extrahieren Sie sie.

1. Starten Sie PowerShell.

   ```bash
   pwsh
   ```

1. Navigieren Sie zu dem Ordner, in dem Sie das Testtoolkit extrahiert haben. Navigieren Sie in diesem Ordner zum Ordner **arm-ttk**.

1. Wenn die [Ausführungsrichtlinie](/powershell/module/microsoft.powershell.core/about/about_execution_policies) Skripts aus dem Internet blockiert, müssen Sie die Blockierung der Skriptdateien deaktivieren. Vergewissern Sie sich, dass Sie sich im Ordner **arm-ttk** befinden.

   ```powershell
   Get-ChildItem *.ps1, *.psd1, *.ps1xml, *.psm1 -Recurse | Unblock-File
   ```

1. Importieren Sie das Modul.

   ```powershell
   Import-Module ./arm-ttk.psd1
   ```

1. Verwenden Sie den folgenden Befehl, um die Tests auszuführen:

   ```powershell
   Test-AzTemplate -TemplatePath /path/to/template
   ```

## <a name="result-format"></a>Ergebnisformat

Bestandene Tests werden in **Grün** mit vorangestelltem **[+]** angezeigt.

Fehlerhafte Tests werden in **Rot** mit vorangestelltem **[-]** angezeigt.

:::image type="content" source="./media/template-test-toolkit/view-results.png" alt-text="Anzeigen von Testergebnissen":::

Die Testergebnisse sind:

```powershell
[+] adminUsername Should Not Be A Literal (24 ms)
[+] apiVersions Should Be Recent (18 ms)
[+] artifacts parameter (16 ms)
[+] DeploymentTemplate Schema Is Correct (17 ms)
[+] IDs Should Be Derived From ResourceIDs (15 ms)
[-] Location Should Not Be Hardcoded (41 ms)
     azuredeploy.json must use the location parameter, not resourceGroup().location (except when used as a default value in the main template)
```

## <a name="test-parameters"></a>Testparameter

Wenn Sie den Parameter **-TemplatePath** angeben, sucht das Toolkit in diesem Ordner nach einer Vorlage mit dem Namen „azuredeploy.json“ oder „maintemplate.json“. Es testet zunächst diese Vorlage und anschließend alle anderen Vorlagen im Ordner und seinen Unterordnern. Die anderen Vorlagen werden als verknüpfte Vorlagen getestet. Wenn ihr Pfad eine Datei mit dem Namen [CreateUiDefinition.json](../managed-applications/create-uidefinition-overview.md) enthält, werden Tests ausgeführt, die für die Benutzeroberflächendefinition relevant sind.

```powershell
Test-AzTemplate -TemplatePath $TemplateFolder
```

Fügen Sie den Parameter **-File** hinzu, um eine Datei in diesem Ordner zu testen. Der Ordner muss jedoch weiterhin über eine Hauptvorlage mit dem Namen „azuredeploy.json“ oder „maintemplate.json“ verfügen.

```powershell
Test-AzTemplate -TemplatePath $TemplateFolder -File cdn.json
```

Standardmäßig werden alle Tests ausgeführt. Verwenden Sie den Parameter **-Test**, um einzelne Tests anzugeben, die ausgeführt werden sollen. Geben Sie den Namen des Tests an. Die Namen finden Sie unter [Testfälle für das Toolkit](test-cases.md).

```powershell
Test-AzTemplate -TemplatePath $TemplateFolder -Test "Resources Should Have Location"
```

## <a name="customize-tests"></a>Anpassen der Tests

Für Resource Manager-Vorlagen führt das Toolkit alle Tests im Ordner **\arm-ttk\testcases\deploymentTemplate** aus. Wenn Sie einen Test dauerhaft entfernen möchten, löschen Sie die Datei aus dem Ordner.

Für [CreateUiDefinition](../managed-applications/create-uidefinition-overview.md)-Dateien werden alle Tests im Ordner **\arm-ttk\testcases\CreateUiDefinition** ausgeführt.

Wenn Sie einen eigenen Test hinzufügen möchten, erstellen Sie eine Datei mit der Namenskonvention: **Your-Custom-Test-Name.test.ps1**.

Der Test kann die Vorlage als Objektparameter oder Zeichenfolgenparameter erhalten. In der Regel können Sie nur das eine oder das andere, nicht aber beides verwenden.

Verwenden Sie Objektparameter, wenn Sie die Eigenschaften eines Abschnitts der Vorlage durchgehen müssen. Ein Test, der den Objektparameter verwendet, weist das folgende Format auf:

```powershell
param(
    [Parameter(Mandatory=$true,Position=0)]
    [PSObject]
    $TemplateObject
)

# Implement test logic that evaluates parts of the template.
# Output error with: Write-Error -Message
```

Das Vorlagenobjekt weist die folgenden Eigenschaften auf:

* $schema
* contentVersion
* parameters
* variables
* ressourcen
* outputs

Sie können beispielsweise eine Auflistung von Parametern mithilfe von `$TemplateObject.parameters` abrufen.

Verwenden Sie Zeichenfolgenparameter, wenn Sie einen Zeichenfolgenvorgang für die gesamte Vorlage ausführen müssen. Ein Test, der den Zeichenfolgenparameter verwendet, weist das folgende Format auf:

```powershell
param(
    [Parameter(Mandatory)]
    [string]
    $TemplateText
)

# Implement test logic that performs string operations.
# Output error with: Write-Error -Message
```

Sie können beispielsweise einen regulären Ausdruck des Zeichenfolgenparameters ausführen, um festzustellen, ob eine bestimmte Syntax verwendet wird.

Wenn Sie mehr über das Implementieren des Tests erfahren möchten, sehen Sie sich die anderen Tests in diesem Ordner an.

## <a name="integrate-with-azure-pipelines"></a>Integration in Azure Pipelines

Sie können das Testtoolkit zu Ihrer Azure Pipeline hinzufügen. Mit einer Pipeline können Sie den Test bei jeder Aktualisierung der Vorlage oder als Teil des Bereitstellungsprozesses ausführen.

Die einfachste Möglichkeit zum Hinzufügen des Testtoolkits zu Ihrer Pipeline besteht in Erweiterungen von Drittanbietern. Hierfür sind die folgenden beiden Erweiterungen verfügbar:

* [Run ARM TTK Tests](https://marketplace.visualstudio.com/items?itemName=Sam-Cogan.ARMTTKExtension)
* [ARM Template Tester](https://marketplace.visualstudio.com/items?itemName=maikvandergaag.maikvandergaag-arm-ttk)

Sie können auch eigene Aufgaben implementieren. Das folgende Beispiel zeigt, wie Sie das Testtoolkit herunterladen.

```json
{
    "environment": {},
    "enabled": true,
    "continueOnError": false,
    "alwaysRun": false,
    "displayName": "Download TTK",
    "timeoutInMinutes": 0,
    "condition": "succeeded()",
    "task": {
        "id": "e213ff0f-5d5c-4791-802d-52ea3e7be1f1",
        "versionSpec": "2.*",
        "definitionType": "task"
    },
    "inputs": {
        "targetType": "inline",
        "filePath": "",
        "arguments": "",
        "script": "New-Item '$(ttk.folder)' -ItemType Directory\nInvoke-WebRequest -uri '$(ttk.uri)' -OutFile \"$(ttk.folder)/$(ttk.asset.filename)\" -Verbose\nGet-ChildItem '$(ttk.folder)' -Recurse\n\nWrite-Host \"Expanding files...\"\nExpand-Archive -Path '$(ttk.folder)/*.zip' -DestinationPath '$(ttk.folder)' -Verbose\n\nWrite-Host \"Expanded files found:\"\nGet-ChildItem '$(ttk.folder)' -Recurse",
        "errorActionPreference": "stop",
        "failOnStderr": "false",
        "ignoreLASTEXITCODE": "false",
        "pwsh": "true",
        "workingDirectory": ""
    }
}
```

Im nächsten Beispiel sehen Sie, wie die Tests ausgeführt werden.

```json
{
    "environment": {},
    "enabled": true,
    "continueOnError": true,
    "alwaysRun": false,
    "displayName": "Run Best Practices Tests",
    "timeoutInMinutes": 0,
    "condition": "succeeded()",
    "task": {
        "id": "e213ff0f-5d5c-4791-802d-52ea3e7be1f1",
        "versionSpec": "2.*",
        "definitionType": "task"
    },
    "inputs": {
        "targetType": "inline",
        "filePath": "",
        "arguments": "",
        "script": "Import-Module $(ttk.folder)/arm-ttk/arm-ttk.psd1 -Verbose\n$testOutput = @(Test-AzTemplate -TemplatePath \"$(sample.folder)\")\n$testOutput\n\nif ($testOutput | ? {$_.Errors }) {\n   exit 1 \n} else {\n    Write-Host \"##vso[task.setvariable variable=result.best.practice]$true\"\n    exit 0\n} \n",
        "errorActionPreference": "continue",
        "failOnStderr": "true",
        "ignoreLASTEXITCODE": "false",
        "pwsh": "true",
        "workingDirectory": ""
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Standardtests finden Sie unter [Testfälle für das Toolkit](test-cases.md).
