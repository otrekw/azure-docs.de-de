---
title: Hosten mehrerer Websites mit Azure Application Gateway
description: Dieser Artikel bietet eine Übersicht über die Azure Application Gateway-Unterstützung für mehrere Websites.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 07/20/2020
ms.author: surmb
ms.topic: conceptual
ms.openlocfilehash: 53f6f37454de886934a483b40daad24204958baf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87474324"
---
# <a name="application-gateway-multiple-site-hosting"></a>Anwendungsgateways – Hosten mehrerer Websites

Das Hosten mehrerer Websites ermöglicht es Ihnen, mehr als eine Webanwendung am selben Port eines Anwendungsgateways zu konfigurieren. Sie können eine effizientere Topologie für Ihre Bereitstellungen konfigurieren, indem Sie bis zu hundert Websites zu einem einzigen Anwendungsgateway hinzufügen. Jede Website kann an ihren eigenen Back-End-Pool weitergeleitet werden. Beispielsweise können die drei Domänen „contoso.com“, „fabrikam.com“ und „adatum.com“ auf die IP-Adresse des Anwendungsgateways verweisen. Sie erstellen drei Listener vom Typ „Mehrere Websites“ und konfigurieren jeden Listener für den jeweiligen Port und die jeweilige Protokolleinstellung. 

Sie können auch Hostnamen mit Platzhaltern in einem Listener für mehrere Standorte und bis zu fünf Hostnamen pro Listener definieren. Weitere Informationen finden Sie unter [Hostnamen mit Platzhaltern in Listenern](#wildcard-host-names-in-listener-preview).

:::image type="content" source="./media/multiple-site-overview/multisite.png" alt-text="Anwendungsgateway für mehrere Standorte":::

> [!IMPORTANT]
> Regeln werden in der Reihenfolge verarbeitet, in der sie im Portal für die v1-SKU aufgeführt werden. Bei der v2-SKU haben genaue Übereinstimmungen eine höhere Rangfolge. Es wird dringend empfohlen, vor dem Konfigurieren eines einfachen Listeners zuerst Listener für mehrere Standorte zu konfigurieren.  So wird sichergestellt, dass der Datenverkehr an das richtige Back-End geleitet wird. Wenn ein einfacher Listener zuerst aufgeführt wird und sich dafür eine Übereinstimmung mit einer eingehenden Anforderung ergibt, wird die Verarbeitung von diesem Listener durchgeführt.

Anforderungen für `http://contoso.com` werden an „ContosoServerPool“ und Anforderungen für `http://fabrikam.com` an „FabrikamServerPool“ weitergeleitet.

Analog dazu können Sie mehrere Unterdomänen derselben übergeordneten Domäne in derselben Application Gateway-Bereitstellung hosten. Beispielsweise können Sie `http://blog.contoso.com` und `http://app.contoso.com` in einer einzelnen Application Gateway-Bereitstellung hosten.

## <a name="wildcard-host-names-in-listener-preview"></a>Hostnamen mit Platzhaltern in Listenern (Vorschau)

Application Gateway ermöglicht mithilfe von HTTP(S)-Listenern für mehrere Standorte das hostbasierte Routing. So können Sie nun im Hostnamen Platzhalterzeichen wie Sternchen (*) und Fragezeichen (?) und bis zu fünf Hostnamen pro HTTP(S)-Listener für mehrere Standorte verwenden. Beispiel: `*.contoso.com`.

Durch ein Platzhalterzeichen im Hostnamen können Sie mehrere Hostnamen einem einzigen Listener zuordnen. So umfasst `*.contoso.com` beispielsweise `ecom.contoso.com`, `b2b.contoso.com` und auch `customer1.b2b.contoso.com`. Mithilfe eines Arrays von Hostnamen können Sie für einen Listener mehrere Hostnamen konfigurieren, um Anforderungen an einen Back-End-Pool weiterzuleiten. So kann ein Listener beispielsweise `contoso.com, fabrikam.com` enthalten und so Anforderungen für beide Hostnamen akzeptieren.

:::image type="content" source="./media/multiple-site-overview/wildcard-listener-diag.png" alt-text="Anwendungsgateway für mehrere Standorte":::

>[!NOTE]
> Dieses Feature befindet sich in der Vorschauphase und ist nur für die Application Gateway-SKUs „Standard_v2“ und „WAF_v2“ verfügbar. Weitere Informationen zu Vorschauversionen finden Sie in den [Nutzungsbestimmungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

>[!NOTE]
>Dieses Feature ist derzeit nur über [Azure PowerShell](tutorial-multiple-sites-powershell.md) und [Azure CLI](tutorial-multiple-sites-cli.md) verfügbar. Unterstützung für das Portal ist in Kürze verfügbar.
> Beachten Sie Folgendes: Da die Portalunterstützung nicht vollständig verfügbar ist, wenn Sie nur den HostNames-Parameter verwenden, wird der Listener im Portal als Basislistener angezeigt, und in der Spalte „Hostname“ der Listenerlistenansicht werden nicht die konfigurierten Hostnamen angezeigt. Wenn Sie Änderungen an einem Platzhalterlistener vornehmen möchten, stellen Sie sicher, dass Sie Azure PowerShell oder die CLI verwenden, bis dies im Portal unterstützt wird.

In [Azure PowerShell](tutorial-multiple-sites-powershell.md) müssen Sie `-HostNames` anstelle von `-HostName` verwenden. Wenn Sie „HostNames“ verwenden, können Sie bis zu fünf Hostnamen als durch Trennzeichen getrennte Werte angeben sowie Platzhalterzeichen verwenden. Zum Beispiel, `-HostNames "*.contoso.com,*.fabrikam.com"`

In [Azure CLI](tutorial-multiple-sites-cli.md) müssen Sie `--host-names` anstelle von `--host-name` verwenden. Wenn Sie „host-names“ verwenden, können Sie bis zu fünf Hostnamen als durch Trennzeichen getrennte Werte angeben sowie Platzhalterzeichen verwenden. Zum Beispiel, `--host-names "*.contoso.com,*.fabrikam.com"`

### <a name="allowed-characters-in-the-host-names-field"></a>Zulässige Zeichen im Feld „Hostnamen“:

* `(A-Z,a-z,0-9)`: alphanumerische Zeichen
* `-`: Bindestrich oder Minus
* `.`: Punkt als Trennzeichen
*   `*`: kann für mehrere zulässige Zeichen stehen
*   `?`: kann für ein einzelnes zulässiges Zeichen stehen

### <a name="conditions-for-using-wildcard-characters-and-multiple-host-names-in-a-listener"></a>Bedingungen für die Verwendung von Platzhalterzeichen und von mehreren Hostnamen in einem Listener:

*   In einem einzelnen Listener können nur bis zu fünf Hostnamen verwendet werden.
*   Das Sternchen (`*`) kann in einer Komponente eines Domänenformat- oder Hostnamens nur einmal verwendet werden. Beispiel: Komponente1 *.Komponente2*.Komponente3. `(*.contoso-*.com)` ist gültig.
*   In einem Hostnamen dürfen maximal zwei Sternchen (`*`) verwendet werden. Beispiel: `*.contoso.*` ist gültig, `*.contoso.*.*.com` ist ungültig.
*   Ein Hostname darf maximal vier Platzhalterzeichen enthalten. Beispiel: `????.contoso.com` und `w??.contoso*.edu.*` sind gültig, `????.contoso.*` ist ungültig.
*   Das Sternchen (`*`) und das Fragezeichen (`?`) dürfen in einer Komponente eines Hostnamens nicht zusammen verwendet werden (`*?`, `?*` oder `**`). Beispiel: `*?.contoso.com` und `**.contoso.com` sind ungültig.

### <a name="considerations-and-limitations-of-using-wildcard-or-multiple-host-names-in-a-listener"></a>Überlegungen und Einschränkungen bei der Verwendung von Platzhaltern oder mehreren Hostnamen in einem Listener:

*   Für [SSL-Terminierung und End-to-End-SSL](ssl-overview.md) müssen Sie das Protokoll als HTTPS konfigurieren und ein Zertifikat hochladen, das in der Listenerkonfiguration verwendet werden soll. Bei einem Listener für mehrere Standorte können Sie auch den Hostnamen eingeben. Dabei handelt es sich in der Regel um den allgemeinen Namen des SSL-Zertifikats. Wenn Sie im Listener mehrere Hostnamen oder Platzhalterzeichen verwenden, müssen Sie Folgendes beachten:
    *   Bei einem Hostnamen mit Platzhalter wie „*.contoso.com“ müssen Sie ein Platzhalterzertifikat mit einem allgemeinen Namen wie „*.contoso.com“ hochladen.
    *   Werden in einem Listener mehrere Hostnamen verwendet, müssen Sie ein SAN-Zertifikat (Subject Alternative Names, alternative Antragstellernamen) mit den allgemeinen Namen hochladen, die mit den verwendeten Hostnamen übereinstimmen.
*   Zur Angabe des Hostnamens kann kein regulärer Ausdruck verwendet werden. Das Muster des Hostnamens kann nur mithilfe von Platzhalterzeichen wie Sternchen (*) und Fragezeichen (?) gebildet werden.
*   Zur Überprüfung der Back-End-Integrität können pro HTTP-Einstellung nicht mehrere [benutzerdefinierte Tests](application-gateway-probe-overview.md) zugeordnet werden. Testen Sie stattdessen eine der Websites am Back-End, oder überprüfen Sie mit „127.0.0.1“ den lokalen Host auf dem Back-End-Server. Wenn Sie jedoch in einem Listener Platzhalter oder mehrere Hostnamen verwenden, werden die Anforderungen für alle angegebenen Domänenmuster je nach Regeltyp (einfach oder pfadbasiert) an den Back-End-Pool weitergeleitet.
*   Die Eigenschaft „hostname“ akzeptiert als Eingabe eine Zeichenfolge, die nur einen Domänennamen ohne Platzhalter enthalten darf, die Eigenschaft „hostnames“ akzeptiert als Eingabe ein Array von Zeichenfolgen mit bis zu fünf Domänennamen mit Platzhaltern. Es können jedoch nicht beide Eigenschaften gleichzeitig verwendet werden.
*   Es ist nicht möglich, für einen Ziellistener mit Platzhalter oder mehreren Hostnamen eine [Umleitungsregel](redirect-overview.md) zu erstellen.

Eine detaillierte Anleitung zur Konfiguration von Hostnamen mit Platzhaltern in einem Listener für mehrere Standorte finden Sie unter [Erstellen eines Anwendungsgateways als Host für mehrere Websites mit Azure PowerShell](tutorial-multiple-sites-powershell.md) oder [mit Azure CLI](tutorial-multiple-sites-cli.md).

## <a name="host-headers-and-server-name-indication-sni"></a>Hostheader und Servernamensanzeige

Es gibt drei allgemeine Mechanismen, um das Hosten mehrerer Websites in der gleichen Infrastruktur zu ermöglichen.

1. Hosten von mehreren Webanwendungen, jeweils mit einer eindeutigen IP-Adresse.
2. Verwenden der Hostnamen zum Hosten mehrerer Webanwendungen mit der gleichen IP-Adresse.
3. Verwenden verschiedener Ports zum Hosten mehrerer Webanwendungen mit der gleichen IP-Adresse.

Derzeit unterstützt Application Gateway eine einzelne öffentliche IP-Adresse, an der es auf Datenverkehr lauscht. Daher werden mehrere Anwendungen mit eigenen IP-Adressen zurzeit nicht unterstützt. 

Application Gateway unterstützt mehrere Anwendungen, die jeweils an verschiedenen Ports lauschen. Für dieses Szenario müssen die Anwendungen aber Datenverkehr über nicht standardmäßige Ports akzeptieren. Diese Konfiguration ist oftmals unerwünscht.

Application Gateway verwendet HTTP 1.1-Hostheader, um mehrere Websites an der gleichen öffentlichen IP-Adresse und dem gleichen Port zu hosten. Die in Application Gateway gehosteten Websites können mit der TLS-Erweiterung für die Servernamensanzeige (Server Name Indication, SNI) auch die SSL-Auslagerung unterstützen. Das bedeutet, dass der Clientbrowser und die Back-End-Webfarm HTTP/1.1- und TLS-Erweiterungen unterstützen müssen, wie in RFC 6066 definiert.

## <a name="next-steps"></a>Nächste Schritte

Hier erfahren Sie, wie Sie in Application Gateway ein Anwendungsgateway als Host für mehrere Websites konfigurieren:
* [Verwenden des Azure-Portals](create-multiple-sites-portal.md)
* [Verwenden von Azure PowerShell](tutorial-multiple-sites-powershell.md) 
* [Verwenden der Azure-Befehlszeilenschnittstelle](tutorial-multiple-sites-cli.md)

Eine vollständige vorlagenbasierte Bereitstellung finden Sie unter [Application Gateway for Multi Hosting](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) (Anwendungsgateway für das Multihosting).
