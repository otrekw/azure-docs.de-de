---
title: Installieren von Azure Spatial Anchors für eine C++/WinRT-HoloLens-Anwendung
description: Konfigurieren eines C++/WinRT-HoloLens-Projekts für die Verwendung von Azure Spatial Anchors
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: how-to
ms.service: azure-spatial-anchors
ms.openlocfilehash: 43d5c1ae03c359dcbef21f8e7ba3205bc6ab0004
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92096112"
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