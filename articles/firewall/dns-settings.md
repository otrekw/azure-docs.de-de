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
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85610641"
---
# <a name="azure-firewall-dns-settings-preview"></a>DNS-Einstellungen für Azure Firewall (Vorschau)

> [!IMPORTANT]
> DNS-Einstellungen für Azure Firewall ist zurzeit als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können einen benutzerdefinierten DNS-Server konfigurieren und den DNS-Proxy für Azure Firewall aktivieren. Diese Einstellungen werden beim Bereitstellen der Firewall oder später über die Seite **DNS-Einstellungen** konfiguriert.

## <a name="dns-servers"></a>DNS-Server

Ein DNS-Server verwaltet Domänennamen und löst diese in IP-Adressen auf. Azure Firewall verwendet standardmäßig Azure DNS für die Namensauflösung. Mit der Einstellung **DNS-Server** können Sie eigene DNS-Server für die Namensauflösung von Azure Firewall konfigurieren. Hierbei kann es sich um einen einzelnen oder mehrerer Server handeln.

### <a name="configure-custom-dns-servers-preview"></a>Konfigurieren benutzerdefinierter DNS-Server (Vorschau)

1. Wählen Sie unter Azure Firewall die Option **Einstellungen** und danach **DNS-Einstellungen** aus.
2. Unter **DNS-Server** können Sie vorhandene DNS-Server eingeben oder hinzufügen, die zuvor in Ihrem virtuellen Netzwerk angegeben wurden.
3. Wählen Sie **Speichern** aus.
4. Die Firewall leitet nun DNS-Datenverkehr zur Namensauflösung an den/die angegebenen DNS-Server weiter.

:::image type="content" source="media/dns-settings/dns-servers.png" alt-text="DNS-Server":::

## <a name="dns-proxy-preview"></a>DNS-Proxy (Vorschauversion)

Sie haben auch die Möglichkeit, Azure Firewall als DNS-Proxy zu konfigurieren. Ein DNS-Proxy fungiert als Vermittler für DNS-Anforderungen, die virtuelle Clientcomputer an einen DNS-Server senden. Wenn Sie einen benutzerdefinierten DNS-Server konfigurieren, sollten Sie den DNS-Proxy aktivieren, um Konflikte bei der DNS-Auflösung zu vermeiden. Zudem sollten Sie die FQDN-Filterung in Netzwerkregeln aktivieren.

Wenn Sie den DNS-Proxy nicht aktivieren, werden die DNS-Anforderungen des Clients unter Umständen zu einem anderen Zeitpunkt als bei der Firewall an einen DNS-Server weitergeleitet oder geben eine andere Antwort als die Firewall zurück. Der DNS-Proxy fügt Azure Firewall in den Pfad der Clientanforderungen ein, um Inkonsistenzen zu vermeiden.

Die Konfiguration des DNS-Proxys umfasst drei Schritte:
1. Zuerst müssen Sie den DNS-Proxy in den DNS-Einstellungen von Azure Firewall aktivieren.
2. Konfigurieren Sie optional den benutzerdefinierten DNS-Server, oder verwenden Sie den angegebenen Standardserver.
3. Zum Schluss müssen Sie die private IP-Adresse von Azure Firewall in den DNS-Servereinstellungen Ihres virtuellen Netzwerks als benutzerdefinierte DNS-Adresse konfigurieren. Dadurch wird sichergestellt, dass der DNS-Datenverkehr an Azure Firewall weitergeleitet wird.

### <a name="configure-dns-proxy-preview"></a>Konfigurieren des DNS-Proxys (Vorschauversion)

Zum Konfigurieren des DNS-Proxys müssen Sie die DNS-Servereinstellung des virtuellen Netzwerks so konfigurieren, dass die private IP-Adresse der Firewall verwendet wird. Anschließend aktivieren Sie den DNS-Proxy in den **DNS-Einstellungen** der Azure Firewall-Richtlinie.

#### <a name="configure-virtual-network-dns-servers"></a>Konfigurieren der DNS-Server für das virtuelle Netzwerk

1. Wählen Sie das virtuelle Netzwerk aus, an das der DNS-Datenverkehr über Azure Firewall weitergeleitet werden soll.
2. Wählen Sie unter **Einstellungen** die Option **DNS-Server** aus.
3. Klicken Sie unter **DNS-Server** auf die Option **Benutzerdefiniert**.
4. Geben Sie die private IP-Adresse der Firewall ein.
5. Wählen Sie **Speichern** aus.

#### <a name="enable-dns-proxy-preview"></a>Aktivieren des DNS-Proxys (Vorschauversion)

1. Wählen Sie Ihre Azure Firewall-Instanz aus.
2. Klicken Sie unter **Einstellungen** auf die Option **DNS-Einstellungen**.
3. Der **DNS-Proxy** ist standardmäßig deaktiviert. Wenn die Firewall aktiviert ist, lauscht sie an Port 53 und leitet DNS-Anforderungen an die konfigurierten DNS-Server weiter.
4. Überprüfen Sie die Konfiguration der **DNS-Server**, um sicherzustellen, dass die Einstellungen zu Ihrer Umgebung passen.
5. Wählen Sie **Speichern** aus.

:::image type="content" source="media/dns-settings/dns-proxy.png" alt-text="DNS-Server":::

## <a name="next-steps"></a>Nächste Schritte

[FQDN-Filterung in Netzwerkregeln](fqdn-filtering-network-rules.md)