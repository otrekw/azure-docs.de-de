---
title: Importieren eines neuen Updates | Microsoft-Dokumentation
description: Schrittanleitung zum Importieren eines neuen Updates in IoT Hub Device Update für IoT Hub.
author: andrewbrownmsft
ms.author: andbrown
ms.date: 2/11/2021
ms.topic: how-to
ms.service: iot-hub-device-update
ms.openlocfilehash: b9d40848abdd85beeca592001b697e3c50b7cd59
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103008561"
---
# <a name="import-new-update"></a>Importieren eines neuen Updates
Hier erfahren Sie, wie Sie ein neues Update in Device Update für IoT Hub importieren. Wenn Sie es noch nicht getan haben, sollten Sie sich mit den grundlegenden [Importkonzepten](import-concepts.md) vertraut machen.

## <a name="prerequisites"></a>Voraussetzungen

* [Zugriff auf einen IoT Hub mit aktiviertem Device Update for IoT Hub](create-device-update-account.md). Es wird empfohlen, dass Sie für Ihren IoT Hub einen S1-Tarif (Standard) oder höher verwenden. 
* Ein IoT-Gerät (oder Simulator), das/der für Device Update in IoT Hub bereitgestellt wird.
   * Wenn Sie ein echtes Gerät verwenden, benötigen Sie eine Updateimagedatei für ein Imageupdate oder eine [APT-Manifestdatei](device-update-apt-manifest.md) für ein Paketupdate.
* [PowerShell 5](https://docs.microsoft.com/powershell/scripting/install/installing-powershell) oder höher.
* Unterstützte Browser:
  * [Microsoft Edge](https://www.microsoft.com/edge)
  * Google Chrome

> [!NOTE]
> Einige an diesen Dienst übermittelte Daten werden möglicherweise in einer Region außerhalb der Region verarbeitet, in der diese Instanz erstellt wurde.

## <a name="create-device-update-import-manifest"></a>Erstellen eines Importmanifests für Geräteupdates

1. Stellen Sie sicher, dass sich Ihre Updateimagedatei oder APT-Manifestdatei in einem Verzeichnis befindet, auf das PowerShell zugreifen kann.

2. Erstellen Sie eine Textdatei namens **AduUpdate.psm1** im Verzeichnis, indem sich Ihre Updateimagedatei oder die APT-Manifestdatei befinden. Öffnen Sie dann das PowerShell-Cmdlet [AduUpdate.psm1](https://github.com/Azure/iot-hub-device-update/tree/main/tools/AduCmdlets), kopieren Sie den Inhalt in Ihre Textdatei, und speichern Sie dann die Textdatei.

3. Navigieren Sie in PowerShell zum Verzeichnis, indem Sie in Schritt 2 Ihr PowerShell-Cmdlet erstellt haben. Führen Sie dann Folgendes aus:

    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process
    Import-Module .\AduUpdate.psm1
    ```

4. Führen Sie die folgenden Befehle aus, indem Sie die Beispielpameterwerte ersetzen, um ein Importmanifest, d. h. eine JSON-Datei zur Beschreibung des Updates, zu generieren:
    ```powershell
    $compat = New-AduUpdateCompatibility -DeviceManufacturer 'deviceManufacturer' -DeviceModel 'deviceModel'

    $importManifest = New-AduImportManifest -Provider 'updateProvider' -Name 'updateName' -Version 'updateVersion' `
                                            -UpdateType 'updateType' -InstalledCriteria 'installedCriteria' `
                                            -Compatibility $compat -Files 'updateFilePath(s)'

    $importManifest | Out-File '.\importManifest.json' -Encoding UTF8
    ```

    Als Kurzreferenz: Hier sind einige Beispielwerte für die vorstehenden Parameter. Sie können sich auch das vollständige [Importmanifestschema](import-schema.md) ansehen, um mehr Details zu erhalten.

    | Parameter | BESCHREIBUNG |
    | --------- | ----------- |
    | deviceManufacturer | Hierbei handelt es sich um den Hersteller des Geräts, mit dem das Update kompatibel ist, z. B. Contoso. Dieser Parameter muss mit der _manufacturer_-[Geräteeigenschaft](https://docs.microsoft.com/azure/iot-hub-device-update/device-update-plug-and-play#device-properties) übereinstimmen.
    | deviceModel | Modell des Geräts, mit dem das Update kompatibel ist, z. B. „Toaster“. Dieser Parameter muss mit der _model_-[Geräteeigenschaft](https://docs.microsoft.com/azure/iot-hub-device-update/device-update-plug-and-play#device-properties) übereinstimmen.
    | updateProvider | Bei diesem Parameter handelt es sich um die Entität, die das Update erstellt oder verantwortlich für dieses ist. Oft ist dies ein Firmenname.
    | updateName | Hierbei handelt es sich um einen Bezeichner für eine Klasse von Updates. Die Klasse kann etwas von Ihnen Gewähltes sein. Oft ist es ein Geräte- oder Modellname.
    | updateVersion | Hierbei handelt es sich um die Versionsnummer, die dieses Update von anderen mit denselben Anbietern und Namen unterscheidet. Der Parameter entspricht nicht der Version einer einzelnen Softwarekomponente auf dem Gerät (kann jedoch ausgewählt werden).
    | updateType | <ul><li>`microsoft/swupdate:1` für Imageupdate angeben</li><li>`microsoft/apt:1` für Paketupdate angeben</li></ul>
    | installedCriteria | <ul><li>Wert von „SWVersion“ für Updatetyp `microsoft/swupdate:1` angeben</li><li>Geben Sie den empfohlenen Wert für den Updatetyp `microsoft/apt:1` an.
    | updateFilePath(s) | Pfad zu der/den Updatedatei(en) auf Ihrem Computer


## <a name="review-generated-import-manifest"></a>Überprüfen des generierten Importmanifests

Beispiel:
```json
{
  "updateId": {
    "provider": "Microsoft",
    "name": "Toaster",
    "version": "2.0"
  },
  "updateType": "microsoft/swupdate:1",
  "installedCriteria": "5",
  "compatibility": [
    {
      "deviceManufacturer": "Fabrikam",
      "deviceModel": "Toaster"
    },
    {
      "deviceManufacturer": "Contoso",
      "deviceModel": "Toaster"
    }
  ],
  "files": [
    {
      "filename": "file1.json",
      "sizeInBytes": 7,
      "hashes": {
        "sha256": "K2mn97qWmKSaSaM9SFdhC0QIEJ/wluXV7CoTlM8zMUo="
      }
    },
    {
      "filename": "file2.zip",
      "sizeInBytes": 11,
      "hashes": {
        "sha256": "gbG9pxCr9RMH2Pv57vBxKjm89uhUstD06wvQSioLMgU="
      }
    }
  ],
  "createdDateTime": "2020-10-08T03:32:52.477Z",
  "manifestVersion": "2.0"
}
```

## <a name="import-update"></a>Importieren des Updates

[!NOTE]
Die folgenden Anweisungen zeigen, wie Sie ein Update über die Benutzeroberfläche des Azure-Portals importieren. Sie können auch das [Geräteupdate für IoT Hub-APIs](https://github.com/Azure/iot-hub-device-update/tree/main/docs/publish-api-reference) verwenden, um ein Update zu importieren. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrer IoT Hub-Instanz mit Device Update.

2. Wählen Sie links auf der Seite unter „Automatische Geräteverwaltung“ die Option „Geräteupdates“ aus.

   :::image type="content" source="media/import-update/import-updates-3.png" alt-text="Importieren von Updates" lightbox="media/import-update/import-updates-3.png":::

3. Oben auf dem Bildschirm werden mehrere Registerkarten angezeigt. Wählen Sie die Registerkarte „Updates“ aus.

   :::image type="content" source="media/import-update/updates-tab.png" alt-text="Updates" lightbox="media/import-update/updates-tab.png":::

4. Wählen Sie unter der Überschrift „Ready to Deploy“ (Bereit für die Bereitstellung) die Option „+ Import New Update“ (+ Neues Update importieren) aus.

   :::image type="content" source="media/import-update/import-new-update-2.png" alt-text="Neues Update importieren" lightbox="media/import-update/import-new-update-2.png":::

5. Wählen Sie das Ordnersymbol oder das Textfeld unter „Select an Import Manifest File“ (Importmanifestdatei auswählen) aus. Daraufhin wird ein Dialogfeld für Dateiauswahl angezeigt. Wählen Sie das Importmanifest aus, das Sie zuvor mithilfe des PowerShell-Cmdlets erstellt haben. Wählen Sie als Nächstes das Ordnersymbol oder das Textfeld unter „Select one or more update files“ (Mindestens eine Updatedatei auswählen) aus. Daraufhin wird ein Dialogfeld für Dateiauswahl angezeigt. Wählen Sie Ihre Updatedatei(en) aus.

   :::image type="content" source="media/import-update/select-update-files.png" alt-text="Updatedateien auswählen" lightbox="media/import-update/select-update-files.png":::

6. Wählen Sie unter „Select a storage container“ (Speichercontainer auswählen) das Ordnersymbol oder das Textfeld aus. Wählen Sie dann das entsprechende Speicherkonto aus. Der Speichercontainer wird zum vorübergehenden Stagen der Updatedateien verwendet.

   :::image type="content" source="media/import-update/storage-account.png" alt-text="Speicherkonto" lightbox="media/import-update/storage-account.png":::

7. Wenn Sie bereits einen Container erstellt haben, können Sie ihn wiederverwenden. (Wählen Sie andernfalls „+ Container“ aus, um einen neuen Speichercontainer für Updates zu erstellen.)  Wählen Sie den Container aus, den Sie verwenden möchten, und klicken Sie auf „Select“ (Auswählen).

   :::image type="content" source="media/import-update/container.png" alt-text="„Container“ auswählen" lightbox="media/import-update/container.png":::

8. Wählen Sie „Submit“ (Übermitteln) aus, um den Importvorgang zu starten.

   :::image type="content" source="media/import-update/publish-update.png" alt-text="Update veröffentlichen" lightbox="media/import-update/publish-update.png":::

9. Der Importvorgang beginnt, und auf dem Bildschirm wird der Abschnitt „Import History“ (Importverlauf) angezeigt. Wählen Sie „Refresh“ (Aktualisieren) aus, um den Fortschritt anzuzeigen, bis der Importvorgang abgeschlossen ist (je nach Größe des Updates kann dieser Vorgang innerhalb weniger Minuten abgeschlossen sein, könnte aber auch länger dauern).

   :::image type="content" source="media/import-update/update-publishing-sequence-2.png" alt-text="Updateimportsequenzierung" lightbox="media/import-update/update-publishing-sequence-2.png":::

10. Wenn in der Spalte „Status“ angezeigt wird, dass der Import erfolgreich war, wählen Sie den Header „Ready to Deploy“ (Bereit für die Bereitstellung) aus. Ihr importiertes Update sollte jetzt in der Liste angezeigt werden.

   :::image type="content" source="media/import-update/update-ready.png" alt-text="Auftragsstatus" lightbox="media/import-update/update-ready.png":::

## <a name="next-steps"></a>Nächste Schritte

[Erstellen Sie Gruppen.](create-update-group.md)

[Informieren Sie sich über Konzepte zum Importieren.](import-concepts.md)
