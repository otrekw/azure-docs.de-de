---
title: DNS-Einstellungen für Azure Firewall-Richtlinien (Vorschauversion)
description: Azure Firewall-Richtlinien können mit DNS-Server- und DNS-Proxyeinstellungen konfiguriert werden.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: how-to
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: bf189e4c9853d9f2ff6e8495423f4f36f14f41d4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85611797"
---
# <a name="azure-firewall-policy-dns-settings-preview"></a>DNS-Einstellungen für Azure Firewall-Richtlinien (Vorschauversion)

> [!IMPORTANT]
> DNS-Einstellungen für Azure Firewall ist zurzeit als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können von nun an für Azure Firewall-Richtlinien einen benutzerdefinierten DNS-Server konfigurieren sowie einen DNS-Proxy aktivieren. Diese Einstellungen werden beim Bereitstellen der Firewall oder später über die Seite **DNS-Einstellungen** konfiguriert.

## <a name="dns-servers"></a>DNS-Server

Ein DNS-Server verwaltet Domänennamen und löst diese in IP-Adressen auf. Azure Firewall verwendet standardmäßig Azure DNS für die Namensauflösung. Mit der Einstellung **DNS-Server** können Sie eigene DNS-Server für die Namensauflösung von Azure Firewall konfigurieren. Hierbei kann es sich um einen einzelnen oder mehrerer Server handeln.

### <a name="configure-custom-dns-servers"></a>Konfigurieren von benutzerdefinierten DNS-Servern

1. Wählen Sie Ihre Firewallrichtlinie aus.
2. Klicken Sie unter **Einstellungen** auf die Option **DNS-Einstellungen**.
3. Unter **DNS-Server** können Sie vorhandene DNS-Server, die zuvor in Ihrem virtuellen Netzwerk eingerichtet wurden, eingeben oder hinzufügen.
4. Wählen Sie **Speichern** aus.
5. Die Firewall leitet nun den DNS-Datenverkehr zur Namensauflösung an den oder die angegebenen DNS-Server weiter.

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

1. Wählen Sie Ihre Azure Firewall-Richtlinie aus.
2. Klicken Sie unter **Einstellungen** auf die Option **DNS-Einstellungen**.
3. Der **DNS-Proxy** ist standardmäßig deaktiviert. Wenn die Firewall aktiviert ist, lauscht sie an Port 53 und leitet DNS-Anforderungen an die konfigurierten DNS-Server weiter.
4. Überprüfen Sie die Konfiguration der **DNS-Server**, um sicherzustellen, dass die Einstellungen zu Ihrer Umgebung passen.
5. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

[FQDN-Filterung in Netzwerkregeln](fqdn-filtering-network-rules.md)