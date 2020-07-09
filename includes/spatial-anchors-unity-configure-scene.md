---
author: craigktreasure
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 1/2/2019
ms.author: crtreasu
ms.openlocfilehash: 7e7825e8247e78cbc0c0e9e22bdbd9326939e0a8
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83998105"
---
Im nächsten Schritt wird die App zur Verwendung des Kontobezeichners und Kontoschlüssels konfiguriert. Diese haben Sie beim [Einrichten der Spatial Anchors-Ressource](#create-a-spatial-anchors-resource) in einen Text-Editor kopiert.

Navigieren Sie im Bereich **Projekt** zu `Assets\AzureSpatialAnchors.SDK\Resources`. Wählen Sie `SpatialAnchorConfig`aus. Geben Sie dann im Bereich **Inspektor**`Account Key` als Wert für `Spatial Anchors Account Key` und `Account ID` als Wert für `Spatial Anchors Account Id` ein.

Öffnen Sie als Nächstes `SpatialAnchorManager.cs`. Suchen Sie `CreateSessionAsync()`, und fügen Sie die folgende Zeile hinzu, die in Ihrer Kontodomäne den vorherigen Wert ersetzt: `session.Configuration.AccountDomain = "MyAccountDomain";`. Sie können diese Zeile direkt vor dem Kommentar `// Configure authentication` hinzufügen.
