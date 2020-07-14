---
title: 'Tutorial: Starten von Plastischer Reader unter Verwendung des Swift-iOS-Codebeispiels'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial wird eine Swift-Beispielanwendung zum Starten von Plastischer Reader konfiguriert und ausgeführt.
services: cognitive-services
author: dylankil
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 06/10/2020
ms.author: dylankil
ms.openlocfilehash: a9259026747102dd65be3bb8da853735098667db
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86049318"
---
# <a name="tutorial-launch-the-immersive-reader-using-the-swift-ios-code-sample"></a>Tutorial: Starten von Plastischer Reader unter Verwendung des Swift-iOS-Codebeispiels

In der [Übersicht](./overview.md) haben Sie gelernt, was Plastischer Reader ist und wie das Tool bewährte Techniken implementiert, um das Leseverständnis von Sprachenlernenden, Leseanfängern und Schülern mit Lernunterschieden zu verbessern. In diesem Tutorial erfahren Sie, wie Sie eine iOS-Anwendung erstellen, die Plastischer Reader startet. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren und Ausführen einer Swift-App für iOS unter Verwendung eines Beispielprojekts
> * Abrufen eines Zugriffstokens
> * Starten von Plastischer Reader mit Beispielinhalt

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Eine Ressource des plastischen Readers, die für die Authentifizierung mit Azure Active Directory konfiguriert ist. Befolgen Sie [diese Anweisungen](./how-to-create-immersive-reader.md) für die Einrichtung. Einige der hier erstellten Werte benötigen Sie bei der Konfiguration der Umgebungseigenschaften. Speichern Sie die Ausgabe Ihrer Sitzung zur späteren Verwendung in einer Textdatei.
* [macOS](https://www.apple.com/macos)
* [Git-Client](https://git-scm.com/)
* [SDK für Plastischer Reader](https://github.com/microsoft/immersive-reader-sdk)
* [Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12)

## <a name="configure-authentication-credentials"></a>Konfigurieren von Anmeldeinformationen für die Authentifizierung

1. Öffnen Sie Xcode und anschließend **immersive-reader-sdk/js/samples/ios/quickstart-swift/quickstart-swift.xcodeproj**.
2. Klicken Sie im oberen Menü auf **Product > Scheme > Edit Scheme...** („Produkt“ > „Schema“ > „Schema bearbeiten...“).
3. Klicken Sie in der Ansicht **Run** (Ausführen) auf die Registerkarte **Arguments** (Argumente).
4. Fügen Sie im Abschnitt **Umgebungsvariablen** die folgenden Namen und Werte hinzu. Verwenden Sie dabei die Werte, die beim Erstellen der Ressource des plastischen Readers angegeben wurden.

```text
TENANT_ID=<YOUR_TENANT_ID>
CLIENT_ID=<YOUR_CLIENT_ID>
CLIENT_SECRET<YOUR_CLIENT_SECRET>
SUBDOMAIN=<YOUR_SUBDOMAIN>
```

Committen Sie die obige Änderung nicht in Ihrer Quellcodeverwaltung, da sie Geheimnisse enthält, die nicht für die Öffentlichkeit bestimmt sind.

## <a name="launch-the-immersive-reader-with-sample-content"></a>Starten von Plastischer Reader mit Beispielinhalt

1. Drücken Sie in Xcode die Tastenkombination **STRG+R**, um das Projekt auszuführen.

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit dem [SDK für Plastischer Reader](https://github.com/microsoft/immersive-reader-sdk) und der [zugehörigen Referenz](./reference.md) vertraut.
* Sehen Sie sich Codebeispiele auf [GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/).
