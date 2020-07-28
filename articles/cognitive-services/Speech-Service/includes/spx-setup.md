---
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/15/2020
ms.author: v-demjoh
ms.openlocfilehash: abfb4f6ba9452581811db1f462089cbafc771266
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86544063"
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
> Unter Windows können von der Speech-Befehlszeilenschnittstelle nur Schriftarten angezeigt werden, die für die Eingabeaufforderung auf dem lokalen Computer verfügbar sind.
> Das [Windows-Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701) unterstützt alle Schriftarten, die von der Speech-Befehlszeilenschnittstelle interaktiv generiert werden.
> Bei der Ausgabe einer Datei können in einem Text-Editor wie Notepad oder einem Webbrowser wie Microsoft Edge ebenfalls alle Schriftarten angezeigt werden.

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
