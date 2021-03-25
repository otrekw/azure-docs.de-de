---
title: Einbetten des Internet Analyzer-Clients | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie den Internet Analyzer-JavaScript-Client in Ihre Anwendung einbetten.
services: internet-analyzer
author: mattcalder
ms.service: internet-analyzer
ms.topic: how-to
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: 0d4b27b85ac7bc61e14a79f29e4e26ec4973ced1
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "84744050"
---
# <a name="embed-the-internet-analyzer-client"></a>Einbetten des Internet Analyzer-Clients

In diesem Artikel wird gezeigt, wie Sie den JavaScript-Client in Ihre Anwendung einbetten. Die Installation dieses Clients ist zum Ausführen von Tests und zum Empfangen von Scorecardanalysen erforderlich. **Der profilspezifische JavaScript-Client wird nach dem Konfigurieren des ersten Tests bereitgestellt.** Anschließend können Sie dem Profil weitere Tests hinzufügen bzw. Tests entfernen, ohne ein neues Skript einbetten zu müssen. Weitere Informationen zu Internet Analyzer finden Sie in der [Übersicht](internet-analyzer-overview.md). 

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Voraussetzungen

Internet Analyzer benötigt Zugriff auf Azure und andere Microsoft-Dienste, um ordnungsgemäß zu funktionieren. Gewähren Sie dem Netzwerk Zugriff auf `fpc.msedge.net` und alle vorkonfigurierten Endpunkt-URLs (anzeigbar über die [CLI](internet-analyzer-cli.md)), bevor Sie den Client einbetten.

## <a name="find-the-client-script-url"></a>Ermitteln der Clientskript-URL

Die Skript-URL kann entweder über das Azure-Portal oder die Azure CLI gesucht werden, nachdem ein Test konfiguriert wurde. Weitere Informationen finden Sie unter [Erstellen einer Internet Analyzer-Ressource](internet-analyzer-create-test-portal.md).

Option 1. Verwenden Sie im Azure-Portal [diesen Link](https://aka.ms/InternetAnalyzerPreviewPortal), um die Vorschauportalseite für Azure Internet Analyzer zu öffnen. Navigieren Sie zu Ihrem Internet Analyzer-Profil, um die Skript-URL anzuzeigen (unter **Einstellungen > Konfiguration**).

Option 2. Überprüfen Sie mithilfe der Azure CLI die Eigenschaft `scriptFileUri`.
```azurecli-interactive
    az extension add --name internet-analyzer    
    az internet-analyzer test list --resource-group "MyInternetAnalyzerResourceGroup" --profile-name "MyInternetAnalyzerProfile"
```

## <a name="client-details"></a>Clientdetails

Das Skript wird speziell für Ihr Profil und Ihre Tests generiert. Nach dem Laden wird das Skript nach einer Verzögerung von zwei Sekunden ausgeführt. Zuerst wird der Kontakt zum Internet Analyzer-Dienst hergestellt, um die Liste der in den Tests konfigurierten Endpunkte abzurufen. Anschließend werden die Messungen ausgeführt und die zeitgesteuerten Ergebnisse in den Internet Analyzer-Dienst hochgeladen.

## <a name="client-examples"></a>Clientbeispiele

Diese Beispiele zeigen einige grundlegende Methoden zum Einbetten des JavaScript-Clients in Ihre Webseite oder Anwendung. Wir verwenden `0bfcb32638b44927935b9df86dcfe397` als Beispielprofil-ID für die Skript-URL.

### <a name="run-on-page-load"></a>Ausführen beim Laden der Seite
Die einfachste Methode ist die Verwendung des Skripttags innerhalb des Metatagblocks. Mit diesem Tag wird das Skript jeweils einmal ausgeführt, wenn die Seite geladen wird.

```html
<html>
<meta>
    <script src="//fpc.msedge.net/client/v2/0bfcb32638b44927935b9df86dcfe397/ab.min.js"></script>
</meta>
<body></body>
</html>
```

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Internet Analyzer](internet-analyzer-faq.md).
