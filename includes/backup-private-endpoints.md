---
author: dcurwin
ms.service: backup
ms.topic: include
ms.date: 03/12/2020
ms.author: dacurwin
ms.openlocfilehash: b8941bb4fa77cb0008cb7271681e972bc21d6588
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83659541"
---
Sie können nun [private Endpunkte](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) verwenden, um Ihre Daten von Servern in einem virtuellen Netzwerk sicher in Ihrem Recovery Services-Tresor zu sichern. Der private Endpunkt verwendet eine IP-Adresse aus dem VNET-Adressraum für Ihren Tresor. Der Netzwerkdatenverkehr zwischen Ihren Ressourcen innerhalb des virtuellen Netzwerks und dem Tresor wird über das virtuelle Netzwerk und eine private Verbindung im Microsoft-Backbone-Netzwerk übertragen. Dadurch wird er vom öffentlichen Internet isoliert. Private Endpunkte können zum Sichern und Wiederherstellen von SQL- und SAP HANA-Datenbanken verwendet werden, die auf Ihren virtuellen Azure-Computern ausgeführt werden. Sie können auch für lokale Server mit dem MARS-Agent verwendet werden.

Die Azure-VM-Sicherung erfordert keine Internetkonnektivität, daher ist es nicht erforderlich, dass private Endpunkte die Netzwerkisolation zulassen.

Weitere Informationen zu privaten Endpunkten für Azure Backup finden Sie [hier](https://docs.microsoft.com/azure/backup/private-endpoints).
