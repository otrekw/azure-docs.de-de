---
title: Installieren von Azure Spatial Anchors für eine C++/WinRT-HoloLens-Anwendung
description: Konfigurieren eines C++/WinRT-HoloLens-Projekts für die Verwendung von Azure Spatial Anchors
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 11/20/2020
ms.topic: how-to
ms.service: azure-spatial-anchors
ms.openlocfilehash: 69134ef87d90fe69de2d9e4e9222e90f65edc785
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "95507007"
---
# <a name="configuring-azure-spatial-anchors-in-a-cwinrt-hololens-project"></a>Konfigurieren von Azure Spatial Anchors in einem C++/WinRT-HoloLens-Projekt

## <a name="requirements"></a>Requirements (Anforderungen)

* Installation von [Visual Studio 2019](https://www.visualstudio.com/downloads/) mit der Workload **Entwicklung für die universelle Windows-Plattform** und der Komponente **Windows 10 SDK (10.0.18362.0 oder höher)** .

## <a name="configuring-a-project"></a>Konfigurieren eines Projekts

Azure Spatial Anchors für HoloLens und C++/WinRT wird über das NuGet-Paket [Microsoft.Azure.SpatialAnchors.WinRT](https://www.nuget.org/packages/Microsoft.Azure.SpatialAnchors.WinRT/) verteilt.

Befolgen Sie [diese Anweisungen](/nuget/consume-packages/install-use-packages-visual-studio), um das NuGet-Paket [Microsoft.Azure.SpatialAnchors.WinRT](https://www.nuget.org/packages/Microsoft.Azure.SpatialAnchors.WinRT/) mithilfe des NuGet-Paket-Managers von Visual Studio in Ihrem Projekt zu installieren.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorgehensweise: Erstellen und Suchen von Ankern in C++/WinRT](./create-locate-anchors-cpp-winrt.md)