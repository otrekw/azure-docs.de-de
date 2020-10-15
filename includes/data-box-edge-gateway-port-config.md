---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 71460af42b4da97a578ae5a3e23a714577e71867
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86218392"
---
| Port-Nr.| ein oder aus | Portbereich| Erforderlich|   Notizen |
|--------|-----|-----|-----------|----------|
| TCP 80 (HTTP)|aus|WAN |Nein|Der ausgehende Port wird für den Internetzugriff zum Abrufen von Updates verwendet. <br>Der ausgehende Webproxy kann vom Benutzer konfiguriert werden. |
| TCP 443 (HTTPS)|aus|WAN|Ja|Der ausgehende Port wird für den Zugriff auf Daten in der Cloud verwendet.<br>Der ausgehende Webproxy kann vom Benutzer konfiguriert werden.|
| UDP 123 (NTP)|aus|WAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist nur dann erforderlich, wenn Sie einen internetbasierten NTP-Server verwenden.  |   
| UDP 53 (DNS)|aus|WAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist nur dann erforderlich, wenn Sie einen internetbasierten DNS-Server verwenden.<br>Es wird empfohlen, den lokalen DNS-Server zu verwenden. |
| TCP 5985 (WinRM)|Aus/Ein|LAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist erforderlich, um eine Verbindung mit dem Gerät per Remote-PowerShell über HTTP herzustellen.  |
| UDP 67 (DHCP)|aus|LAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist nur dann erforderlich, wenn Sie einen lokalen DHCP-Server verwenden.  |
| TCP 80 (HTTP)|Aus/Ein|LAN|Ja|Dies ist der eingehende Port für die lokale Benutzeroberfläche auf dem Gerät für die lokale Verwaltung. <br>Beim Zugriff auf die lokale Benutzeroberfläche über HTTP erfolgt automatisch eine Umleitung auf HTTPS.  |
| TCP 443 (HTTPS)|Aus/Ein|LAN|Ja|Dies ist der eingehende Port für die lokale Benutzeroberfläche auf dem Gerät für die lokale Verwaltung. |
| TCP 445 (SMB)|Geben Sie in|LAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist nur dann erforderlich, wenn Sie eine Verbindung über SMB herstellen. |
| TCP 2049 (NFS)|Geben Sie in|LAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist nur dann erforderlich, wenn Sie eine Verbindung über NFS herstellen. |
