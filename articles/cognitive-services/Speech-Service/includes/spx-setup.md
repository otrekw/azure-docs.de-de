---
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/15/2020
ms.author: v-demjoh
ms.openlocfilehash: b0f70fccf3f7f4a6856ae64d0946c2c473fed93c
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86050263"
---
## <a name="prerequisites"></a>Voraussetzungen

Die einzige Voraussetzung ist ein Azure Speech-Abonnement. Informationen zum Erstellen eines neuen Abonnements – falls Sie noch keins besitzen – finden Sie im [Handbuch](../get-started.md#new-resource).

## <a name="download-and-install"></a>Herunterladen und Installieren

#### <a name="windows-install"></a>[Windows-Installation](#tab/windowsinstall)

Führen Sie diese Schritte aus, um die Speech-Befehlszeilenschnittstelle unter Windows zu installieren:

1. Installieren Sie entweder [.NET Framework 4.7](https://dotnet.microsoft.com/download/dotnet-framework/net471) oder [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0)
2. Laden Sie das [Zip-Archiv](https://aka.ms/speech/spx-zips.zip) der Speech-Befehlszeilenschnittstelle herunter, und extrahieren Sie es.
3. Wechseln Sie in das Stammverzeichnis `spx-zips`, das Sie aus dem Download extrahiert haben, und extrahieren Sie das benötigte Unterverzeichnis (`spx-net471` für .NET Framework 4.7 oder `spx-netcore-win-x64` für .NET Core 3.0 auf einer x64-CPU).

Wechseln Sie an der Eingabeaufforderung das Verzeichnis zu diesem Speicherort, und geben Sie dann `spx` ein, um die Hilfe zur Speech-Befehlszeilenschnittstelle anzuzeigen.

> [!NOTE]
> PowerShell prüft bei der Suche nach einem Befehl nicht das lokale Verzeichnis. Ändern Sie in Powershell das Verzeichnis zum Speicherort von `spx`, und rufen Sie das Tool auf, indem Sie `.\spx` eingeben.
> Wenn Sie dieses Verzeichnis Ihrem Pfad hinzufügen, finden Powershell und die Windows-Eingabeaufforderung `spx` von jedem Verzeichnis aus auch ohne enthaltenes Präfix `.\`.

#### <a name="linux-install"></a>[Linux-Installation](#tab/linuxinstall)

Führen Sie diese Schritte aus, um die Speech-Befehlszeilenschnittstelle unter Linux auf einer x64-CPU zu installieren:

1. Installieren Sie [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0).
2. Laden Sie das [Zip-Archiv](https://aka.ms/speech/spx-zips.zip) der Speech-Befehlszeilenschnittstelle herunter, und extrahieren Sie es.
3. Wechseln Sie zum Stammverzeichnis `spx-zips`, das Sie aus dem Download extrahiert haben, und extrahieren Sie `spx-netcore-30-linux-x64` in ein neues Verzeichnis `~/spx`.
4. Geben Sie an einem Terminal die folgenden Befehle ein:
   1. `cd ~/spx`
   2. `sudo chmod +r+x spx`
   3. `PATH=~/spx:$PATH`

Geben Sie `spx` ein, um die Hilfe für die Speech-Befehlszeilenschnittstelle anzuzeigen.

***

## <a name="create-subscription-config"></a>Erstellen der Abonnementkonfiguration

Um mit der Verwendung der Speech-Befehlszeilenschnittstelle zu beginnen, müssen Sie zuerst Ihren Speech-Abonnementschlüssel und die Regionsinformationen eingeben. Informationen zur Ermittlung Ihres Regionsbezeichners finden Sie auf der Seite zur [Unterstützung von Regionen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk). Nachdem Sie Ihren Abonnementschlüssel und den Regionsbezeichner (z. B. `eastus`, `westus`) erhalten haben, führen Sie die folgenden Befehle aus.

```shell
spx config @key --set YOUR-SUBSCRIPTION-KEY
spx config @region --set YOUR-REGION-ID
```

Ihre Abonnementauthentifizierung wird jetzt für zukünftige SPX-Anforderungen gespeichert. Wenn Sie einen dieser gespeicherten Werte entfernen müssen, führen Sie `spx config @region --clear` oder `spx config @key --clear` aus.
