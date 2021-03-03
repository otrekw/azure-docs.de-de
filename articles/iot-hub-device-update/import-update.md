---
title: Importieren eines neuen Updates | Microsoft-Dokumentation
description: Schrittanleitung zum Importieren eines neuen Updates in IoT Hub Device Update für IoT Hub.
author: andbrown
ms.author: andbrown
ms.date: 2/11/2021
ms.topic: how-to
ms.service: iot-hub-device-update
ms.openlocfilehash: d8757f3076f784576f95bbdfc30abf578446c776
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660648"
---
# <a name="import-new-update"></a>Importieren eines neuen Updates
Hier erfahren Sie, wie Sie ein neues Update in Device Update für IoT Hub importieren.

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

2. Klonen Sie das [Device Update für IoT Hub-Repository](https://github.com/azure/iot-hub-device-update), oder laden Sie es als ZIP-Datei an einen Speicherort herunter, auf den Sie über PowerShell zugreifen können (klicken Sie nach dem Herunterladen der ZIP-Datei mit der rechten Maustaste auf die Registerkarte `Properties` > `General`, und aktivieren Sie `Unblock` im Abschnitt `Security`, um Eingabeaufforderungen bei PowerShell-Sicherheitswarnungen zu vermeiden).

3. Navigieren Sie in PowerShell zum Verzeichnis `tools/AduCmdlets`, und führen Sie Folgendes aus:

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

    Als Kurzreferenz: Hier sind einige Beispielwerte für die vorstehenden Parameter. Eine vollständige Dokumentation finden Sie unten im Schema des vollständigen Importmanifests.

    | Parameter | BESCHREIBUNG |
    | --------- | ----------- |
    | deviceManufacturer | Hersteller des Geräts, mit dem das Update kompatibel ist, z. B. „Contoso“
    | deviceModel | Modell des Geräts, mit dem das Update kompatibel ist, z. B. „Toaster“
    | updateProvider | Anbieterteil der Update-Identität, z. B. „Fabrikam“
    | updateName | Namensteil der Update-Identität, z. B. „imageUpdate“
    | updateVersion | Updateversion, z. B. „2.0“
    | updateType | <ul><li>`microsoft/swupdate:1` für Imageupdate angeben</li><li>`microsoft/apt:1` für Paketupdate angeben</li></ul>
    | installedCriteria | <ul><li>Wert von „SWVersion“ für Updatetyp `microsoft/swupdate:1` angeben</li><li>Geben Sie den empfohlenen Wert für den Updatetyp `microsoft/apt:1` an.
    | updateFilePath(s) | Pfad zu der/den Updatedatei(en) auf Ihrem Computer

    Schema des vollständigen Importmanifests

    | Name | type | BESCHREIBUNG | Beschränkungen |
    | --------- | --------- | --------- | --------- |
    | UpdateId | `UpdateId`-Objekt | Update-Identität. |
    | UpdateType | Zeichenfolge | Updatetyp: <ul><li>Geben `microsoft/apt:1` Sie an, wenn Sie ein paketbasiertes Update mit dem Referenz-Agent ausführen.</li><li>Geben `microsoft/swupdate:1` Sie an, wenn Sie ein imagebasiertes Update mit dem Referenz-Agent ausführen.</li><li>Geben `microsoft/simulator:1` Sie an, wenn Sie den Beispiel-Agent-Simulator verwenden.</li><li>Geben Sie einen benutzerdefinierten Typ an, wenn Sie einen benutzerdefinierten Agent entwickeln.</li></ul> | <ul><li>Format: `{provider}/{type}:{typeVersion}`</li><li>Insgesamt maximal 32 Zeichen</li></ul> |
    | InstalledCriteria | Zeichenfolge | Vom Agent interpretierte Zeichenfolge zur Ermittlung, ob das Update erfolgreich angewendet wurde:  <ul><li>Geben Sie den **Wert** von „SWVersion“ für den Updatetyp `microsoft/swupdate:1` an.</li><li>Geben Sie `{name}-{version}` für den Updatetyp `microsoft/apt:1` an, dessen Name und Version aus der APT-Datei abgerufen werden.</li><li>Geben Sie den Hash der Updatedatei für den Updatetyp `microsoft/simulator:1` an.</li><li>Geben Sie eine benutzerdefinierte Zeichenfolge an, wenn Sie einen benutzerdefinierten Agent entwickeln.</li></ul> | Maximal 64 Zeichen |
    | Kompatibilität | Array von `CompatibilityInfo`-Objekten | Kompatibilitätsinformationen des Geräts, das mit diesem Update kompatibel ist. | Maximal 10 Elemente |
    | CreatedDateTime | date/time | Datum und Uhrzeit, an dem/zu der das Update erstellt wurde. | Durch Trennzeichen getrenntes ISO 8601-Datums- und Uhrzeitformat, in UTC |
    | ManifestVersion | Zeichenfolge | Schemaversion des Importmanifests. Geben Sie den Wert `2.0` an, der mit der Schnittstelle `urn:azureiot:AzureDeviceUpdateCore:1` und der Schnittstelle `urn:azureiot:AzureDeviceUpdateCore:4` kompatibel ist.</li></ul> | Muss gleich `2.0` sein. |
    | Dateien | Array von `File`-Objekten | Aktualisieren von Nutzlastdateien | Maximal 5 Dateien |

Hinweis: Alle Felder sind erforderlich.

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
