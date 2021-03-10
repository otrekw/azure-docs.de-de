---
title: Verwenden von Azure Blob Storage für die Modellkonvertierung
description: Enthält eine Beschreibung der allgemeinen Schritte zum Einrichten und Verwenden von Blobspeicher für die Modellkonvertierung.
author: jakrams
ms.author: jakras
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: d8a6fd458cdcf79cdeb693b25acf72d4ec48def7
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102507516"
---
# <a name="use-azure-blob-storage-for-model-conversion"></a>Verwenden von Azure Blob Storage für die Modellkonvertierung

Für den Dienst für die [Modellkonvertierung](model-conversion.md) ist der Zugriff auf Azure Blob Storage erforderlich, damit Eingabedaten abgerufen und Ausgabedaten gespeichert werden können. In diesem Artikel wird beschrieben, wie Sie die am häufigsten verwendeten Schritte ausführen.

## <a name="prepare-azure-storage-accounts"></a>Vorbereiten von Azure Storage-Konten

- Erstellen eines Speicherkontos (StorageV2)
- Erstellen eines Eingabeblobcontainers im Speicherkonto (z. B. mit dem Namen „arrinput“)
- Erstellen eines Ausgabeblobcontainers im Speicherkonto (z. B. mit dem Namen „arroutput“)

> [!TIP]
> Eine Schritt-für-Schritt-Anleitung zur Einrichtung Ihres Speicherkontos finden Sie unter [Schnellstart: Konvertieren eines Modells für das Rendering](../../quickstarts/convert-model.md).

Für die Erstellung des Speicherkontos und der Blobcontainer kann eines der folgenden Tools verwendet werden:

- [Azure-Portal](https://portal.azure.com)
- [Azure CLI](/cli/azure/install-azure-cli)
- [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/)
- SDKs (C#, Python...)

## <a name="ensure-azure-remote-rendering-can-access-your-storage-account"></a>Sicherstellen, dass für Azure Remote Rendering Zugriff auf Ihr Speicherkonto besteht

Für Azure Remote Rendering müssen Modelldaten aus Ihrem Speicherkonto abgerufen und wieder hineingeschrieben werden.

Es gibt zwei Möglichkeiten, wie Sie für Azure Remote Rendering den Zugriff auf Ihr Speicherkonto gewähren können:

### <a name="connect-your-azure-storage-account-with-your-azure-remote-rendering-account"></a>Verbinden Ihres Azure Storage-Kontos mit Ihrem Azure Remote Rendering-Konto

Führen Sie die Schritte im Abschnitt [Erstellen eines Kontos](../create-an-account.md#link-storage-accounts) aus.

### <a name="retrieve-sas-for-the-storage-containers"></a>Abrufen der SAS für die Speichercontainer

Shared Access Signatures (SAS) werden verwendet, um Lesezugriff für die Eingabe und Schreibzugriff für die Ausgabe zu gewähren. Wir empfehlen Ihnen, bei jeder Konvertierung eines Modells neue URIs zu generieren. Da URIs nach einiger Zeit ablaufen, besteht beim Speichern über einen längeren Zeitraum das Risiko, dass es für Ihre Anwendung zu einer unerwarteten Beschädigung kommt.

Ausführliche Informationen zu SAS finden Sie in der [SAS-Dokumentation](../../../storage/common/storage-sas-overview.md).

Zum Generieren eines SAS-URI können Sie Folgendes verwenden:

- Azure PowerShell-Modul
  - Siehe [PowerShell-Beispielskripts](../../samples/powershell-example-scripts.md)
- [Azure CLI](/cli/azure/install-azure-cli)
- [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/)
  - Klicken Sie mit der rechten Maustaste auf den Container „Shared Access Signature abrufen“ (Lesen/Auflisten für Eingabecontainer, Schreiben für Ausgabecontainer).
- SDKs (C#, Python...)

Ein Beispiel für die Verwendung von Shared Access Signatures bei der Ressourcenkonvertierung finden Sie in der Datei „Conversion.ps1“ unter den [PowerShell-Beispielskripts](../../samples/powershell-example-scripts.md#script-conversionps1).

## <a name="upload-an-input-model"></a>Hochladen eines Eingabemodells

Zum Starten der Konvertierung eines Modells müssen Sie es hochladen, indem Sie eine der folgenden Optionen verwenden:

- [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/): Komfortable Benutzeroberfläche zum Hochladen, Herunterladen und Verwalten von Dateien in Azure-Blobspeicher
- [Azure-Befehlszeile](../../../storage/blobs/storage-quickstart-blobs-cli.md)
- [Azure PowerShell-Modul](/powershell/azure/install-az-ps)
  - Siehe [PowerShell-Beispielskripts](../../samples/powershell-example-scripts.md)
- [Verwenden eines Storage-SDK (Python, C#...)](../../../storage/index.yml)
- [Verwenden der Azure Storage-REST-API](/rest/api/storageservices/blob-service-rest-api)

Ein Beispiel für das Hochladen von Daten für die Konvertierung finden Sie in der Datei „Conversion.ps1“ unter den [PowerShell-Beispielskripts](../../samples/powershell-example-scripts.md#script-conversionps1).

## <a name="get-a-sas-uri-for-the-converted-model"></a>Abrufen eines SAS-URI für das konvertierte Modell

Dieser Schritt ähnelt dem [Abrufen einer SAS für die Speichercontainer](#retrieve-sas-for-the-storage-containers). Dieses Mal müssen Sie aber einen SAS-URI für die Modelldatei abrufen, die in den Ausgabecontainer geschrieben wurde.

Klicken Sie beispielsweise zum Abrufen eines SAS-URI mit dem [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) mit der rechten Maustaste auf die Modelldatei, und wählen Sie die Option „Shared Access Signature abrufen“ aus.

Eine Shared Access Signature (SAS) zum Laden von Modellen wird benötigt, wenn Sie Ihr Speicherkonto nicht mit Ihrem Azure Remote Rendering-Konto verbunden haben. Informationen zum Verbinden Ihres Kontos finden Sie unter [Erstellen eines Kontos](../create-an-account.md#link-storage-accounts).

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren der Modellkonvertierung](configure-model-conversion.md)
- [REST-API für die Modellkonvertierung](conversion-rest-api.md)
