---
title: 'Tutorial: Starten von Plastischer Reader unter Verwendung der Android-Codebeispiele'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial wird eine Android-Beispielanwendung zum Starten von Plastischer Reader konfiguriert und ausgeführt.
services: cognitive-services
author: dylankil
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 06/10/2020
ms.author: dylankil
ms.custom: devx-track-java
ms.openlocfilehash: f8420a42b87481b5e51b383ace1ba8a510d2a6a7
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87366737"
---
# <a name="tutorial-start-the-immersive-reader-using-the-android-java-code-sample"></a>Tutorial: Starten von Plastischer Reader unter Verwendung des Java-basierten Android-Codebeispiels

In der [Übersicht](./overview.md) haben Sie gelernt, was Plastischer Reader ist und wie das Tool bewährte Techniken implementiert, um das Leseverständnis von Sprachenlernenden, Leseanfängern und Schülern mit Lernunterschieden zu verbessern. In diesem Tutorial erfahren Sie, wie Sie eine Android-Anwendung erstellen, die Plastischer Reader startet. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren und Ausführen einer App für Android mit einem Beispielprojekt
> * Abrufen eines Zugriffstokens
> * Starten von Plastischer Reader mit Beispielinhalt

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Eine Ressource des plastischen Readers, die für die Authentifizierung mit Azure Active Directory konfiguriert ist. Befolgen Sie [diese Anweisungen](./how-to-create-immersive-reader.md) für die Einrichtung. Einige der hier erstellten Werte benötigen Sie bei der Konfiguration der Umgebungseigenschaften. Speichern Sie die Ausgabe Ihrer Sitzung zur späteren Verwendung in einer Textdatei.
* [Git](https://git-scm.com/).
* [Immersive Reader SDK](https://github.com/microsoft/immersive-reader-sdk)
* [Android Studio](https://developer.android.com/studio)

## <a name="configure-authentication-credentials"></a>Konfigurieren von Anmeldeinformationen für die Authentifizierung

1. Starten Sie Android Studio, und öffnen Sie das Projekt im Verzeichnis **immersive-reader-sdk/js/samples/quickstart-java-android** (Java) oder im Verzeichnis **immersive-reader-sdk/js/samples/quickstart-kotlin** (Kotlin).

1. Erstellen Sie eine Datei mit dem Namen **env** im Ordner **/assets**. Fügen Sie die folgenden Namen und Werte hinzu, und geben Sie entsprechende Werte an. Committen Sie diese Datei nicht in die Quellcodeverwaltung, da sie Geheimnisse enthält, die nicht für die Öffentlichkeit bestimmt sind.
    
    ```text
    TENANT_ID=<YOUR_TENANT_ID>
    CLIENT_ID=<YOUR_CLIENT_ID>
    CLIENT_SECRET=<YOUR_CLIENT_SECRET>
    SUBDOMAIN=<YOUR_SUBDOMAIN>
    ```

## <a name="start-the-immersive-reader-with-sample-content"></a>Starten von Plastischer Reader mit Beispielinhalt

Wählen Sie im AVD-Manager einen Geräteemulator aus, und führen Sie das Projekt aus.

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit dem [Immersive Reader SDK](https://github.com/microsoft/immersive-reader-sdk) und der [zugehörigen Referenz](./reference.md) vertraut.
* Sehen Sie sich Codebeispiele auf [GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/) an.