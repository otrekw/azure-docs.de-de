---
title: Hinzufügen der Azure-Portal-URLs zu einer Liste sicherer Adressen für Ihre Firewall oder Ihren Proxyserver
description: Hinzufügen dieser URL zu einer Proxyserverumgehung für die Kommunikation mit dem Azure-Portal und dessen Dienste
services: azure-portal
keywords: ''
author: mgblythe
ms.author: mblythe
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: c5bba7296a05cfbb72698a991ece1ef298689bd1
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76900661"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>Hinzufügen der Azure-Portal-URLs zu einer Liste sicherer Adressen für Ihre Firewall oder Ihren Proxyserver

Sie können lokale Sicherheitsgeräte so konfigurieren, dass Sicherheitseinschränkungen für die Azure-Portal-URLs umgangen werden. Diese Konfiguration kann die Leistung und Konnektivität zwischen Ihrem lokalen Netzwerk oder einem WAN und der Azure-Cloud verbessern.

Netzwerkadministratoren stellen häufig Proxyserver, Firewalls oder andere Geräte bereit. Diese Geräte helfen dabei, die Art und Weise zu steuern, wie Benutzer auf das Internet zugreifen. Regeln, die Benutzer schützen sollen, können gerechtfertigten und geschäftsbezogenen Internetdatenverkehr blockieren oder verlangsamen. Dies betrifft z. B. auch Ihre Kommunikation mit Azure. Zur Optimierung der Konnektivität zwischen Ihrem Netzwerk und dem Azure-Portal und dessen Diensten wird empfohlen, Azure-Portal-URLS zu Ihrer Liste mit sicheren Adressen hinzuzufügen.

## <a name="azure-portal-urls-for-proxy-bypass"></a>Azure-Portal-URLs für Proxyumgehung

Die für das Azure-Portal in die Liste mit sicheren Adressen aufzunehmenden URL-Endpunkte sind für die Azure-Cloud spezifisch, in der Ihre Organisation bereitgestellt wird. Um dem Netzwerkdatenverkehr an diese Endpunkte zu erlauben, Einschränkungen zu umgehen, wählen Sie Ihre Cloud aus. Fügen Sie dann die Liste der URLs Ihrem Proxyserver oder der Firewall hinzu.

#### <a name="public-cloudtabpublic-cloud"></a>[Öffentliche Cloud](#tab/public-cloud)

```
*.aadcdn.microsoftonline-p.com
*.aka.ms
*.applicationinsights.io
*.azure.com
*.azure.net
*.azureafd.net
*.azure-api.net
*.azuredatalakestore.net
*.azureedge.net
*.loganalytics.io
*.microsoft.com
*.microsoftonline.com
*.microsoftonline-p.com
*.msauth.net
*.msftauth.net
*.trafficmanager.net
*.visualstudio.com
*.windows.net
*.windows-int.net
```

#### <a name="us-government-cloudtabus-government-cloud"></a>[US- Government Cloud](#tab/us-government-cloud)

```
*.azure.us
*.loganalytics.us
*.microsoft.us
*.microsoftonline.us
*.msauth.net
*.usgovcloudapi.net
*.usgovtrafficmanager.net
*.windowsazure.us
```

#### <a name="china-government-cloudtabchina-government-cloud"></a>[China-Government Cloud](#tab/china-government-cloud)

```
*.azure.cn
*.microsoft.cn
*.microsoftonline.cn
*.chinacloudapi.cn
*.trafficmanager.cn
*.chinacloudsites.cn
*.windowsazure.cn
```
---

> [!NOTE]
> Der Datenverkehr für diese Endpunkte verwendet TCP-Standardports für HTTP (80) und HTTPS (443).
>
>
## <a name="next-steps"></a>Nächste Schritte

Sie möchten IP-Adressen auf eine Liste mit sicheren Adressen setzen? Laden Sie die Liste der IP-Bereiche der Microsoft Azure-Rechenzentren für Ihre Cloud herunter:

* [Weltweit](https://www.microsoft.com/download/details.aspx?id=56519)
* [US- Government](https://www.microsoft.com/download/details.aspx?id=57063)
* [Deutschland](https://www.microsoft.com/download/details.aspx?id=57064)
* [China](https://www.microsoft.com/download/details.aspx?id=57062)

Andere Microsoft-Dienste verwenden zusätzliche URLs und IP-Adressen für die Konnektivität. Weitere Informationen zum Optimieren der Netzwerkkonnektivität für Microsoft 365-Dienste finden Sie unter [Set up your network for Office 365 (Einrichten Ihres Netzwerks für Office 365)](/office365/enterprise/set-up-network-for-office-365).
