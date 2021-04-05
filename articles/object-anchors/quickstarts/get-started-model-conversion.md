---
title: 'Schnellstart: Erstellen eines Object Anchors-Modells, das in einer App verwendet werden soll'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie ein Object Anchors-Modell anhand eines 3D-Modells erstellen.
author: craigktreasure
manager: virivera
ms.author: crtreasu
ms.date: 02/22/2021
ms.topic: quickstart
ms.service: azure-object-anchors
ms.openlocfilehash: 69d23b9d02eb176a2e42985ef5c3673e83d9bb7e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102607899"
---
# <a name="quickstart-create-an-object-anchors-model-from-a-3d-model"></a>Schnellstart: Erstellen eines Object Anchors-Modells aus einem 3D-Modell

Azure Object Anchors ist ein verwalteter Clouddienst, der 3D-Modelle in KI-Modelle konvertiert, die objektbezogene Mixed Reality-Erlebnisse für HoloLens ermöglichen. In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe des C# bzw. .Net Core SDK ein Object Anchors-Modell aus einem 3D-Modell erstellen.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Erstellen eines Object Anchors-Kontos
> * Konvertieren eines 3D-Modells zum Erstellen eines Object Anchors-Modells

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie für diese Schnellstartanleitung sicher, dass Sie über Folgendes verfügen:

* Einen Windows-Computer, auf dem <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> installiert ist.
* <a href="https://git-scm.com" target="_blank">Git für Windows</a>.
* Das <a href="https://dotnet.microsoft.com/download/dotnet-core/3.1">.NET Core 3.1 SDK</a>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-object-anchors-account"></a>Erstellen eines Object Anchors-Kontos

Zunächst müssen Sie ein Konto mit dem Object Anchors-Dienst erstellen.

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und wählen Sie **Ressource erstellen** aus.

   :::image type="content" source="./media/create-aoa-resource-1.png" alt-text="Neue Ressource erstellen":::

2. Suchen Sie nach der **Object Anchors**-Ressource.

   Suchen Sie nach „Object Anchors“.

   :::image type="content" source="./media/create-aoa-resource-2.png" alt-text="Auswählen der Object Anchors-Ressource":::

   Wählen Sie in der **Object Anchors**-Ressource in den Suchergebnissen **Erstellen > Object Anchors** aus.

   :::image type="content" source="./media/create-aoa-resource-3.png" alt-text="Erstellen einer Object Anchors-Ressource":::

3. Führen Sie folgende Aktionen im Dialogfeld **Object Anchors-Konto** aus:
    * Geben Sie einen eindeutigen Ressourcennamen ein.
    * Wählen Sie das Abonnement aus, an das die Ressource angefügt werden soll.
    * Erstellen Sie eine Ressourcengruppe, oder verwenden Sie eine vorhandene.
    * Wählen Sie die Region aus, in der Sie die Ressource platzieren möchten.

    :::image type="content" source="./media/create-aoa-resource-4.png" alt-text="Eingeben der Kontodetails der Object Anchors-Ressource":::

    Wählen Sie **Erstellen** aus, um mit der Ressourcenerstellung zu beginnen.

4. Nachdem die Ressource erstellt wurde, wählen Sie **zu Ressourcen wechseln** aus.

   :::image type="content" source="./media/create-aoa-resource-5.png" alt-text="Zu Ressource wechseln":::

5. Wechseln Sie zur Übersichtsseite:

   Notieren Sie sich die **Kontodomäne**. Sie benötigen die Information später.

   :::image type="content" source="./media/create-aoa-resource-6.1.png" alt-text="Kopieren der Kontodomäne für Ihre Object Anchors-Ressource":::

   Notieren Sie sich die **Konto-ID**. Sie benötigen die Information später.

   :::image type="content" source="./media/create-aoa-resource-6.2.png" alt-text="Kopieren der Konto-ID für Ihre Object Anchors-Ressource":::

   Wechseln Sie zur Seite **Zugriffsschlüssel**, und notieren Sie sich den **Primärschlüssel**. Sie benötigen die Information später.

   :::image type="content" source="./media/create-aoa-resource-7.png" alt-text="Kopieren des Kontoschlüssels für Ihre Object Anchors-Ressource":::

## <a name="get-the-sample-project"></a>Herunterladen des Beispielprojekts

[!INCLUDE [Clone Sample Repo](../../../includes/object-anchors-clone-sample-repository.md)]

## <a name="convert-a-3d-model"></a>Konvertieren eines 3D-Modells

Nun können Sie Ihr 3D-Modell konvertieren.

1. Öffnen Sie `quickstarts/conversion/Conversion.sln` in Visual Studio. Diese Lösung enthält ein C#-Konsolenprojekt.

2. Öffnen Sie die im Stammverzeichnis des Projekts aufgeführte `Configuration.cs`-Datei, und ersetzen Sie die `set-me`-Werte in den folgenden Feldern:

   | Feld         | BESCHREIBUNG                                                         |
   |---------------|---------------------------------------------------------------------|
   | AccountDomain | Die **Kontodomäne** des oben erstellten Object Anchors-Kontos |
   | AccountId     | Die **Konto-ID** des oben erstellten Object Anchors-Kontos     |
   | AccountKey    | Der **Primärschlüssel** des oben erstellten Object Anchors-Kontos     |

   Es gibt vier zusätzliche Felder, die überprüft werden müssen:

    | Feld                    | BESCHREIBUNG                       |
    | ---                      | ---                               |
    | InputAssetPath           | Der absolute Pfad zu einem 3D-Modell auf dem lokalen Computer. Die unterstützten Dateiformate sind `fbx`, `ply`, `obj`, `glb` und `gltf`. |
    | AssetDimensionUnit       | Dies ist die Maßeinheit für das 3D-Modell. Auf alle unterstützten Maßeinheiten kann mithilfe der `Azure.MixedReality.ObjectAnchors.Conversion.AssetLengthUnit`-Enumeration zugegriffen werden. |
    | Gravity                  | Dies ist die Richtung des Schwerpunktvektors des 3D-Modells. Dieser 3D-Vektor gibt die Abwärtsrichtung im Koordinatensystem Ihres Modells an. Wenn beispielsweise das negative `y` die Abwärtsrichtung im dreidimensionalen Raum des Modells angibt, dann lautet der Wert `Vector3(0.0f, -1.0f, 0.0f)`. |

3. Erstellen Sie das Projekt, und führen Sie es aus, um Ihr 3D-Modell hochzuladen und einen neuen Konvertierungsauftrag bei dem Dienst zu registrieren, und warten Sie, bis dieser abgeschlossen ist. Sobald der Auftrag abgeschlossen ist, wird das Object Anchors-Modell neben der im `InputAssetPath` angegebenen Datei herunterladen. Etwa folgende Konsolenausgabe sollte angezeigt werden:

   ```shell
    Asset   : ***********
    Gravity : ***********
    Unit    : ***********
    Attempting to upload asset...
    Attempting to create asset conversion job...
    Successfully created asset conversion job. Job ID: ***********
    Waiting for job completion...

    Asset conversion job completed successfully.
    Attempting to download result as '***********'...
    Success!
   ```

   Notieren Sie sich die **Auftrags-ID**, um sie später zu verwenden. Diese kann beim Debuggen oder bei der Problembehandlung hilfreich sein.

4. Sobald der Auftrag erfolgreich abgeschlossen ist, sollte eine Datei im Format `<Model-Filename-Without-Extension>_<JobID>.ou` am angegebenen Ausgabespeicherort angezeigt werden. Wenn der Dateiname Ihres 3D-Modells beispielsweise `chair.ply` und Ihre Auftrags-ID `00000000-0000-0000-0000-000000000000` ist, dann lautet der Dateiname, der vom Dienst ausgegeben wird, `chair_00000000-0000-0000-0000-000000000000.ou`.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein Object Anchors-Konto erstellt und ein 3D-Modell konvertiert, um ein Object Anchors-Modell zu erstellen. Wenn Sie erfahren möchten, wie Sie dieses Modell mit dem Object Anchors-SDK in Ihrer Mixed Reality-App integrieren, fahren Sie mit den folgenden Artikeln fort:

> [!div class="nextstepaction"]
> [Unity (HoloLens)](get-started-unity-hololens.md)

> [!div class="nextstepaction"]
> [Unity HoloLens mit MRTK](get-started-unity-hololens-mrtk.md)

> [!div class="nextstepaction"]
> [HoloLens DirectX](get-started-hololens-directx.md)
