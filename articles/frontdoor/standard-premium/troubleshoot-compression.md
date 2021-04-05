---
title: Behandlung von Problemen bei de Dateikomprimierung in Azure Front Door Standard/Premium
description: Erfahren Sie, wie Sie Probleme bei der Dateikomprimierung in Azure Front Door beheben können. In diesem Artikel werden verschiedene mögliche Ursachen behandelt.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2020
ms.author: qixwang
ms.openlocfilehash: c912ebde5499ec2f9f68e5f5762af60823faefe1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101098086"
---
# <a name="troubleshooting-azure-front-door-standardpremium-file-compression"></a>Behandeln von Problemen bei der Dateikomprimierung in Azure Front Door Standard/Premium

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

Dieser Artikel unterstützt Sie bei der Behandlung von Problemen bei der Dateikomprimierung in Azure Front Door Standard/Premium.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="symptom"></a>Symptom

Die Komprimierung für die Route ist aktiviert, die Dateien werden aber nicht komprimiert zurückgegeben.

> [!TIP]
> Um zu überprüfen, ob Ihre Dateien komprimiert zurückgegeben werden, müssen Sie ein Tool wie [Fiddler](https://www.telerik.com/fiddler) oder die [Entwicklertools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) Ihres Browsers verwenden.  Überprüfen Sie die HTTP-Antwortheader, die mit Ihrem zwischengespeicherten CDN.Inhalt zurückgegeben werden.  Wenn ein Header mit dem Namen `Content-Encoding` mit einem Wert von **gzip**, **bzip2** oder **deflate** vorhanden ist, werden Ihre Inhalte komprimiert.
> 
> ![Content-Encoding-Header](../media/troubleshoot-compression/content-header.png)
> 

## <a name="cause"></a>Ursache

Es gibt mehrere mögliche Ursachen, darunter folgende:

* Der angeforderte Inhalt eignet sich nicht zur Komprimierung.
* Die Komprimierung ist für den angeforderten Dateityp nicht aktiviert.
* Die HTTP-Anforderung enthielt keinen Header, um einen gültigen Komprimierungstyp anzufordern.
* Der Ursprung sendet segmentierten Inhalt.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

> [!TIP]
> Genauso wie beim Bereitstellen neuer Endpunkte dauert es eine gewisse Zeit, bis Änderungen an der ADF-Konfiguration im gesamten Netzwerk verteilt sind.  Änderungen werden in der Regel innerhalb von 90 Minuten angewendet.  Wenn Sie die Komprimierung für Ihren CDN-Endpunkt zum ersten Mal einrichten, sollten Sie jedoch 1–2 Stunden warten, um sicherzugehen, dass die Komprimierungseinstellungen an alle POPs verteilt wurden. 
> 

### <a name="verify-the-request"></a>Überprüfen der Anforderung

Zuerst sollten Sie die Anforderung überprüfen. Sie können die **[Entwicklertools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)** Ihres Browsers verwenden, um die Anforderungen anzuzeigen.

* Überprüfen Sie die Anforderung, die an die Endpunkt-URL gesendet wird (`<endpointname>.z01.azurefd.net`), nicht Ihre ursprüngliche Anforderung.
* Überprüfen Sie, ob die Anforderung einen **Accept-Encoding**-Header enthält und ob der Wert für diesen Header **gzip**, **deflate** oder **bzip2** enthält.

![CDN-Anforderungsheader](../media/troubleshoot-compression/request-headers.png)

### <a name="verify-compression-settings"></a>Überprüfen der Komprimierungseinstellungen

Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu dem Endpunkt, und wählen Sie im Panel „Routen“ die Schaltfläche **Konfigurieren** aus. Überprüfen Sie, ob die Komprimierung **aktiviert** ist.

![CDN-Komprimierungseinstellungen](../media/troubleshoot-compression/compression-settings.png)

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Überprüfen Sie die Anforderung auf dem Ursprungsserver auf einen **Über** -Header.

Der **Über** -HTTP-Header informiert den Webserver darüber, dass die Anforderung von einem Proxyserver übergeben wird.  Microsoft IIS-Webserver komprimieren Antworten standardmäßig nicht, wenn die Anforderung einen **Via**-Header enthält.  Um dieses Verhalten zu überschreiben, führen Sie die folgenden Schritte aus:

* **IIS 6**: Legen Sie in den IIS-Metabasiseigenschaften „HcNoCompressionForProxies="FALSE"“ fest. Weitere Informationen finden Sie unter [IIS 6-Komprimierung](/previous-versions/iis/6.0-sdk/ms525390(v=vs.90)).
* **IIS 7 oder höher**: Legen Sie in der Serverkonfiguration sowohl für **noCompressionForHttp10** als auch für **noCompressionForProxies** die Option *False* fest. Weitere Informationen finden Sie unter [HTTP Compression](https://www.iis.net/configreference/system.webserver/httpcompression) (HTTP-Komprimierung, in englischer Sprache).
