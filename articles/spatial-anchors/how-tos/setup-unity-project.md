---
title: Installieren von Azure Spatial Anchors für Unity
description: Konfigurieren eines Unity-Projekts für die Verwendung von Azure Spatial Anchors
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 07/31/2020
ms.topic: how-to
ms.service: azure-spatial-anchors
ms.openlocfilehash: a23cff8bcef2d62b1e415997ffd6afc80cf47793
ms.sourcegitcommit: 85eb6e79599a78573db2082fe6f3beee497ad316
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2020
ms.locfileid: "87812256"
---
# <a name="configuring-azure-spatial-anchors-in-a-unity-project"></a>Konfigurieren von Azure Spatial Anchors in einem Unity-Projekt

## <a name="requirements"></a>Requirements (Anforderungen)

Azure Spatial Anchors unterstützt derzeit Unity 2019.4 (LTS) mit den folgenden Konfigurationen:

* Unity 2019.4 mit AR Foundation 3.1 wird in Azure Spatial Anchors 2.4.0 und höheren Versionen unterstützt.

## <a name="configuring-a-project"></a>Konfigurieren eines Projekts

Azure Spatial Anchors für Unity wird derzeit mithilfe eines Unity-Ressourcenpakets (`.unitypackage`) verteilt. Dieses ist in den [GitHub-Releases](https://github.com/Azure/azure-spatial-anchors-samples/releases) verfügbar.

### <a name="import-the-asset-package"></a>Importieren des Ressourcenpakets

1. Laden Sie die Datei `AzureSpatialAnchors.unitypackage` für die Version, die Sie als Ziel verwenden möchten, aus den [GitHub-Releases](https://github.com/Azure/azure-spatial-anchors-samples/releases) herunter.
2. Befolgen Sie [diese Anweisungen](https://docs.unity3d.com/Manual/AssetPackagesImport.html), um das Unity-Ressourcenpaket in Ihr Projekt zu importieren.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Erstellen und Suchen von Ankern in Unity](./create-locate-anchors-unity.md)
