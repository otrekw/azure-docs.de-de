---
title: Abrufen eines SAS-URIs für Ihr VM-Image – Azure Marketplace
description: Generieren Sie im Azure Marketplace einen SAS-URI (Shared Access Signature) für Ihre virtuellen Festplatten (VHDs).
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 08/14/2020
ms.openlocfilehash: a84f287c6303e093d68dd462ccc5cecc34b463cd
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2020
ms.locfileid: "89144505"
---
# <a name="get-a-sas-uri-for-your-vm-image"></a>Abrufen eines SAS-URIs für Ihr VM-Image

Während des Veröffentlichungsvorgangs müssen Sie einen SAS (Shared Access Signature)-URI für jede VHD angeben, die Ihren Plänen (zuvor als SKUs bezeichnet) zugeordnet ist. Microsoft benötigt während des Zertifizierungsprozesses Zugriff auf diese VHDs. Sie geben den URI im Partner Center auf der Registerkarte **Pläne** ein.

Für das Generieren von SAS-URIs für Ihre VHDs gelten folgende Anforderungen:

- Sie unterstützen nur nicht verwaltete VHDs.
- Es sind nur Auflistungs- und Leseberechtigungen erforderlich. Gewähren Sie keinen Schreib- oder Löschzugriff.
- Die Zugriffsdauer (Ablaufdatum) sollte mindestens drei Wochen betragen, gerechnet ab dem Erstellungsdatum des SAS-URI.
- Um Fehler aufgrund von UTC-Zeitabweichungen zu vermeiden, sollten Sie das Startdatum auf einen Tag vor dem aktuellen Datum festlegen. Wählen Sie beispielsweise den 15.6.2020 aus, wenn das aktuelle Datum der 16. Juni 2020 ist.

## <a name="generate-the-sas-address"></a>Generieren der SAS-Adresse

Es gibt zwei Tools, die häufig zum Erstellen einer SAS-Adresse (URL) verwendet werden:

1. **Microsoft Azure Storage-Explorer**: Grafisches Tool für Windows, macOS und Linux.
2. **Microsoft Azure CLI**: Empfohlen für andere Betriebssysteme als Windows und für automatisierte oder Continuous Integration-Umgebungen.

### <a name="using-tool-1-microsoft-storage-explorer"></a>Tool 1: Microsoft Storage-Explorer

1. Laden Sie den [Microsoft Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) herunter, und installieren Sie ihn.
2. Öffnen Sie den Explorer, und wählen Sie im Menü auf der linken Seite die Option **Konto hinzufügen** aus.
3. Wählen Sie im Dialogfeld **Verbindung mit Azure Storage herstellen** die Option **Azure-Konto hinzufügen** aus, und melden Sie sich bei Ihrem Azure-Konto an.
4. Erweitern Sie im linken Bereich des Explorers den Knoten **Speicherkonten**.
5. Klicken Sie mit der rechten Maustaste auf Ihre virtuelle Festplatte, und wählen Sie **Shared Access Signature abrufen** aus.
6. Füllen Sie im Dialogfeld **Shared Access Signature** die folgenden Felder aus:

    1. Startzeit: Das Startdatum der Berechtigung für den Zugriff auf die virtuelle Festplatte. Geben Sie ein Datum an, das einen Tag vor dem aktuellen Datum liegt.
    2. Ablaufzeit: Das Ablaufdatum der Berechtigung für den Zugriff auf die virtuelle Festplatte. Geben Sie ein Datum an, das mindestens drei Wochen nach dem aktuellen Datum liegt.
    3. Berechtigungen: Wählen Sie die Berechtigungen für das Lesen und Auflisten aus.
    4. Containerebene: Aktivieren Sie das Kontrollkästchen URI für Shared Access Signature auf Containerebene generieren.

    ![Dialogfeld „Shared Access Signature“](media/vm/create-sas-uri-storage-explorer.png)

7. Wählen Sie **Erstellen** aus, um den zugehörigen SAS-URI für diese VHD zu erstellen. Das Dialogfeld wird aktualisiert und enthält danach Details zu diesem Vorgang.

8. Kopieren Sie den URI, und speichern Sie ihn in einer Textdatei an einem sicheren Ort.

    ![Kopieren des URIs](media/vm/create-sas-uri-shared-access-signature-details.png)

    Damit wurde der SAS-URI für den Zugriff auf Containerebene generiert. Bearbeiten Sie die Textdatei, und fügen Sie den Namen der VHD hinzu, um dies spezifisch anzugeben.

9. Fügen Sie den Namen Ihrer virtuellen Festplatte nach der Zeichenfolge „vhds“ in den SAS-URI ein (einschließlich Schrägstrich). Der endgültige SAS-URI sollte wie folgt aussehen:

    `<blob-service-endpoint-url> + /vhds/ + <vhd-name>? + <sas-connection-string>`

1. Wiederholen Sie diese Schritte für jede VHD in den Plänen, die Sie veröffentlichen möchten.

### <a name="using-tool-2-azure-cli"></a>Tool 2: Azure CLI

1. Laden Sie die [Microsoft Azure CLI](https://azure.microsoft.com/documentation/articles/xplat-cli-install/) herunter, und installieren Sie diese. Es stehen Versionen für Windows, macOS und verschiedene Linux-Distributionen zur Verfügung.
2. Erstellen Sie eine PowerShell-Datei (Dateierweiterung PS1), kopieren Sie den folgenden Code in die Datei, und speichern Sie sie lokal.

    ```JSON
    az storage container generate-sas --connection-string ‘DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net’ --name <vhd-name> --permissions rl --start ‘<start-date>’ --expiry ‘<expiry-date>’
    ```

3. Bearbeiten Sie die Datei, um die folgenden Parameterwerte zu verwenden. Verwenden Sie für Datumsangaben das UTC-Format für Datum und Uhrzeit, z. B. 2020-04-01T00:00:00Z.

    - account-name: Der Name Ihres Azure Storage-Kontos
    - account-key: Der Schlüssel Ihres Azure Storage-Kontos
    - vhd-name: Der Name Ihrer virtuellen Festplatte
    - start-date: Das Startdatum der Berechtigung für den Zugriff auf die virtuelle Festplatte. Geben Sie ein Datum an, das einen Tag vor dem aktuellen Datum liegt.
    - expiry-date: Das Ablaufdatum der Berechtigung für den Zugriff auf die virtuelle Festplatte. Geben Sie ein Datum an, das mindestens drei Wochen nach dem aktuellen Datum liegt.

    Dieses Beispiel enthält richtige Parameterwerte (zum Zeitpunkt der Veröffentlichung dieses Dokuments):

    `az storage container generate-sas --connection-string ‘DefaultEndpointsProtocol=https;AccountName=st00009;AccountKey=6L7OWFrlabs7Jn23OaR3rvY5RykpLCNHJhxsbn9ON c+bkCq9z/VNUPNYZRKoEV1FXSrvhqq3aMIDI7N3bSSvPg==;EndpointSuffix=core.windows.net’ --name vhds -- permissions rl --start ‘2020-04-01T00:00:00Z’ --expiry ‘2021-04-01T00:00:00Z’`

1. Speichern Sie die Änderungen.
2. Führen Sie dieses Skript mit einem der folgenden Verfahren mit Administratorrechten aus, um eine SAS-Verbindungszeichenfolge für den Zugriff auf Containerebene zu erstellen:

    - Führen Sie das Skript an der Konsole aus. Klicken Sie unter Windows mit der rechten Maustaste auf das Skript, und wählen Sie **Als Administrator ausführen** aus.
    - Führen Sie das Skript mit einem PowerShell-Skript-Editor aus, z. B. [Windows PowerShell ISE](https://docs.microsoft.com/powershell/scripting/components/ise/introducing-the-windows-powershell-ise). Auf diesem Bildschirm wird die Erstellung einer SAS-Verbindungszeichenfolge in diesem Editor angezeigt:

    [![Erstellung einer SAS-Verbindungszeichenfolge im PowerShell-Editor](media/vm/create-sas-uri-power-shell-ise.png)](media/vm/create-sas-uri-power-shell-ise.png#lightbox)

6. Kopieren Sie die SAS-Verbindungszeichenfolge, und speichern Sie sie in einer Textdatei an einem sicheren Ort. Bearbeiten Sie diese Zeichenfolge, und fügen Sie die Informationen zum Speicherort der virtuellen Festplatte hinzu, um den endgültigen SAS-URI zu erstellen.
7. Navigieren Sie im Azure-Portal zu dem Blobspeicher, der die VHD mit dem neuen URI enthält.
8. Kopieren Sie die URL des Blob-Dienstendpunkts:

    ![Die URL des Blob-Dienstendpunkts wird kopiert.](media/vm/create-sas-uri-blob-endpoint.png)

9. Bearbeiten Sie die Textdatei mit der SAS-Verbindungszeichenfolge aus Schritt 6. Erstellen Sie den gesamten SAS-URI im folgenden Format:

    `<blob-service-endpoint-url> + /vhds/ + <vhd-name>? + <sas-connection-string>`

## <a name="verify-the-sas-uri"></a>Überprüfen des SAS-URI

Überprüfen Sie den SAS-URI, bevor Sie ihn im Partner Center veröffentlichen, um Probleme im Zusammenhang mit der SAS-URI-Übermittlung der Anforderung zu vermeiden. Dieser Schritt ist optional, wird aber empfohlen.

- Der URI enthält den Namen Ihrer VHD-Imagedatei, einschließlich der Dateierweiterung `.vhd`.
- `Sp=rl` wird ungefähr in der Mitte Ihres URI angezeigt. Mit dieser Zeichenfolge wird angegeben, dass Zugriffsberechtigungen zum Lesen und Auflisten gewährt werden.
- `sr=c` steht für Zugriff auf Containerebene.
- Kopieren Sie den URI, und fügen Sie ihn in einen Browser ein, um das Blob testweise herunterzuladen. (Sie können den Vorgang vor Abschluss des Downloads abbrechen.)

## <a name="next-step"></a>Nächster Schritt

- Informieren Sie sich unter [Erstellen eines Angebots für virtuelle Azure-Computer](azure-vm-create-offer.md).
