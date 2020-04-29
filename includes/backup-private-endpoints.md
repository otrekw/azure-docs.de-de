---
author: dcurwin
ms.service: backup
ms.topic: include
ms.date: 03/12/2020
ms.author: dacurwin
ms.openlocfilehash: 024f70a21b36bffa662ba6f568adc8efbd0a909d
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81539599"
---
Sie können nun [private Endpunkte](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) verwenden, um Ihre Daten von Servern in einem virtuellen Netzwerk sicher in Ihrem Recovery Services-Tresor zu sichern. Der private Endpunkt verwendet eine IP-Adresse aus dem VNET-Adressraum für Ihren Tresor. Der Netzwerkdatenverkehr zwischen Ihren Ressourcen innerhalb des virtuellen Netzwerks und dem Tresor wird über das virtuelle Netzwerk und eine private Verbindung im Microsoft-Backbone-Netzwerk übertragen. Dadurch wird er vom öffentlichen Internet isoliert. Private Endpunkte können zum Sichern und Wiederherstellen von SQL- und SAP HANA-Datenbanken verwendet werden, die auf Ihren virtuellen Azure-Computern ausgeführt werden. Sie können auch für lokale Server mit dem MARS-Agent verwendet werden.

Die Azure-VM-Sicherung erfordert keine Internetkonnektivität, daher ist es nicht erforderlich, dass private Endpunkte die Netzwerkisolation zulassen.

>[!NOTE]
> Diese Funktion befindet sich derzeit in eingeschränkter Verfügbarkeit und ist in den Regionen „USA, Westen-Mitte“, „USA, Süden-Mitte“, „USA, Westen 2“ und „USA, Osten“ verfügbar (Verfügbarkeit in weiteren Azure-Regionen folgt). Füllen Sie [diese Umfrage](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapUQk5EQ1QxRzVOWDNDS1Y1Q0xLTkdLQ0U0RC4u) aus, und senden Sie uns eine E-Mail an azbackupnetsec@microsoft.com, wenn Sie an der Verwendung privater Endpunkte für Azure Backup interessiert sind. Die Möglichkeit zur Verwendung dieses Features unterliegt der Genehmigung des Azure Backup-Diensts.
