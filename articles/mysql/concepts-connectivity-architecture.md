---
title: Verbindungsarchitektur – Azure Database for MySQL
description: Hier wird die Verbindungsarchitektur für Ihren Azure Database for MySQL-Server beschrieben.
author: Bashar-MSFT
ms.author: bahusse
ms.service: mysql
ms.topic: conceptual
ms.date: 2/11/2021
ms.openlocfilehash: 3ec582a429008fc073f68cbc9795e264d6814ccb
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101730013"
---
# <a name="connectivity-architecture-in-azure-database-for-mysql"></a>Verbindungsarchitektur in Azure Database for MySQL
In diesem Artikel wird die Verbindungsarchitektur von Azure Database for MySQL beschrieben, und Sie erfahren, wie Datenverkehr von Clients innerhalb und außerhalb von Azure an Ihre Azure Database for MySQL-Instanz weitergeleitet wird.

## <a name="connectivity-architecture"></a>Verbindungsarchitektur
Die Verbindung mit Ihrer Azure Database for MySQL-Instanz wird über ein Gateway hergestellt, das eingehende Verbindungen an den physischen Ort weiterleitet, an dem sich Ihr Server in unseren Clustern befindet. Das folgende Diagramm veranschaulicht den Datenverkehrsfluss.

:::image type="content" source="./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png" alt-text="Übersicht über die Verbindungsarchitektur":::

Wenn der Client eine Verbindung mit der Datenbank herstellt, wird die Verbindungszeichenfolge für den Server in die Gateway-IP-Adresse aufgelöst. Das Gateway lauscht an Port 3306 auf die IP-Adresse. Innerhalb des Datenbankclusters wird der Datenverkehr an die entsprechende Azure Database for MySQL-Instanz weitergeleitet. Für die Verbindungsherstellung mit Ihrem Server (etwa von Unternehmensnetzwerken aus) muss daher die **clientseitige Firewall geöffnet werden, damit ausgehender Datenverkehr unsere Gateways erreichen kann**. Weiter unten finden Sie eine vollständige, nach Region aufgeschlüsselte Liste der IP-Adressen, die von unseren Gateways verwendet werden.

## <a name="azure-database-for-mysql-gateway-ip-addresses"></a>IP-Adressen des Azure Database for MySQL-Gateways

Der Gatewaydienst wird in einer Gruppe von zustandslosen Serverknoten gehostet, deren IP-Adresse der Client beim Herstellen einer Verbindung mit einem Azure Database for MySQL-Server zuerst erreichen würde. 

Im Rahmen der kontinuierlichen Dienstwartung wird die Computehardware, auf der die Gateways gehostet werden, regelmäßig aktualisiert, um eine möglichst sichere und leistungsfähige Umgebung bereitzustellen. Bei einer Aktualisierung der Gatewayhardware wird zuerst ein neuer Ring von Serverknoten erstellt. Dieser neue Ring dient dem Datenverkehr für alle neu erstellten Azure Database for MySQL-Server. Er verfügt über eine andere IP-Adresse als frühere Gatewayringe in derselben Region, damit der Datenverkehr unterschieden werden kann. Sobald der neue Ring voll funktionsfähig ist, wird für die ältere Gatewayhardware, die für die vorhandenen Server genutzt wurde, die Außerbetriebnahme geplant. Kunden, die ihre Server auf der Gatewayhardware ausführen und eine Verbindung mit älteren Gatewayringen herstellen, werden drei Monate vor der Außerbetriebnahme der Gatewayhardware per E-Mail und im Azure-Portal benachrichtigt. Die Außerbetriebnahme von Gateways kann sich in den folgenden Fällen auf die Konnektivität Ihrer Server auswirken: 

* Sie codieren die Gateway-IP-Adressen fest in die Verbindungszeichenfolgen Ihrer Anwendung ein. Dies wird **nicht empfohlen**. Sie sollten den vollqualifizierten Domänennamen (FQDN) Ihres Servers im Format „<servername>.mysql.database.azure.com“ in der Verbindungszeichenfolge für Ihre Anwendung verwenden. 
* Sie aktualisieren die neuen Gateway-IP-Adressen nicht in der clientseitigen Firewall, sodass ausgehender Datenverkehr die neuen Gatewayringe nicht erreichen kann.

Die folgende Tabelle enthält die Gateway-IP-Adressen des Azure Database for MySQL-Gateways für alle Datenregionen. In dieser Tabelle werden immer die aktuellsten Informationen zu den Gateway-IP-Adressen für die einzelnen Regionen aufgeführt. Die Spalten in der folgenden Tabelle stellen Folgendes dar:

* **Gateway-IP-Adressen:** In dieser Spalte werden die aktuellen IP-Adressen der Gateways aufgelistet, die auf der neuesten Hardwaregeneration gehostet werden. Wenn Sie einen neuen Server bereitstellen, sollten Sie die clientseitige Firewall öffnen, um ausgehenden Datenverkehr an die in dieser Spalte aufgeführten IP-Adressen zuzulassen.
* **Gateway-IP-Adressen (Außerbetriebnahme):** In dieser Spalte werden die IP-Adressen der Gateways aufgelistet, die auf einer älteren Hardwaregeneration gehostet werden, die derzeit außer Betrieb genommen wird. Wenn Sie einen neuen Server bereitstellen, können Sie diese IP-Adressen ignorieren. Bei einem vorhandenen Server behalten Sie die Ausgangsregel in der Firewall für diese IP-Adressen bei, da sie noch nicht außer Betrieb genommen wurden. Wenn Sie die Firewallregeln für diese IP-Adressen entfernen, treten möglicherweise Verbindungsfehler auf. Sie sollten stattdessen die neuen IP-Adressen, die in der Spalte „Gateway-IP-Adressen“ aufgeführt sind, proaktiv der Ausgangsregel in der Firewall hinzufügen, sobald Sie über die Außerbetriebnahme benachrichtigt werden. Damit stellen Sie sicher, dass bei einer Migration Ihres Servers zur neuesten Gatewayhardware keine Unterbrechungen der Konnektivität mit dem Server auftreten.
* **Gateway-IP-Adressen (außer Betrieb genommen):** In diesen Spalten werden die IP-Adressen der Gatewayringe aufgelistet, die bereits außer Betrieb genommen wurden und nicht mehr verwendet werden. Sie können diese IP-Adressen sicher aus Ihrer ausgehenden Firewallregel entfernen. 


| **Regionsname** | **Gateway-IP-Adressen** |**Gateway-IP-Adressen (Außerbetriebnahme)** | **Gateway-IP-Adressen (außer Betrieb genommen)** |
|:----------------|:-------------------------|:-------------------------------------------|:------------------------------------------|
| Australien, Mitte| 20.36.105.0  | | |
| Australien, Mitte 2     | 20.36.113.0  | | |
| Australien (Osten) | 13.75.149.87, 40.79.161.1     |  | |
| Australien, Südosten |191.239.192.109, 13.73.109.251   |  | |
| Brasilien Süd |191.233.201.8, 191.233.200.16    |  | 104.41.11.5|
| Kanada, Mitte |40.85.224.249  | | |
| Kanada, Osten | 40.86.226.166    | | |
| USA (Mitte) | 23.99.160.139, 52.182.136.37, 52.182.136.38 | 13.67.215.62 | |
| China, Osten | 139.219.130.35    | | |
| China, Osten 2 | 40.73.82.1  | | |
| China, Norden | 139.219.15.17    | | |
| China, Norden 2 | 40.73.50.0     | | |
| Asien, Osten | 191.234.2.139, 52.175.33.150, 13.75.33.20, 13.75.33.21     | | |
| East US |40.71.8.203, 40.71.83.113 |40.121.158.30|191.238.6.43 |
| USA (Ost) 2 | 40.70.144.38, 52.167.105.38  | 52.177.185.181 | |
| Frankreich, Mitte | 40.79.137.0, 40.79.129.1  | | |
| Frankreich, Süden | 40.79.177.0     | | |
| Deutschland, Mitte | 51.4.144.100     | | |
| Deutschland, Nordosten | 51.5.144.179  | | |
| Indien, Mitte | 104.211.96.159     | | |
| Indien, Süden | 104.211.224.146  | | |
| Indien, Westen | 104.211.160.80    | | |
| Japan, Osten | 40.79.192.23, 40.79.184.8 | 13.78.61.196 | |
| Japan, Westen | 191.238.68.11, 40.74.96.6, 40.74.96.7     | 104.214.148.156 | |
| Korea, Mitte | 52.231.17.13   | 52.231.32.42 | |
| Korea, Süden | 52.231.145.3     | 52.231.200.86 | |
| USA Nord Mitte | 52.162.104.35, 52.162.104.36    | 23.96.178.199 | |
| Nordeuropa | 52.138.224.6, 52.138.224.7  | 40.113.93.91 |191.235.193.75 |
| Südafrika, Norden  | 102.133.152.0    | | |
| Südafrika, Westen | 102.133.24.0   | | |
| USA Süd Mitte |104.214.16.39, 20.45.120.0  |13.66.62.124  |23.98.162.75 |
| Südostasien | 40.78.233.2, 23.98.80.12     | 104.43.15.0 | |
| VAE, Mitte | 20.37.72.64  | | |
| Vereinigte Arabische Emirate, Norden | 65.52.248.0    | | |
| UK, Süden | 51.140.184.11   | | |
| UK, Westen | 51.141.8.11  | | |
| USA, Westen-Mitte | 13.78.145.25     | | |
| Europa, Westen |13.69.105.208, 104.40.169.187 | 40.68.37.158 | 191.237.232.75 |
| USA (Westen) |13.86.216.212, 13.86.217.212 |104.42.238.205  | 23.99.34.75|
| USA, Westen 2 | 13.66.226.202  | | |
||||

## <a name="connection-redirection"></a>Verbindungsumleitung

Azure Database for MySQL unterstützt eine zusätzliche Verbindungsrichtlinie, **Umleitung**, mit der die Netzwerklatenz zwischen Clientanwendungen und MySQL-Servern reduziert werden kann. Bei diesem Feature gibt der Server die Back-End-Adresse des Knotens, auf dem der MySQL-Server gehostet wird, an den Client zurück, nachdem die erste TCP-Sitzung mit dem Azure Database for MySQL-Server eingerichtet wurde. Anschließend werden alle nachfolgenden Pakete direkt an den Server übertragen, wobei das Gateway umgangen wird. Wenn Pakete direkt an den Server übertragen werden, wird die Leistung von Latenz und Durchsatz verbessert.

Dieses Feature wird in Azure Database for MySQL-Servern mit den Engine-Versionen 5.6, 5.7 und 8.0 unterstützt.

Die Umleitungsunterstützung steht in der von Microsoft entwickelten Erweiterung [PHP mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) und in [PECL](https://pecl.php.net/package/mysqlnd_azure) zur Verfügung. Weitere Informationen zur Verwendung der Umleitung in Ihren Anwendungen finden Sie im Artikel [Konfigurieren der Umleitung](./howto-redirection.md).

> [!IMPORTANT]
> Die Umleitungsunterstützung in der PHP-Erweiterung [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) befindet sich derzeit in der Vorschauphase.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./howto-manage-firewall-using-portal.md)
* [Erstellen und Verwalten von Firewallregeln für Azure Database for MySQL mithilfe der Azure CLI](./howto-manage-firewall-using-cli.md)
* [Herstellen einer Verbindung mit Azure Database for MySQL mit Umleitung](./howto-redirection.md)