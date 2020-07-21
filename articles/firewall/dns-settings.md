---
title: DNS-Einstellungen für Azure Firewall (Vorschau)
description: Sie können Azure Firewall mit DNS-Server- und DNS-Proxyeinstellungen konfigurieren.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 9c7182205df8d276bece4758d6d4430864883d32
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85610641"
---
# <a name="azure-firewall-dns-settings-preview"></a>DNS-Einstellungen für Azure Firewall (Vorschau)

> [!IMPORTANT]
> DNS-Einstellungen für Azure Firewall ist zurzeit als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können einen benutzerdefinierten DNS-Server konfigurieren und den DNS-Proxy für Azure Firewall aktivieren. Sie können diese Einstellungen über die Seite **DNS-Einstellungen** konfigurieren, wenn Sie die Firewall bereitstellen, oder die Einstellungen zu einem späteren Zeitpunkt vornehmen.

## <a name="dns-servers"></a>DNS-Server

Ein DNS-Server verwaltet Domänennamen und löst diese in IP-Adressen auf. Azure Firewall verwendet standardmäßig Azure DNS für die Namensauflösung. Mit der Einstellung **DNS-Server** können Sie eigene DNS-Server für die Azure Firewall-Namensauflösung konfigurieren. Sie können einen einzelnen Server oder mehrere Server konfigurieren.

### <a name="configure-custom-dns-servers-preview"></a>Konfigurieren benutzerdefinierter DNS-Server (Vorschau)

1. Wählen Sie unter Azure Firewall die Option **Einstellungen** und danach **DNS-Einstellungen** aus.
2. Unter **DNS-Server** können Sie vorhandene DNS-Server eingeben oder hinzufügen, die zuvor in Ihrem virtuellen Netzwerk angegeben wurden.
3. Wählen Sie **Speichern** aus.
4. Die Firewall leitet nun DNS-Datenverkehr zur Namensauflösung an den/die angegebenen DNS-Server weiter.

:::image type="content" source="media/dns-settings/dns-servers.png" alt-text="DNS-Server":::

## <a name="dns-proxy-preview"></a>DNS-Proxy (Vorschau)

Sie können Azure Firewall so konfigurieren, dass sie als DNS-Proxy fungiert. Ein DNS-Proxy fungiert als Vermittler für DNS-Anforderungen von virtuellen Clientcomputern an einen DNS-Server. Wenn Sie einen benutzerdefinierten DNS-Server konfigurieren, sollten Sie den DNS-Proxy aktivieren, um DNS-Auflösungskonflikte zu vermeiden, und zudem die FQDN-Filterung in den Netzwerkregeln aktivieren.

Wenn Sie den DNS-Proxy nicht aktivieren, können DNS-Anforderungen vom Client zu einem anderen Zeitpunkt an einen DNS-Server weitergeleitet werden oder eine andere Antwort als die der Firewall zurückgeben. Der DNS-Proxy fügt Azure Firewall in den Pfad der Clientanforderungen ein, um Inkonsistenzen zu vermeiden.

Für die Konfiguration des DNS-Proxy sind drei Schritte erforderlich:
1. Aktivieren Sie den DNS-Proxy in den DNS-Einstellungen von Azure Firewall.
2. Konfigurieren Sie optional den benutzerdefinierten DNS-Server, oder verwenden Sie die bereitgestellten Standardwerte.
3. Zum Schluss müssen Sie die private IP-Adresse von Azure Firewall als benutzerdefinierte DNS-Adresse in den DNS-Servereinstellungen Ihres virtuellen Netzwerks konfigurieren. Dadurch wird sichergestellt, dass DNS-Datenverkehr an Azure Firewall weitergeleitet wird.

### <a name="configure-dns-proxy-preview"></a>Konfigurieren des DNS-Proxys (Vorschau)

Zum Konfigurieren des DNS-Proxys müssen Sie die DNS-Servereinstellungen Ihres virtuellen Netzwerks für die Verwendung der privaten IP-Adresse der Firewall konfigurieren. Aktivieren Sie dann den DNS-Proxy in der Azure Firewall-Richtlinie **DNS-Einstellungen**.

#### <a name="configure-virtual-network-dns-servers"></a>Konfigurieren von DNS-Servern für virtuelle Netzwerke

1. Wählen Sie das virtuelle Netzwerk aus, an das der DNS-Datenverkehr über Azure Firewall weitergeleitet werden soll.
2. Wählen Sie unter **Einstellungen** die Option **DNS-Server** aus.
3. Wählen Sie unter **DNS-Server** die Option **Benutzerdefiniert** aus.
4. Geben Sie die private IP-Adresse der Firewall ein.
5. Wählen Sie **Speichern** aus.

#### <a name="enable-dns-proxy-preview"></a>Aktivieren des DNS-Proxys (Vorschau)

1. Wählen Sie Ihre Azure Firewall-Instanz aus.
2. Wählen Sie unter **Einstellungen** die Option **DNS-Einstellungen** aus.
3. Der **DNS-Proxy** ist standardmäßig deaktiviert. Wenn die Firewall aktiviert ist, lauscht sie an Port 53 und leitet DNS-Anforderungen an die konfigurierten DNS-Server weiter.
4. Überprüfen Sie die Konfiguration der **DNS-Server**, um sicherzustellen, dass die Einstellungen für Ihre Umgebung geeignet sind.
5. Wählen Sie **Speichern** aus.

:::image type="content" source="media/dns-settings/dns-proxy.png" alt-text="DNS-Proxy":::

## <a name="next-steps"></a>Nächste Schritte

[FQDN-Filterung in Netzwerkregeln](fqdn-filtering-network-rules.md)