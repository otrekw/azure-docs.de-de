---
title: Shared Access Signature-URI für VM-Images – Azure Marketplace
description: Generieren Sie im Azure Marketplace einen SAS-URI (Shared Access Signature) für Ihre virtuellen Festplatten (VHDs).
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 07/29/2020
ms.openlocfilehash: 2bc129fc37347bd108ad62409490c5ce31b7728f
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87538930"
---
# <a name="get-shared-access-signature-uri-for-your-vm-image"></a>Abrufen des Shared Access Signature-URI für Ihr VM-Image

In diesem Artikel wird beschrieben, wie Sie einen SAS-URI (Shared Access Signature Uniform Resource Identifier) für die einzelnen virtuellen Festplatten (VHDs) generieren.

Während des Veröffentlichungsvorgangs müssen Sie einen URI für jede VHD angeben, die Ihren Plänen (zuvor als SKUs bezeichnet) zugeordnet ist. Microsoft benötigt während des Zertifizierungsprozesses Zugriff auf diese VHDs. Sie geben den URI im Partner Center auf der Registerkarte **Pläne** ein.

Stellen Sie beim Generieren von SAS-URIs für Ihre VHDs sicher, dass die folgenden Anforderungen erfüllt sind:

* Es werden nur nicht verwaltete virtuelle Festplatten unterstützt.
* Nur die Berechtigungen `List` und `Read` sind erforderlich. Gewähren Sie keinen Schreib- oder Löschzugriff.
* Die Zugriffsdauer (Ablaufdatum) sollte mindestens drei Wochen betragen, gerechnet ab dem Erstellungsdatum des SAS-URI.
* Um Fehler aufgrund von UTC-Zeitabweichungen zu vermeiden, sollten Sie das Startdatum auf einen Tag vor dem aktuellen Datum festlegen. Wählen Sie beispielsweise 05.10.2019 aus, wenn das aktuelle Datum der 6. Oktober 2019 ist.

## <a name="generate-the-sas-address"></a>Generieren der SAS-Adresse

Es gibt zwei Tools, die häufig zum Erstellen einer SAS-Adresse (URL) verwendet werden:

* **Microsoft Azure Storage-Explorer**: Im Azure-Portal verfügbares grafisches Tool.
* **Microsoft Azure CLI**: Empfohlen für andere Betriebssysteme als Windows und für automatisierte oder Continuous Integration-Umgebungen.

### <a name="use-microsoft-azure-storage-explorer"></a>Verwenden von Microsoft Azure Storage-Explorer

1. Wechseln Sie im Azure-Portal zu Ihrem Speicherkonto.
2. Öffnen Sie im Explorer-Bereich auf der linken Seite das Tool **Storage-Explorer** (Vorschau).
3. Klicken Sie mit der rechten Maustaste auf Ihre virtuelle Festplatte, und wählen Sie **Shared Access Signature abrufen** aus.
4. Das Dialogfeld **Shared Access Signature** wird angezeigt. Füllen Sie die folgenden Felder aus:

    * **Startzeit**: Das Startdatum der Berechtigung für den Zugriff auf die virtuelle Festplatte. Geben Sie ein Datum an, das einen Tag vor dem aktuellen Datum liegt.
    * **Ablaufzeit**: Das Ablaufdatum der Berechtigung für den Zugriff auf die virtuelle Festplatte. Geben Sie ein Datum an, das mindestens drei Wochen nach dem aktuellen Datum liegt.
    * **Berechtigungen**: Wählen Sie die Berechtigungen für das Lesen und Auflisten aus.
    * **Containerebene**: Aktivieren Sie das Kontrollkästchen **URI für Shared Access Signature auf Containerebene generieren**.

        :::image type="content" source="media/create-sas-uri-storage-explorer.png" alt-text="Darstellung des Dialogfelds „Shared Access Signature“":::

5. Wählen Sie **Erstellen** aus, um den zugehörigen SAS-URI für diese VHD zu erstellen. Das Dialogfeld wird aktualisiert und enthält danach Details zu diesem Vorgang.
6. Kopieren Sie den **URI**, und speichern Sie ihn in einer Textdatei an einem sicheren Ort.

    :::image type="content" source="media/create-sas-uri-shared-access-signature-details.png" alt-text="Darstellung des Dialogfelds mit den Details zu „Shared Access Signature“":::
7. Wiederholen Sie diese Schritte für jede VHD in den Plänen, die Sie veröffentlichen möchten.

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

1. Laden Sie die [Microsoft Azure CLI](https://azure.microsoft.com/documentation/articles/xplat-cli-install/) herunter, und installieren Sie sie. Es stehen Versionen für Windows, macOS und verschiedene Linux-Distributionen zur Verfügung.
2. Erstellen Sie eine PowerShell-Datei (Dateierweiterung PS1), kopieren Sie den folgenden Code in die Datei, und speichern Sie sie lokal.

    ```PowerShell
    az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' --name <vhd-name> --permissions rl --start '<start-date>' --expiry '<expiry-date>'
    ```

3. Bearbeiten Sie die Datei, um die folgenden Parameterwerte zu verwenden. Verwenden Sie für Datumsangaben das UTC-Format für Datum und Uhrzeit, z. B. `2020-04-01T00:00:00Z`.

    * `<account-name>`: Der Name Ihres Azure Storage-Kontos.
    * `<account-key>`: Der Schlüssel Ihres Azure Storage-Kontos.
    * `<vhd-name>`: Der Name Ihrer virtuellen Festplatte.
    * `<start-date>`: Das Startdatum der Berechtigung für den Zugriff auf die virtuelle Festplatte. Geben Sie ein Datum an, das einen Tag vor dem aktuellen Datum liegt.
    * `<expiry-date>`: Das Ablaufdatum der Berechtigung für den Zugriff auf die virtuelle Festplatte. Geben Sie ein Datum an, das mindestens drei Wochen nach dem aktuellen Datum liegt.

    In diesem Beispiel werden richtige Parameterwerte angezeigt (zum Zeitpunkt der Erstellung dieses Dokuments):

    ```PowerShell
    az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=st00009;AccountKey=6L7OWFrlabs7Jn23OaR3rvY5RykpLCNHJhxsbn9ONc+bkCq9z/VNUPNYZRKoEV1FXSrvhqq3aMIDI7N3bSSvPg==;EndpointSuffix=core.windows.net' --name vhds --permissions rl --start '2020-04-01T00:00:00Z' --expiry '2021-04-01T00:00:00Z'
    ```

4. Speichern Sie die Änderungen.
5. Führen Sie dieses Skript mit einem der folgenden Verfahren mit Administratorrechten aus, um eine **SAS-Verbindungszeichenfolge** für den Zugriff auf Containerebene zu erstellen:

    * Führen Sie das Skript an der Konsole aus. Klicken Sie unter Windows mit der rechten Maustaste auf das Skript, und wählen Sie **Als Administrator ausführen** aus.
    * Führen Sie das Skript mit einem PowerShell-Skript-Editor aus, z. B. [Windows PowerShell ISE](https://docs.microsoft.com/powershell/scripting/components/ise/introducing-the-windows-powershell-ise). Auf diesem Bildschirm wird die Erstellung einer SAS-Verbindungszeichenfolge in diesem Editor angezeigt:

     :::image type="content" source="media/create-sas-uri-power-shell-ise.png" alt-text="Darstellung der Erstellung einer SAS-Verbindungszeichenfolge per Windows PowerShell ISE":::

6. Kopieren Sie die SAS-Verbindungszeichenfolge, und speichern Sie sie in einer Textdatei an einem sicheren Ort. Bearbeiten Sie diese Zeichenfolge, und fügen Sie die Informationen zum Speicherort der virtuellen Festplatte hinzu, um den endgültigen SAS-URI zu erstellen.
7. Navigieren Sie im Azure-Portal zu dem Blobspeicher, der die VHD mit dem neuen URI enthält.
8. Kopieren Sie die URL des **Blob-Dienstendpunkts**. Dies ist im folgenden Screenshot dargestellt.

    :::image type="content" source="media/create-sas-uri-blob-endpoint.png" alt-text="Darstellung des Blob-Dienstendpunkts":::

9. Bearbeiten Sie die Textdatei mit der SAS-Verbindungszeichenfolge aus Schritt 6. Erstellen Sie den gesamten SAS-URI im folgenden Format:

    `<blob-service-endpoint-url> + /vhds/ + <vhd-name>? + <sas-connection-string>`

    Wenn der Name der VHD beispielsweise `TestRGVM2.vhd` lautet, ergibt sich der folgende SAS-URI:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

Wiederholen Sie diese Schritte für jede VHD in den Plänen, die Sie veröffentlichen möchten.

## <a name="verify-the-sas-uri"></a>Überprüfen des SAS-URI

Überprüfen Sie jeden erstellten SAS-URI, indem Sie anhand der Prüfliste Folgendes sicherstellen:

* Der URI hat das folgende Format: `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`
* Der URI enthält den Namen Ihres VHD-Images, einschließlich der Dateierweiterung „.vhd“.
* `sp=rl` wird ungefähr in der Mitte Ihres URI angezeigt. Mit dieser Zeichenfolge wird angegeben, dass die Zugriffsberechtigungen `Read` und `List` gewährt werden.
* `sr=c` steht für Zugriff auf Containerebene.
* Kopieren Sie den URI, und fügen Sie ihn in einen Browser ein, um das Blob testweise herunterzuladen. (Sie können den Vorgang vor Abschluss des Downloads abbrechen.)

## <a name="next-step"></a>Nächster Schritt

Falls Sie Schwierigkeiten beim Erstellen eines SAS-URI haben, helfen Ihnen die Informationen unter [Allgemeine Probleme mit der SAS-URL](common-sas-uri-issues.md) weiter. Andernfalls speichern Sie den SAS-URI in einem sicheren Speicherort zur späteren Verwendung. Sie benötigen ihn zum Veröffentlichen Ihres VM-Angebots in Partner Center.

* [Erstellen eines Angebots für virtuelle Azure-Computer](azure-vm-create-offer.md)
