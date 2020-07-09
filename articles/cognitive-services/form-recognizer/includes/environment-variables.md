---
title: Umgebungsvariablen
description: Festlegen von Umgebungsvariablen
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 05/06/2020
ms.author: pafarley
ms.openlocfilehash: 28362a81461b63440ad752071f11b3603a979995
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86100287"
---
Erstellen Sie unter Verwendung des Schlüssels und des Endpunkts der von Ihnen erstellten Ressource zwei Umgebungsvariablen für die Authentifizierung:

* `FORM_RECOGNIZER_KEY`: Der Ressourcenschlüssel zum Authentifizieren Ihrer Anforderungen.
* `FORM_RECOGNIZER_ENDPOINT`: Der Ressourcenendpunkt zum Senden von API-Anforderungen. Er sieht wie folgt aus: 
  * `https://<your-custom-subdomain>.cognitiveservices.azure.com`

>[!NOTE]
> Nach dem 1. Juli 2019 erstellte Endpunkte für Ressourcen, bei denen es sich nicht um Testressourcen handelt, verwenden das unten gezeigte benutzerdefinierte Format für Subdomänen. Weitere Informationen und eine vollständige Liste mit regionalen Endpunkten finden Sie unter [Benutzerdefinierte Unterdomänennamen für Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Verwenden Sie die folgenden Anweisungen, um auf Ihrem Betriebssystem Umgebungsvariablen festzulegen.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx FORM_RECOGNIZER_KEY <replace-with-your-form-recognizer-key>
setx FORM_RECOGNIZER_ENDPOINT <replace-with-your-form-recognizer-endpoint>
```

Nachdem Sie die Umgebungsvariablen hinzugefügt haben, schließen Sie das Konsolenfenster, und öffnen Sie es neu.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export FORM_RECOGNIZER_KEY=<replace-with-your-product-name-key>
export FORM_RECOGNIZER_ENDPOINT=<replace-with-your-product-name-endpoint>
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source ~/.bashrc` aus, damit die Änderungen wirksam werden.

#### <a name="macos"></a>[macOS](#tab/unix)

Bearbeiten Sie `.bash_profile`, und fügen Sie die Umgebungsvariable hinzu:

```bash
export FORM_RECOGNIZER_KEY=<replace-with-your-product-name-key>
export FORM_RECOGNIZER_ENDPOINT=<replace-with-your-product-name-endpoint>
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source .bash_profile` aus, damit die Änderungen wirksam werden.
***
