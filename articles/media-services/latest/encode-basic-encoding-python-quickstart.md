---
title: 'Schnellstart: Grundlegende Media Services-Codierung mit Python'
description: In dieser Schnellstartanleitung wird veranschaulicht, wie Sie die grundlegende Codierung mit Python und Azure Media Services durchführen.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.workload: ''
ms.topic: quickstart
ms.date: 7/2/2021
ms.author: inhenkel
ms.openlocfilehash: a0534bc562d20f96cc1c12a8b4dbe0adfc8f9282
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114284402"
---
# <a name="media-services-basic-encoding-with-python"></a>Grundlegende Media Services-Codierung mit Python

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

## <a name="introduction"></a>Einführung

In dieser Schnellstartanleitung wird veranschaulicht, wie Sie die grundlegende Codierung mit Python und Azure Media Services durchführen. Hierbei wird die API „2020-05-01 Media Service v3“ verwendet.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
- Erstellen Sie eine Ressourcengruppe für diese Schnellstartanleitung.
- Erstellen Sie ein Media Services v3-Konto.
- Beschaffen Sie sich Ihren Speicherkontoschlüssel.
- Erstellen Sie einen Dienstprinzipal und einen Schlüssel.

## <a name="get-the-sample"></a>Abrufen des Beispiels

Erstellen Sie einen Fork, und klonen Sie das Beispiel, das sich im [Repository mit den Python-Beispielen](https://github.com/Azure-Samples/media-services-v3-python) befindet. In dieser Schnellstartanleitung verwenden wir das Beispiel „BasicEncoding“.

## <a name="create-the-env-file"></a>Erstellen der ENV-Datei

Ermitteln Sie die Werte über Ihr Konto, um eine *ENV*-Datei zu erstellen. Vergeben Sie hierbei keinen Namen, sondern verwenden Sie nur die Erweiterung (.env).  Nutzen Sie *sample.env* als Vorlage, und speichern Sie die Datei *.env* dann unter Ihrem lokalen Klon im Ordner „BasicEncoder“.

## <a name="use-python-virtual-environments"></a>Verwenden virtueller Python-Umgebungen
Wir empfehlen, dass Sie für jedes Beispiel immer eine virtuelle Python-Umgebung mithilfe der folgenden Schritte erstellen und aktivieren:

1. Öffnen Sie den Beispielordner in VSCode oder einem anderen Editor.
2. Erstellen der virtuellen Umgebung

    ``` bash
      # py -3 uses the global python interpreter. You can also use python -m venv .venv.
      py -3 -m venv .venv
    ```

   Dieser Befehl führt das Python-Modul „venv“ aus und erstellt eine virtuelle Umgebung in einem Ordner namens „.venv“.

3. Aktivieren der virtuellen Umgebung:

    ``` bash
      .venv\scripts\activate
    ```

  Eine virtuelle Umgebung ist ein Ordner innerhalb eines Projekts, der eine Kopie eines bestimmten Python-Interpreters isoliert. Sobald Sie diese Umgebung aktiviert haben (was in Visual Studio Code automatisch erfolgt), wird durch das Ausführen von „pip install“ nur eine Bibliothek in dieser Umgebung installiert. Wenn Sie dann Ihren Python-Code ausführen, wird er im exakten Kontext der Umgebung mit spezifischen Versionen jeder einzelnen Bibliothek ausgeführt. Und wenn Sie dann „pip freeze“ ausführen, erhalten Sie die genaue Liste dieser Bibliotheken. (In vielen Beispielen in dieser Dokumentation erstellen Sie die Datei „requirements.txt“ für die benötigten Bibliotheken und verwenden dann „pip install -r requirements.txt“. Eine Datei mit den Anforderungen ist generell erforderlich, wenn Sie Code in Azure bereitstellen.)

## <a name="set-up"></a>Einrichten

Einrichten und [Konfigurieren Ihrer lokalen Python-Entwicklungsumgebung für Azure](/azure/developer/python/configure-local-development-environment)

Installieren Sie die Bibliothek „azure-identity“ für Python. Dieses Modul ist für die Azure Active Directory-Authentifizierung erforderlich. Ausführliche Informationen finden Sie unter [Azure-Identitätsclientbibliothek für Python: Version 1.6.0](/python/api/overview/azure/identity-readme#environment-variables).

  ``` bash
  pip install azure-identity
  ```

Installieren Sie das Python SDK für [Azure Media Services](/python/api/overview/azure/media-services).

Die Pypi-Seite für das Media Services Python SDK mit den neuesten Versionsdetails finden Sie unter [azure-mgmt-media](https://pypi.org/project/azure-mgmt-media/).

  ``` bash
  pip install azure-mgmt-media
  ```

Installieren Sie das [Azure Storage SDK für Python](https://pypi.org/project/azure-storage-blob/):

  ``` bash
  pip install azure-storage-blob
  ```

Sie können optional ALLE Anforderungen für ein bestimmtes Beispiel installieren. Verwenden Sie dazu die Datei „requirements.txt“ im Ordner mit den Beispielen.

  ``` bash
  pip install -r requirements.txt
  ```

## <a name="try-the-code"></a>Ausprobieren des Codes

Der folgende Code ist ausführlich kommentiert.  Verwenden Sie das gesamte Skript oder Teile davon für Ihr eigenes Skript.

In diesem Beispiel wird eine Zufallszahl für die Benennung von Elementen generiert, damit Sie diese beim Ausführen des Skripts als gemeinsam erstellte Gruppe identifizieren können.  Die Zufallszahl ist optional und kann entfernt werden, wenn Sie mit dem Testen des Skripts fertig sind.

Wir verwenden die SAS-URL in diesem Beispiel nicht als Eingabemedienobjekt.

[!code-python[Main](../../../media-services-v3-python/BasicEncoding/basic-encoding.py)]

## <a name="delete-resources"></a>Löschen von Ressourcen

Nachdem Sie die Schnellstartanleitung durchgearbeitet haben, sollten Sie die in der Ressourcengruppe erstellten Ressourcen löschen.

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit dem [Python-SDK für Media Services](/python/api/azure-mgmt-media/) vertraut.

## <a name="resources"></a>Ressourcen

- Informieren Sie sich über die [Verwaltungs-API](/python/api/overview/azure/mediaservices/management) von Azure Media Services.
- Erfahren Sie, wie Sie die [Storage-APIs mit Python](/azure/developer/python/azure-sdk-example-storage-use?tabs=cmd) verwenden.
- Lesen Sie weitere Informationen zur [Azure-Identitätsclientbibliothek für Python](/python/api/overview/azure/identity-readme#environment-variables).
- Informieren Sie sich ausführlicher über [Azure Media Services v3](./media-services-overview.md).
- Erfahren Sie mehr über die [Azure Python SDKs](/azure/developer/python).
- Erfahren Sie mehr über die [Verwendungsmuster für Azure Python SDKs](/azure/developer/python/azure-sdk-library-usage-patterns).
- Weitere Azure Python SDKs finden Sie im [Index mit Azure Python SDKs](/azure/developer/python/azure-sdk-library-package-index).
- [Referenz zu den Python SDKs von Azure Storage Blob](/python/api/azure-storage-blob/)